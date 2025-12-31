---
layout: default
title: Nexus & S3 Blob Store
parent: DevOps
nav_order: 3
---

# Nexus & S3 Blob Store


> 이미지 데이터를 저장하는 저장소를 S3로 설정하려면 **Blob 스토어**와 **Repository**를 모두 생성해야 하며, 기본 저장소를 사용하려면 **Repository**만 생성하면 됩니다. S3에 업로드된 이미지는 Blob 데이터로 저장되며, S3에서 직접 이미지를 Pull 할 수는 없습니다.

## 1. Blob 스토어 만들기

Nexus 설정 페이지에서 **Create blob store**를 클릭하여 생성합니다.

![]({{site.url}}/assets/imgs/blob.png)

| 설정 항목 | 입력 값 |
| :--- | :--- |
| **Type** | S3 |
| **Name** | 원하는 스토어명 |
| **Bucket** | S3 버킷명 |
| **Prefix** | 하위 폴더 경로명 |
| **Access Key ID** | AWS Access Key |
| **Secret Access Key** | AWS Secret Key |

*그 외 설정값은 기본값 혹은 필요에 따라 조정합니다.*

## 2. Repository 만들기

Nexus 설정 페이지에서 **Create repository**를 클릭하고 **docker (hosted)** 타입을 선택합니다.

![]({{site.url}}/assets/imgs/repo.png)

### 주요 설정 사항
- **Name**: 원하는 레파지토리 이름
- **Format**: `docker`
- **Type**: `hosted`

#### Repository Connectors
- **HTTP**: 사용할 포트 번호 지정 (예: 30000)

#### Docker Registry API Support
- **Enable Docker V1 API**: 체크

#### Storage
- **Blob store**: 위에서 생성한 S3 Blob 스토어 선택 (없으면 `default`)
- **Strict Content Type Validation**: 체크

## 3. 넥서스 서버 재실행

새로운 도커 레지스트리 포트를 외부에서 사용하려면 서버 재실행이 필요합니다. 넥서스가 도커로 실행 중인 경우 다음 명령어를 사용합니다.

```bash
# 1. 실행 중인 컨테이너 확인
docker ps

# 2. 특정 컨테이너 중지
docker stop {container_id}

# 3. 신규 포트(예: 30000)를 포함하여 재실행
docker run -d -p 80:8081 -p 30000:30000 \
  -v /data/nexus-data:/nexus-data \
  sonatype/nexus3
```
*(예시에서는 30000 포트를 추가로 개방하였습니다.)*