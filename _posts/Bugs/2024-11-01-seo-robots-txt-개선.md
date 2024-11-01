---
layout: post
title: "[SEO] robots.txt 개선"
description: robots.txt을 개선하여 검색 최적화를 해보자
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

### 2. 원인 분석 및 결과

### 3. 문제 해결

- - -

## 1. 문제점

구글 검색 콘솔에 sitemap을 등록하고 몇일이 지났다.\
그러나 아래와 같이 제출한 사이트맵은 계속 가져올 수 없음 상태가 지속되었다.\
또한 색인도 생성되지 않았다.

![](/assets/img/sitemap_stat.png)

![](/assets/img/total_count.png)

### 2. 원인 분석 및 결과

문제점에 대한 원인을 분석하기 위해 두가지 행동을 하였다.

1. sitemap.xml 유효성 체크
2. robots.xml 유효성 체크

#### 1. sitemap.xml 유효성 체크

sitemap.xml이란

* 웹사이트의 URL 구조를 검색 엔진에 알려주는 파일
* 검색 엔진은 사이트맵을 통해 페이지를 효율적으로 크롤링하고, 검색 결과에 빠르게 반영함
* URL 구조와 정보를 명확히 제공해 검색 노출을 최적화하는 역할

사이트맵 유효성 체크는 XML-Sitemaps.com을 사용하였다.\
링크: <https://www.xml-sitemaps.com/validate-xml-sitemap.html>\
유효성 검사를 진행한 결과, 문제가 없음을 확인하였다.

![](/assets/img/success.png)

#### 2. robots.txt 유효성 체크

robots.txt란

* 파일은 웹사이트의 크롤링 규칙을 검색 엔진에 알려주는 파일
* 이를 통해 특정 페이지나 디렉토리를 크롤러가 접근할 수 있게 하거나 차단할 수 있음

robots.txt 유효성 체크는 Google Search Console 설정에서 직접 확인하였다.\
설정 -> robots.txt 보고서열기를 클릭하면 확인가능하다.\
**확인 결과, robots.txt에 Sitemap이 어디인지 명시하는 라인에 문제가 있음을 확인하였다.**

![](/assets/img/crawling_stat.png)



### 3. 문제 해결

Sitemap 필드에 내 사이트 sitemap 주소로 변경한 뒤,\
(sitemap.xml은 프로젝트 루트 디렉토리에 존재함)
원격 깃 저장소에 push했다.\

