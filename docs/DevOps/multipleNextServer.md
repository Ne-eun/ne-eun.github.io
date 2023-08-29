---
layout: default
title: 다중 Next서버 배포용 도커파일
parent: DevOps
nav_order: 1
---

# 다중 nextjs(12버전)서버 배포를 위한 도커파일
다중 서버를 하나의 젠킨스에서 배포하다보니 각각 노드버전이나 사용하는 노드모듈의 버전이 상이할 수 있어   
독립된 빌드 환경을 만들고자 도커를 활용하였다.   
모노레포 빌드 툴로는 terborepo를 사용했다.   
AWS의 오토스케일링을 활용한 무중단 배포에서는 배포 전, 후 서버의 빌드 아이디가 달라 문제가 생길 수 있다.   
도커로 구성하면 빌드된 파일을 share 해야한다.   
빌드아이디를 고정으로 쓸 수도 있지만 사이트 캐싱에도 사용되어 수정사항이 제대로 반영되지 않을 수 있다.   
Dockerfile 초안은 turborepo 예제를 참고하여 만들었다.   
https://github.com/vercel/turborepo/tree/main/examples/with-docker

```docker
FROM node:16-alpine AS builder
RUN apk add --no-cache libc6-compat
RUN apk update
# Set working directory
WORKDIR /app
RUN yarn global add turbo@1.4.3
COPY . .
RUN turbo prune --scope=activity --docker

FROM node:16-alpine AS installer
RUN apk add --no-cache libc6-compat
RUN apk update
WORKDIR /app
ARG TARGET_SERVER
COPY --from=builder /app/out/json/ .
COPY --from=builder /app/out/yarn.lock ./yarn.lock
COPY --from=builder /app/out/full/ .
# env폴더 안에있는 target env파일을 빌드환경 .env로 이동
COPY ./apps/activity/env/.env.${TARGET_SERVER} /app/apps/activity/.env
COPY .gitignore .gitignore
COPY turbo.json turbo.json

RUN yarn install
RUN yarn turbo run build --filter=activity

FROM node:16-alpine AS runner
WORKDIR /app
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs
USER nextjs

COPY --from=installer /app/apps/activity/next.config.js .
COPY --from=installer /app/apps/activity/package.json .
COPY --from=installer /app/apps/activity/public ./apps/activity/public

# Automatically leverage output traces to reduce image size 
# https://nextjs.org/docs/advanced-features/output-file-tracing
COPY --from=installer --chown=nextjs:nodejs /app/apps/activity/.next/standalone ./
COPY --from=installer --chown=nextjs:nodejs /app/apps/activity/.next/static ./apps/activity/public/activity/_next/static
ENV PORT=3000
EXPOSE 3000

CMD node apps/activity/server.js
```
여러개의 서버 중 activity(단지 이름임)의 결과물이다.
하나씩 정리해보자

해당 도커파일은 각각의 터보레포 apps/activity root에 넣어주었다.   

## builder
```docker
FROM node:16-alpine AS builder
RUN apk add --no-cache libc6-compat
RUN apk update
# Set working directory
WORKDIR /app
RUN yarn global add turbo@1.4.3
COPY . .
RUN turbo prune --scope=activity --docker
```
먼저 base가 되는 node 버전을 명시하고 turbo를 설치해줬다.   
그 후 소스코드를 working directory에 옮긴 후 activity 프로젝트만 따로 추출했다.   
(이건 터보레포의 빌드 옵션)

## installer
```docker
FROM node:16-alpine AS installer
RUN apk add --no-cache libc6-compat
RUN apk update
WORKDIR /app
ARG TARGET_SERVER
COPY --from=builder /app/out/json/ .
COPY --from=builder /app/out/yarn.lock ./yarn.lock
COPY --from=builder /app/out/full/ .
# env폴더 안에있는 target env파일을 빌드환경 .env로 이동
COPY ./apps/activity/env/.env.${TARGET_SERVER} /app/apps/activity/.env
COPY .gitignore .gitignore
COPY turbo.json turbo.json

RUN yarn install
RUN yarn turbo run build --filter=activity
```

그 다음 builder 컨테이너에서 필요한 파일만 옮기고 빌드했다.   
/out/full 폴더는 터보레포의 prune 명령어를 진행하면 떨어지는 폴더 구조이다.   

## runner
```docker
FROM node:16-alpine AS runner
WORKDIR /app
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs
USER nextjs

COPY --from=installer /app/apps/activity/next.config.js .
COPY --from=installer /app/apps/activity/package.json .
COPY --from=installer /app/apps/activity/public ./apps/activity/public

COPY --from=installer --chown=nextjs:nodejs /app/apps/activity/.next/standalone ./
COPY --from=installer --chown=nextjs:nodejs /app/apps/activity/.next/static ./apps/activity/public/activity/_next/static
ENV PORT=3000
EXPOSE 3000

CMD node apps/activity/server.js
```
runner단계에서는 빌더단계에서 완성된 파일들을 재구성하고 최종 이미지를 만들었다.   
/app/apps/activity/.next/static에 있는 파일을 public/activity/_next/static로 옮긴 이유는   
next의 경우 모든 css, image, js등 페이지 구성에 필요한 static파일의 경로가   
{도메인주소}/_next/static로 고정되어 있는데 하나의 도메인에서 다중 서버를 운용할 경우   
어떤 서버로의 요청인지 구분하지 못해 적합한 서버의 데이터를 불러오지 못하는 에러가 발생한다.   
따라서 요청을 구분하기위해 prefix를 달아 nextjs의 정적 데이터를 호스팅하는 public폴더로 이동했다.   
(s3를 활용해도 좋다.)


## standalone
next를 배포할때는 standalone옵션을 넣어주길 권장하는데 이는 실행에 필요한 파일만 뽑아 최소한의 용량으로 만들어준다.   
따라서 실행 또한 next start가 아닌 node 명령어로 실행한다.   
```js
const path = require('path')
module.exports = withTM({
  output: 'standalone',
  experimental: {
    outputFileTracingRoot: path.join(__dirname, '../../'),
  },
})
```
stadalone로 빌드하려면 next.config.js에 이 두가지 옵션을 추가한다.   
experimental는 turborepo특성상 install을 하면 노드모듈이 프로젝트폴더 상위에 생성되기때문에 추가하였다.
node명령어로 실행하기 위해 두가지 모듈도 필요하므로 설치해주자
```cmd
yarn add sharp
yarn add prop-types
```