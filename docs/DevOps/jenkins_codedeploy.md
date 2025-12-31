---
layout: default
title: Jenkins & CodeDeploy
parent: DevOps
nav_order: 4
---

# Jenkins & CodeDeploy


## 배포 프로세스 요약
**도커 이미지 빌드** → **Nexus Docker Registry 전송** → **AWS CodeDeploy 배포 생성**

### 젠킨스 파이프라인 스크립트
```bash
echo "current Branch ${GITLAB_BRANCH_TAG}" # 현재 브랜치 콘솔 출력
echo "${DOCKER_IMAGE_NAME}" # 도커 이미지 이름 출력

# 1. 도커 이미지 빌드
docker build -t ${DOCKER_BASE_URL}/${DOCKER_IMAGE_NAME} -f apps/${PROJECT_TARGET}/Dockerfile .

echo "---------- Image Upload -------------"
# 2. 도커 이미지 Nexus로 Push
docker push ${DOCKER_BASE_URL}/${DOCKER_IMAGE_NAME}

# 3. 미사용(Dangling) 도커 이미지 제거
docker rmi $(docker images -f "dangling=true" -q)

echo "---------- Zipping scripts for CodeDeploy -------------"
# 4. 기존 작업 폴더 정리 및 신규 생성
rm -rf ${DOCKER_IMAGE_NAME}
mkdir -p ${DOCKER_IMAGE_NAME}

# 5. CodeDeploy 실행 스크립트 및 설정 복사
cp ./appspec.yml ${DOCKER_IMAGE_NAME}/
cp ./scripts/stop_server.sh ${DOCKER_IMAGE_NAME}/
cp ./scripts/start_server.sh ${DOCKER_IMAGE_NAME}/

# 6. start_server.sh에 실행 명령어 추가
echo "sudo docker pull ${DOCKER_BASE_URL}/${DOCKER_IMAGE_NAME}" >> ${DOCKER_IMAGE_NAME}/start_server.sh
echo "sudo docker run -p 80:3000 -d --restart='always' ${DOCKER_BASE_URL}/${DOCKER_IMAGE_NAME}" >> ${DOCKER_IMAGE_NAME}/start_server.sh

# 7. 배포 파일 압축
cd ${DOCKER_IMAGE_NAME}
zip -r ${DOCKER_IMAGE_NAME}.zip ./*

echo "---------- Upload to S3 -------------"
# 8. S3에 ZIP 파일 업로드
aws s3 cp ${DOCKER_IMAGE_NAME}.zip s3://waug-code-deploy/waug_web_v2/${DOCKER_IMAGE_NAME}.zip

echo "---------- Start CodeDeploy -------------"
# 9. AWS CodeDeploy 배포 생성
aws deploy create-deployment \
  --application-name waug_web_v2 \
  --deployment-group-name ${DOCKER_IMAGE_NAME} \
  --ignore-application-stop-failures \
  --auto-rollback-configuration enabled=true,events=DEPLOYMENT_FAILURE \
  --region ap-northeast-2 \
  --s3-location bucket=waug-code-deploy,bundleType=zip,key=waug_web_v2/${DOCKER_IMAGE_NAME}.zip
```

## 트러블슈팅

### 1. HTTP/HTTPS 이슈로 Nexus 접근 불가 시
Jenkins가 설치된 EC2 인스턴스의 도커 설정을 수정해야 합니다.
- **파일 경로**: `/etc/docker/daemon.json` (파일이 없으면 생성)
- **설정 내용**:
  ```json
  {
    "insecure-registries" : ["이미지 저장소 주소"]
  }
  ```

### 2. 권한 문제로 이미지 Push 실패 시
Jenkins EC2 인스턴스에서 직접 로그인을 수행합니다.
```bash
docker login ${Nexus 주소}
```
