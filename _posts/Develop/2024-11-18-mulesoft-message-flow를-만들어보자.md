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

## 4. Message Flow 구현

해당 글에서 거시적으로 접근하고자 한다.

아래 그림이 구현하고자 하는 .플로우이다.

해당 플로우에 대한 정보는 다음과 같다.

sourceDB의 데이터베이스이름과 테이블 이름으로 해당 테이블의 메타데이터를 조회하고

해당 테이블의 primary key column에 대한 메타데이터를 조회하는 flow이다.

해당 플로우에서는 3개의 변수를 사용한다.

1. 데이터베이스 이름
2. 테이블 이름
3. sourceDB에서 쿼리 결과값

![](/assets/img/custom_flow.png)

아래에는 Query Metadata from sourceDB의 SQL과 input parameters이다.

MuleSoft에서 input parameter 변수 사용 시, SQL에서는 :변수이름으로 사용하고 

input parameter에는 json객체로 선언하고 키는 사용할 변수명, 값에는 사용할 값을 기입한다. 

flow 내 선언한 변수를 사용할 경우 vars.선언한 변수 이름으로 접근 가능하다.

```
// SQL

SELECT 
    COLUMN_NAME,         
    DATA_TYPE,         
    CHARACTER_MAXIMUM_LENGTH AS MAX_LENGTH, 
    IS_NULLABLE,         
    COLUMN_KEY,           
    COLUMN_DEFAULT,      
    EXTRA                
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = :database_name  
  AND TABLE_NAME = :table_name
  AND COLUMN_KEY = 'PRI'; 

// input-parameters  

{	
  database_name: vars.database_name,
  table_name: vars.table_name
}
```

위와 같은 방법으로 flow를 구현하고 이를 테스트 한 결과는 다음과 같다.



![](/assets/img/test_result.png)


테스트 결과, 설정한 변수로 지정된 데이터베이스명과 테이블명에 따라, 기본 키 컬럼의 메타데이터를 조회할 수 있음을 확인하였다.
