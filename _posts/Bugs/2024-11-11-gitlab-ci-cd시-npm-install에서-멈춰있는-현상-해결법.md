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

어느날 갑지가 원래 돌아가던 Gitlab CICD pipeline이 정상적으로 동작하지 않음\
npm install -g @sap/cds-dk에서 계속 멈춰 있어 개발서버가 정상적으로 배포되지 않음

## 2. 원인 분석 및 결과

문제점을 해결하기 위해 아래와 같이 기본 run 명령어에 --verbose 옵션을 추가하였다.\

```
RUN npm install -global @sap/cds-dk --verbose
```
확

## 3. 문제 해결



