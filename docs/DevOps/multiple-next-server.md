---
layout: default
title: 다중 Next서버 배포용 도커파일
parent: DevOps
nav_order: 1
---

# 다중 Next.js (v12) 서버 배포를 위한 Dockerfile

하나의 젠킨스에서 여러 서버를 배포하고 있었는데 사용하는 노드 버전이나 노드 모듈의 버전이 상이하여
이를 해결하고 독립된 빌드 환경을 구축하기 위해 도커와 **Turborepo**를 활용하였습니다.

## 주요 고려 사항
- **무중단 배포**: AWS Auto Scaling을 활용한 무중단 배포 전, 후 서버의 빌드 아이디가 달라 문제가 생길 수 있다. 따라서 해당 빌드 산출물을 적절히 관리해야 합니다. 도커로 빌드한 파일을 share하여 관리 할 수 있습니다. 빌드 아이디를 고정으로 쓸 수도 있지만 사이트 캐싱에도 해당 아이디가 사용되어 수정사항이 제대로 반영되지 않는 문제가 발생할 수 있으니 주의해야 합니다.
- **Turborepo 활용**: 모노레포 환경에서 특정 프로젝트만 추출(`prune`)하여 빌드합니다.
- **참고**: [Turborepo Docker 예제](https://github.com/vercel/turborepo/tree/main/examples/with-docker)

## 전체 Dockerfile 예시
```docker
# 1. Builder 단계
FROM node:16-alpine AS builder
RUN apk add --no-cache libc6-compat
RUN apk update
WORKDIR /app
RUN yarn global add turbo@1.4.3
COPY . .
RUN turbo prune --scope={project} --docker

# 2. Installer 단계
FROM node:16-alpine AS installer
RUN apk add --no-cache libc6-compat
RUN apk update
WORKDIR /app
ARG TARGET_SERVER
COPY --from=builder /app/out/json/ .
COPY --from=builder /app/out/yarn.lock ./yarn.lock
COPY --from=builder /app/out/full/ .
# 환경별 .env 설정
COPY ./apps/{project}/env/.env.${TARGET_SERVER} /app/apps/{project}/.env
COPY .gitignore .gitignore
COPY turbo.json turbo.json

RUN yarn install
RUN yarn turbo run build --filter={project}

# 3. Runner 단계
FROM node:16-alpine AS runner
WORKDIR /app
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs
USER nextjs

COPY --from=installer /app/apps/{project}/next.config.js .
COPY --from=installer /app/apps/{project}/package.json .
COPY --from=installer /app/apps/{project}/public ./apps/{project}/public

# Standalone 출력물 복사 및 정적 파일 경로 재구성
COPY --from=installer --chown=nextjs:nodejs /app/apps/{project}/.next/standalone ./
COPY --from=installer --chown=nextjs:nodejs /app/apps/{project}/.next/static ./apps/{project}/public/{project}/_next/static

ENV PORT=3000
EXPOSE 3000

CMD ["node", "apps/{project}/server.js"]
```

---

## 단계별 상세 설명

### 1. Builder
- 베이스 이미지 지정 및 `turbo` 전역 설치.
- `turbo prune --scope={project}` 명령어를 통해 특정 프로젝트와 그 의존성만 추출합니다.

### 2. Installer
- builder 컨테이너에서 필요한 파일을 옮긴 후 `yarn install`을 진행하고 빌드를 수행합니다.
- `/out/full` 폴더는 터포레포의 prune 명령어를 진행하면 생성되는 구조입니다.
- 빌드 타겟 서버(프로젝트)에 맞는 `.env` 파일을 복사합니다.

### 3. Runner & 정적 파일 처리
- 완성된 파일들을 재구성하고 최종 실행 환경을 구축합니다.
- **정적 파일(`static`) 경로 문제**: 다중 서버 운용 시 모든 서버의 정적 파일 경로가 `{도메인}/_next/static`으로 겹치는 문제를 방지하기 위해, `public/{prefix}/_next/static` 형태로 경로를 재구성하여 호스팅합니다. (S3에 올려서 관리하는 방식도 추천)

### 4. Standalone 모드 활용
Next.js 배포 시 용량을 최소화하기 위해 `standalone` 옵션을 사용합니다.  
실행 시 `next start` 대신 `node` 명령어를 직접 사용합니다.

#### `next.config.js` 설정
experimental는 turborepo특성상 install을 하면 노드모듈이 프로젝트폴더 상위에 생성되기때문에 추가합니다.
```javascript
const path = require('path')

module.exports = {
  output: 'standalone',
  experimental: {
    // Turborepo 특성을 고려한 경로 설정
    outputFileTracingRoot: path.join(__dirname, '../../'),
  },
}
```

#### 필수 모듈 설치
Node 명령어로 직접 실행하기 위해 다음 모듈들이 필요합니다.
```bash
yarn add sharp prop-types
```