---
layout: post
title: Gitlab CI CD시 npm install에서 멈춰있는 현상 해결법
description: Gitlab CI CD시 npm install에서 멈춰있는 현상 해결법에 대해 다룹니다.
date: 2024-11-11T13:07:00 +09:00
categories: bugs
keyword:
  - docker
tags:
  - docker
img-tag: github
---
# 목차

### 1. 문제점

### 2. 원인 분석 및 결과

### 3. 문제 해결

- - -

## 1. 문제점

어느날 갑지가 원래 돌아가던 Gitlab CICD pipeline이 정상적으로 동작하지 않음

npm install -g @sap/cds-dk에서 계속 멈춰 있어 개발서버가 정상적으로 배포되지 않음

## 2. 원인 분석 및 결과

문제점을 해결하기 위해 아래와 같이 기본 run 명령어에 --verbose 옵션을 추가하였다.

```
RUN npm install -global @sap/cds-dk --verbose
```

확인 결과, 패키지 설치에서 멈춰 있는 현상을 발견하였다.

그리고 10분 정도 대기해보니 아래와 같은 로그로 원인을 유추 할 수 있었다.

![](/assets/img/docker_log.png)

원인은 node 버전이 안 맞는 것이다.

고로 npm install이 정상적으로 실행되지 않았다.

## 3. 문제 해결

어디서 docker 컨테이너 내 node 버전을 설정해주는 지를 찾아야 된다.

.gitlab-ci.yml에서 빌드 시 사용하는 변수를 설정하는 부분이 있습니다.

```yml
// .gitlab-ci.yml
variables:
  DOCKER_BUILDKIT: 1 # BuildKit 활성화
  IMAGE_NAME: $NEXUS3_PRIVATE_HOST/$APP_NAME:$APP_VERSION
  BUILD_ARG_NODE_IMAGE: --build-arg node_image=$ARG_NODE_IMAGE
  BUILD_ARG_EXPOSE_PORT: --build-arg expose_port=$ARG_EXPOSE_PORT
  BUILD_ARGS: $BUILD_ARG_NODE_IMAGE $BUILD_ARG_EXPOSE_PORT

```

\--build-arg를 사용하여 변수 node_image 설정합니다.

변수 $ARG_NODE_IMAGE는 gitlab Settings > CI/CID > Variables에서 설정합니다.

\--build-arg로 node_image 변수를 build 변수로 설정하였기 때문에 아래 Dockerfile에서 

ARG node_image로 Dockerfile 내 사용이 가능합니다.

```
ARG node_image
ARG expose_port
FROM ${node_image}

```

변수 $ARG_NODE_IMAGE 값을 확인해보니, node:latest로 되어 있었습니다.

latest 태그로 배포하는 행위는 절대 하지 말아야된다는 교훈을 얻습니다.

**node:latest를 node:20.15.0으로 변경하였습니다.**

**노드 버전을 fix하여 문제를 해결했습니다.**
