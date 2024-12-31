---
layout: post
title: "[MuleSoft] Message Flow를 만들어보자"
description: MuleSoft의 Message Flow에 대해 알아보고 구현해보자
date: 2024-11-18T11:05:00 +09:00
categories: develop
keyword:
  - MuleSoft
  - MessageFlow
tags:
  - Back-End
  - MuleSoft
img-tag: mulesoft
---
## 개요

### 목표

* MuleSoft의 Message Flow에 대해 알아보고 구현해보자

## 목차

### 1. Message Flow의 정의

### 2. Message Flow 종류

### 3. MariaDB 연결

### 4. Message Flow 구현

- - -

## 1. Message Flow의 정의

Message Flow는 메시지가 source에서 DataWeave translation map을 거쳐 대상까지 이동하는 경로입니다.

시스템과 파트너 간에 메시지를 보내고 받기 위해 메시지 흐름을 설정합니다.

*파트너: 외부 시스템을 의미합니다.*

Message Flow는 엔드포인트, 메시지 유형, 식별자, 매핑과 같은 객체로 구성됩니다. 

소스에서 타겟으로 비즈니스 트랜잭션 처리를 조정합니다.

![](/assets/img/message-flow.png)

## 2. Message Flow 종류

### Inbound

인바운드 메시지 흐름은 파트너로부터 메시지를 수신하고 

이를 내부 애플리케이션 메시지 형식으로 변환한 다음 변환된 메시지를 백엔드 시스템으로 전송합니다.

### Outbound

아웃바운드 메시지 흐름은 백엔드 애플리케이션에서 메시지를 수신하고

해당 메시지를 파트너의 메시지 형식으로 변환한 다음, EDI 메시지를 파트너에게 전송합니다.

## 3. MariaDB 연결

Message Flow 구현에 있어 Listener, Select, Transform Message가 기본적으로 필요하다.

Select 컴포넌트에서 사용하는 MariaDB 연결은 MuleSoft에서 기본적으로 제공하지 않음

고로 커스텀 연결 Config를 생성하여 구현해야 함

### Listener

Message Flow의 호출을 정의하는 컴포넌트 

### Select

Database에서 조회를 하는 컴포넌트

### Transform Message

DataWeave 형식을 사용하여 Data를 변환하는 컴포넌트

아래는 데이터베이스 설정창이다. 

![](/assets/img/mariadb_connect.png)

Mariadb Driver를 설치하기 위해

Cofigure 버튼을 눌러 Add Maven Dependency를 클릭한다.

![](/assets/img/add_maven_dependency.png)

```
<dependency>
	<groupId>org.mariadb.jdbc</groupId>
	<artifactId>mariadb-java-client</artifactId>
	<version>3.2.0</version>
</dependency>	
```

해당 mariaDB Dependency를 입력하고 Finish 버튼을 눌러 프로젝트에 설치를 합니다.

중요한 것은 

URL 형식이 jdbc:mariadb:// 이고
Driver class name인 org.mariadb.jdbc.Driver 인 것이다.
