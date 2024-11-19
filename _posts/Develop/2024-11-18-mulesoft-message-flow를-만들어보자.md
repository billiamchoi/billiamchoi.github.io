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

### 3.

### 4.

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
