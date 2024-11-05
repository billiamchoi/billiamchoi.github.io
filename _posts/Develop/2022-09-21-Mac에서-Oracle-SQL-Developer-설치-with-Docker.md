---
layout: post
title: Mac에서 Oracle SQL Developer 설치 with Docker
description: Mac에서 Oracle SQL Developer 설치를 Docker를 사용해서 해보자
date: 2022-09-21T16:47:00 +09:00
categories: develop
keyword:
  - oracle
tags:
  - MacOS
  - docker
  - Oracle
img-tag: study
---

오늘은 mac OS에서 Oracle SQL Developer를 설치하는 방법에 대해 알아보자.

순서는 다음과 같다.

**1. JDK 8버전 설치**\
**2. Docker 설치**\
**3. docker-oracle-xe-11g docker image pull**\
**4. Oracle SQL Developer 설치**\
**5. Docker Container 구동 및 SQL Developer 초기 설정**

## **1. JDK 8버전 설치**

jdk 8버전이 없는 경우

아래 링크에서 JDK 8 버전 설치

http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

## **2. Docker 설치**

아래 링크에 가서 운영체제에 맞게 Docker를 설치한다

https://docs.docker.com/desktop/install/mac-install/

설치 후 도커를 열어서 실행

도커 열어서 키고 아래와 같이

Docker Desktop is running 문구가 뜨면 docker는 세팅 완료!

![](/assets/img/58e0e22d-2841-4fd5-974e-eeb0f6a8b0e6-image.png)

## **3. docker-oracle-xe-11g docker image pull**

터미널에 아래 코드를 입력하여 docker hub에서 docker-oracle-xe-11g image를 받자

```
$ docker pull deepdiver/docker-oracle-xe-11g
```

## **4. Oracle SQL Developer 설치**

아래 링크에서 SQL Developer 22.2.1를 설치하자

https://www.oracle.com/database/sqldeveloper/technologies/download/

## **5. Docker Container 구동 및 SQL Developer 초기 설정**

아래 코드를 터미널에 입력하여 docker-oracle-xe-11g image로 docker container 구동하자

```
docker run -d -p 59160:22 -p 59161:1521 deepdiver/docker-oracle-xe-11g
```

SQL Developer를 실행하고 좌측 상단에 + 버튼을 눌러 새로운 connection을 만들자

![](/assets/img/e7a60ab4-7032-45f3-a564-c6758e92c812-image.png)

connection 이름은 편한대로 하고 필자는 localhost로 했다.

사용자 이름은 system

비밀번호 oracle

그리고 세부정로 탭에

port는 59161

- 도커 컨테이너의 1521 포트를 로컬 호스트 49161 포트로 포트 포워딩 했기 때문

SID는 xe로 설정

![](/assets/img/f06a1624-7d58-4843-a72c-29e2bba8a9ea-image.png)

그리고 접속을 누르고 아래와 같이 아까 설정한 connection 이름의 워크 시트가 생기면 설정 완료.

![](/assets/img/be2a83e4-e3a6-4fce-90f5-ebcfa8279d69-image.png)

수고하셨습니다.
