---
layout: post
title: React UI Framework 분석 및 장단점 비교
description: React 유명 UI Framework 분석 및 장단점 비교
date: 2024-11-06T15:12:00 +09:00
categories: study
keyword:
  - React
tags:
  - Front-End
img-tag: study
---
## 개요

### 목표

* 본문에서 3개 React UI Framework를 소개하고 각각의 특징점, 장단점을 비교 분석합니다.\
  선정 기준: github star count

## 목차

### 1. Material-UI (MUI)

### 2. Ant Design

### 3. Chakra UI

### 4. 비교

- - -

## 1. Material-UI (MUI)

* **정의**

  * Material UI는 Google의 Material Design을 구현한 오픈소스 React 구성 요소 라이브러리입니다.

    * Material Design이란?

      * 플랫 디자인의 장점을 살리면서도 빛에 따른 종이의 그림자 효과를 이용하여 입체감을 살리는
        디자인 방식
* **특징**

  * 탬플릿 스토어가 존재하며 무료 탬플릿이 있음
    링크: https://mui.com/store/
  * Transfer List가 존재함

    * Transtfer List란?

      * 두 개의 목록 간에 아이템을 이동시킬 수 있는 UI 컴포넌트입니다

        ![](/assets/img/1.transfer_list.png)
  * **Server-side Data handling이 안됨**

    * 아래 4가지 항목을 무료버전에서 사용할 수 없음

      1. 서버 측 페이징
      2. 서버 측 필터링
      3. 서버 측 정렬
      4. 서버 측 검색
    * **그러므로 무료 버전만 사용할 예정이면, Material-UI는 비추천합니다.**
* **통계**

  * star count

    * 93.9k
* **유료 버전**

  * 종류

    * **MUI X Pro**

      * 연간 $180 또는 월 $15로, 고급 데이터 그리드와 같은 추가 컴포넌트를 제공합니다.
    * **MUI X Premium**

      * 연간 $588 또는 월 $49로, Pro 버전의 모든 기능과 더불어 추가적인 고급 컴포넌트를 포함합니다.
  * 특징

    * MUI의 고급 데이터 그리드, 차트, 날짜 및 시간 피커, 트리 뷰 같은 컴포넌트를 포함한 Pro/Enterprise 패키지를 유료로 제공합니다.
    * 특히 데이터 그리드는 대규모 데이터를 다룰 수 있는 고급 기능(가상 스크롤, 고정 열, 열 정렬 및 필터링 등)을 포함함
    * Pro와 Enterprise 버전으로 나뉨
    * Enterprise는 대기업에 맞춘 추가 지원 서비스와 더 많은 고급 기능을 포함하고 있음
* **적합한 용도**

  * 현대적인 웹 애플리케이션이나 기업용 애플리케이션에 잘 어울리는 프로페셔널한 디자인
* **링크**

  * <https://github.com/mui/material-ui>

  ​

## 2. Ant Design

* **정의**

  * Alibaba에서 개발한 Ant Design은 우아하고 세련된 디자인으로 유명한 UI Framework
* **특징**

  * AntV라는 데이터 시각화 패키지 제공함
* **통계**

  * star count

    * 92.4k
* **유료 버전**

  * 공식적인 유료 버전은 존재하지 않음
    그러나 AntV의 일부 고급 시각화 도구나 플러그인은 유료 옵션에 포함 될 수 있음
* **적합한 용도**

  * 대시보드나 구조적인 레이아웃을 필요로 하는 기업용 애플리케이션.
* **링크**

  * <https://github.com/ant-design/ant-design>

## 3. Chakra UI

* **정의**

  * Chakra UI는 단순하고 유연하며 접근성에 중점을 둔 UI Framework
* **특징**

  * 접근성에 중점을 두고 있음
  * 스타일링을 위한 훅과 테마 및 컴포넌트 조합 기능 제공
  * Tree Shaking 지원

    * Tree Shaking이란?

      * 사용된 컴포넌트만 번들에 포함하는 행위.
      * 이로 인해 불필요한 코드가 제거되어 번들 크기가 줄어들고, 로딩 속도가 향상됨
  * 모듈 단위 사용 가능
* **통계**

  * star count

    * 37.8k
