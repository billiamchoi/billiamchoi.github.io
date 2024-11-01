---
layout: post
title: "Multi-tenant architecture (ft. Salesforce)"
description: "Multi-tenant architecture란 무엇인가"
date: 2023-02-07T16:47:00 +09:00
categories: study
keyword:
  - multi-tenant-architecture
tags: ["CRM", "Salesforce"]
img-tag: study
---

## 1. Multi-tenant architecture란 무엇인가

- 멀티테넌시는 소프트웨어 애플리케이션의 단일 인스턴스가 여러 고객에게 서비스를 제공하는 아키텍처임

### 1.1 Multi-tenant architecture의 특징

- 테넌트는 사용자 인터페이스의 색상 또는 비즈니스 규칙과 같은 애플리케이션의 일부를 사용자 정의할 수 있는 기능을 제공받음

- 테넌트는 애플리케이션의 코드를 사용자 정의할 수는 없음

- Multi-tenant architecture는 애플리케이션의 여러 인스턴스가 공유 환경에서 작동함

- 이 아키텍처는 각 테넌트가 물리적으로 통합되어 있지만 논리적으로 분리되어 있기 때문에 작동할 수 있음

- 소프트웨어의 단일 인스턴스가 한 서버에서 실행된 다음 여러 테넌트에 서비스를 제공함

- 이러한 방식으로 다중 테넌트 아키텍처의 소프트웨어 애플리케이션은 구성, 데이터, 사용자 관리 및 기타 속성의 전용 인스턴스를 공유할 수 있음

- 멀티테넌시 애플리케이션은 동일한 사용자, 디스플레이, 규칙 및 데이터베이스 스키마를 공유할 수 있음

- 사용자는 범위 및 데이터베이스 스키마에 대한 규칙을 사용자 정의할 수 있음.

### 1.2 Multi-tenant architecture의 유형

![](/assets/img/4c7bd735-76a1-4698-9092-87de231129ab-image.png)

**a. Single application, single database**

- 세 가지 모델 유형 중 가장 단순한 형태임.

- 공유 자원을 사용하기 때문에 테넌트 입장에서 상대적으로 비용이 적게 듬.

- 이 형식은 단일 애플리케이션 및 데이터베이스 인스턴스를 사용하여 여러 동시 테넌트를 호스트하고 데이터를 저장함.

- 단일 공유 데이터베이스 스키마를 사용하면 더 쉽게 확장할 수 있음.

- 그러나 운영 비용이 더 높을 수 있고 노이즈 이웃 효과가 잠재적으로 성능에 영향을 미칠 수 있음.

**b. Single application, multiple database**

- 이것은 여러 스키마가 있는 단일 데이터베이스 사용이 포함됩니다. 이 테넌트 시스템은 각 테넌트에 대한 개별 데이터베이스가 있는 단일 애플리케이션 인스턴스를 사용함.

- 이 아키텍처는 확장하기가 더 어려우며 각 데이터베이스에 더 많은 비용과 오버헤드가 있음.

- 서로 다른 테넌트의 데이터를 서로 다르게 처리해야 하는 경우
  ex) 지역별로 서로 다른 규정을 통과해야 하는 경우
- 별도의 데이터베이스는 잠재적인 노이즈 이웃 효과를 줄이는 데도 도움이 됨.

**c. Multiple application, multiple database**

- 이 유형은 비용, 관리 및 유지 관리 측면에서 상대적으로 복잡하지만 접근 방식이 안전하고 선택한 기준으로 테넌트를 구분할 수 있음.

## 2. Salesforce에서 Multi-tenant architecture 구현

![](/assets/img/094b5e2a-d302-48a4-b1ec-3c2c6334de7a-image.png)

세일즈 포스에서는 **확장 가능한 공유 서비스** (Shared Scalable Services)를 제공합니다.

- 2개의 데이터센터에서 4개의 고객 데이터 복사본을 관리함
- 75개 이상의 운영 인스턴스를 가동함

### 2.1 Salesforce Multi-tenant architecture 핵심 포인트

- 고유한 **OrgID**(식별자)를 사용하여 고객의 데이터가 안전하게 분리되어 있음

- 모든 데이터 접근 작업에는 **OrgID**가 포함이 됨

_이를 도식화 하면 위 그림과 같음_

### 2.2 주요 아키텍처 원칙

1. Stateless Appservers

2. No Database Definition Language (DDL) at Runtime

3. All tables partitioned by OrgID

4. Smart Primary Key, Polymorphic Foreign keys

5. Creative de-normalization and pivoting

6. Use every RDBMS feature & optimization

### 2.3 Salesforce Data structure

![](/assets/img/43a76c8b-16b7-487c-b7d9-5976758d6295-image.png)

**메타데이터, 데이터 및 피벗 테이블** 구조는 가상 데이터 구조에 해당하는 데이터를 저장함

### 2.4 성능을 위한 Data Partitioning

- OrgId로 분할된 테이블 해시

- 별도의 연결 풀이 물리적 호스트를 가리킴

- 앱 계층도 OrgId로 동적으로 분할됨

- 트랜잭션 무효화를 포함한 분산 메타데이터 캐시

### 2.5 Multitenant Query Optimizer

![](/assets/img/e0769d10-d717-4ff7-a475-95e4e5d9e878-image.png)

#### Step by Step

1. 원천 질의 결과는 API이나 global search를 통해 나옴

2. Query 전에 user visibility와 filter selectivity를 확인

   - user visibility

     - 사용자가 접근할 수 있는 row의 수

   - filter selectivity
     - filter column에 대응하는 index

3. 2번을 토대로 동적인 query를 작성

4. 가장 최적화된 query를 사용하면 최적의 결과를 도출

### 2.6 Core CRM Site 설계

![](/assets/img/cba2eb94-8774-4150-bd52-3f3b9de2bfa6-image.png)

Salesforce Core CRM은 4개의 온라인 데이터 사본을 사용한 중복 사이트 로 구성됨

**Production cluster capacity**

- 11+1 (active + spare) 노드로 구성

- 2개의 노드 클러스터로 구성된 Active Data Guard가 빠른 데이터 복원을 위해 항시 대기중

**Data Guard to remote site**

- 원격 복제를 강화함

- 15분 이내 빠른 site switing이 가능함

- planned 및 unplanned maintenance windows 단축

### 2.7 instance이란 무엇인가?

![](/assets/img/4f9ce30a-3b5f-4878-8acd-d3000ad7f064-image.png)

인스턴스는 위의 그림(Shared Database부터 Virtual Application Componets)을 다 포함하는 단위이다.

결론으로*** Salesforce는 data partitioning과 Multitenant Query Optimizer를 토대로 사용자의 질의에 대한 결과를 가상의 Application Components를 통해 화면으로 보여준다.***

[출처] https://www.techtarget.com/whatis/definition/multi-tenancy
[출처] https://www.salesforce.com/video/1768346/
