---
layout: post
title: "[SEO] robots.txt 개선"
description: "robots.txt 문법을 "
date: 2024-11-01T10:08:00 +09:00
categories: bugs
keyword:
  - SEO
  - robots.txt
tags:
  - blog
  - html
img-tag: html
---
# 목차

### 1. 문제점

### 2. 원인

### 3. 결과

## 1. 문제점

구글 검색 콘솔에 sitemap을 등록하고 몇일이 지났다.\
그러나 아래와 같이 제출한 사이트맵은 계속 가져올 수 없음 상태가 지속되었다.\
또한 색인도 생성되지 않았다.

![](/assets/img/sitemap_stat.png)

![](/assets/img/total_count.png)

이에 두가지 행동을 취했다.

1. sitemap.xml 유효성 체크
2. robots.xml 유효성 체크