* **유료 버전**

  * 종류

    * **마케팅 UI**

      * $149로, 마케팅에 특화된 UI 컴포넌트를 제공합니다.
    * **애플리케이션 UI**

      * $149로, 애플리케이션 개발에 필요한 UI 컴포넌트를 포함합니다.
    * **마케팅 + 애플리케이션 UI**

      * $249로, 위 두 가지를 모두 포함한 패키지입니다.
    * **팀 라이선스**

      * $899로, 최대 5명의 개발자가 사용할 수 있는 라이선스입니다.
  * 특징

    * **Chakra UI Pro**는 Chakra UI의 유료 버전으로, 프리미엄 디자인 요소와 템플릿을 제공하여 개발자가 더 빠르게 고퀄리티의 웹 애플리케이션을 구축할 수 있도록 돕습니다.\
      Chakra UI Pro는 다음과 같은 고급 기능과 리소스를 포함합니다

      1. **프리미엄 컴포넌트**

         * 일반 Chakra UI에서는 제공되지 않는 추가 컴포넌트(예: 고급 카드, 내비게이션 바, 히어로 섹션, 모달)들이 포함되어 있어 디자인과 기능을 더욱 쉽게 추가할 수 있습니다.
      2. **디자인 템플릿**

         * 로그인, 회원가입, 대시보드 등 자주 사용되는 템플릿이 미리 제공되며, 이를 활용해 프로젝트의 기본 레이아웃을 빠르게 설정할 수 있습니다.
      3. **정기 업데이트 및 유지 보수**

         * 유료 사용자는 Chakra 팀으로부터 정기적으로 업데이트와 개선된 기능을 받을 수 있으며, 문제 발생 시 빠른 지원을 받을 수 있습니다.
      4. **비즈니스 친화적인 라이선스**

         * Chakra UI Pro는 상업 프로젝트에서도 사용할 수 있도록 라이선스가 마련되어 있어, 기업 및 대규모 프로젝트에서도 안전하게 활용할 수 있습니다.
* **적합한 용도**

  * 접근성과 커스터마이징이 중요한 경량화된 프로젝트
* **링크**

  * <https://github.com/chakra-ui/chakra-ui>

## 4. 비교

#### 특성별 비교

| 특성           | Material-UI (MUI)  | Ant Design       | Chakra UI    |
| ------------ | ------------------ | ---------------- | ------------ |
| **디자인 스타일**  | Material Design 기반 | 엔터프라이즈 지향        | 모던하고 유연한 스타일 |
| **컴포넌트 다양성** | 매우 다양              | 매우 다양            | 제한적          |
| **커스터마이징**   | 중간 정도 (약간 복잡)      | 약간 어려움           | 매우 유연함       |
| **국제화 지원**   | 제한적                | 매우 강력            | 제한적          |
| **접근성**      | 기본 제공              | 기본 제공            | 매우 우수        |
| **성능**       | 중간 정도 (크기 조정 필요)   | 중간 정도 (크기 조정 필요) | 경량화 (성능 우수)  |
| **커뮤니티와 지원** | 매우 활발              | 매우 활발            | 상대적으로 적음     |

#### 장단점 비교

| Framework       | 장점                                                                                  | 단점                                                          |
| --------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **Material-UI** | \- 구글 Material Design 기반<br>- 다양한 컴포넌트 제공<br>- 테마 커스터마이징 용이<br>- 강력한 커뮤니티와 문서화      | \- 복잡한 커스터마이징<br>- 제한된 디자인 유연성<br>- 번들 크기 큼 <br>- 핵심 기능이 유료 |
| **Ant Design**  | \- 엔터프라이즈 애플리케이션에 최적<br>- 데이터 중심 컴포넌트 강점<br>- 국제화(i18n) 지원<br>- 디자인 언어 기반 커스터마이징 용이 | \- 스타일 변경 어려움<br>- 디자인 통일성으로 유연성 제한<br>- 번들 크기 큼            |
| **Chakra UI**   | \- 모던하고 유연한 스타일<br>- 경량화된 컴포넌트로 성능 우수<br>- 테마 기반 손쉬운 커스터마이징<br>- 높은 접근성 제공          | \- 제한된 컴포넌트<br>- 상대적으로 작은 커뮤니티<br>- 엔터프라이즈 기능 부족            |
