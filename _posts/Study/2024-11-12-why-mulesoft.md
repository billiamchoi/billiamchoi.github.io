---
layout: post
title: About MuleSoft
description: 뮬소프트에 대해 알아보자
date: 2024-11-12T11:17:00 +09:00
categories: study
keyword:
  - MuleSoft
tags:
  - Back-End
img-tag: study
---
## 개요

### 목표

* MuleSoft의 도입 이유 및 특성에 대해 알아보자

## 목차

### 1. SI의 변화

### 2. API 관리 Trend

### 3. Full Life Cycle API Management

### 4. API-led Connectivity

- - -

## 1. SI의 변화

![](/assets/img/si_변화.22.png)

* SI에서 개발 방법은 총 3개의 트랜드로 변화하였다.
* point to point 에서 ESB를 거쳐 MSA에 도달할 때 까지 API 관리의 중요성을 더 높아짐

## 2. API 관리 Trend

* 다양한 유형의 API 채택으로 전사 표준화 대책 필요
* 소비자 요구 충족을 위한 Customer Experience 강화 및 Headless Commerce API 아키텍쳐/환경 부각
* 클라우드 전환에 따른 API를 통한 해킹 위험 부각으로 보안 거버넌스 강화 필요

## 3. Full Life Cycle API Management

### Full Life Cycle API Management란?

API를 생성부터 폐기까지 전체 수명 주기 동안 감독하는 프로세스를 의미합니다.\
여기에는 API 설계, 게시, 문서화, 보안 및 분석까지 모든 것이 포함됩니다.\
효과적인 API 전략에는 API를 쉽게 검색하고 재사용할 수 있게 하고 적절하게 관리하고 보안을 유지하는 API 관리 솔루션이 포함되어야 합니다.

### 3가지 API 라이프사이클 단계

![](/assets/img/three_life_cycle_api_management.png)

#### Design

API 라이프사이클의 첫 번째 단계는 디자인입니다.\
API가 생성될 때입니다. API 디자인은 외부에서 내부로의 관점에서 시작하며, API의 "인터페이스/계약"으로 시작합니다. 

API 개발자는 먼저 API의 "사용자 인터페이스"를 만들고 API가 어떻게 보이고 동작하는지 결정합니다.\
이를 API 계약이라고도 합니다.\
이 접근 방식은 일반적으로 "디자인 우선" 접근 방식이라고 하며 최상의 경험을 위해 최적화하기 위해 의도적인 API 디자인 라이프사이클을 따라야 합니다.\
이 단계는 개발자가 쉽게 이해할 수 있는 방식으로 계약을 지정하기 위해 사람이 읽을 수 있는 방식으로 수행해야 합니다.

이 단계에서 API 개발자는 다음 작업을 수행합니다.

**디자인**: 프로세스 및 비즈니스 요구 사항 식별, 논리적 데이터 모델 생성, 논리적 서비스로 변환, API 그룹화

**시뮬레이션**: 모델 API 리소스, 모델 API 작업/메서드, 모델 요청/응답 페이로드/코드

**피드백**: API 모의, 대화형 콘솔 게시, 노트북 사용 사례 생성, 개발자 피드백 수신

**검증**: 개발자 피드백을 기반으로 적절한 API 디자인을 수정하고 지속적으로 검증합니다.

잘 설계된 모든 API는 다른 API에서 반복성을 가질 것입니다. 이는 API의 구조적 수준(명사 리소스)과 메서드 수준(동사) 모두에서 모범 사례 패턴으로 쉽게 캡슐화될 수 있습니다.\
따라서 API 개발자가 설계 프로세스를 진행하면서 반복 가능한 패턴을 발견하고 공유할 수 있는 것이 중요합니다.

#### Implementation

API 구현은 차세대 기업을 가능하게 하는 데 중요한 부분입니다.\
수백 또는 수천 개의 API를 백엔드와 서로 연결할 수 있도록 하는 것이 핵심입니다. 이는 체계적인 방식으로 수행되어야 합니다.

구현 단계에는 빌드와 테스트라는 두 가지 단계가 있습니다. 

API 개발자가 다음과 같은 아키텍처 패턴을 쉽게 사용할 수 있도록 하는 체계적인 접근 방식을 구축합니다.

* **Orchestration** 
* **Transformation** 
* **Routing**
* **Data mapping** 
* **Connectivity across systems**

API가 빌드되면 예상대로 작동하는지 확인하기 위해 API 테스트를 여러 차례 거칩니다.\
문제 없이 테스트를 완료하면 다음 라이프사이클 단계로 넘어갈 수 있지만, 대부분의 경우 API는 배포로 넘어가기 전에 여러 차례의 테스트와 조정을 거칩니다.\
여기서 테스트 자동화 도구는 중요한데, 이는 지속적인 제공 및 배포의 DevOps 프로세스에 통합되기 때문입니다. 

#### Management

애플리케이션 빌딩 블록이 보안 및 아키텍처 거버넌스의 모범 사례를 따르고 있는지 확인하려면 런타임에 정책을 적용하는 것이 중요합니다.\
API 관리자를 통해 모든 트래픽을 모니터링하는 것도 마찬가지로 중요한데, 약한 고리 하나만 있어도 배를 침몰시킬 수 있기 때문입니다.

이 수명 주기 단계에는 다음 단계가 있습니다.

* **Secure**
* **Deploy**
* **Monitor**
* **Troubleshoot**
* **Manage**

또한, 이러한 API는 다른 목적으로 재사용하기 위해 발견 가능해야 합니다.\
소비하는 개발자가 쉽게 찾고, 조사하고, 이해할 수 있도록 적절하게 게시할 수 있는 능력은 전체 프로그램을 만들거나 망칠 수 있습니다.

## 4. API-led Connectivity

### API-led Connectivity란 무엇인가?

API-led Connectivity는 조직의 생태계 내에서 재사용 가능하고 목적이 있는 API를 통해 데이터를 애플리케이션에 연결하는 체계적인 방법입니다.\
이러한 API는 시스템에서 데이터를 잠금 해제하고, 데이터를 프로세스로 구성하거나, 경험을 제공하는 특정 역할을 하도록 개발되었습니다. 

![](/assets/img/api-led-connectivity.png)

### API-led Connectivity의 특징

#### Experience API

* Web / Mobile Application 등 End User가 직접 사용하는 Application과 연결							
* Process 복잡함은 노출하지 않고, Application과 상호작용 할 수 있는 단순하고 일관적인 인터페이스 제공							

#### Process API

* 기능 Process 담당 : System API 연결하여 여러 System의 Orchestration을 통해 Process를 구현				- Process Automation으로 구현							

#### System API

* Database, System 등과 직접 연결하여 Data 추출 및 관리
* 예산 범위내 기한 준수
* 재사용 가능
* 변화 대비 설계
* 거버넌스/규정준수/보안 및 확장성 고려한 구축
