---
layout: post
title: "[GitHub Blog-2] GitHub Page 구축 및 Netlify 연동"
description: GitHub Page 구축하고 Netlify 초기 설정법에 대해 배운다.
date: 2024-10-30T14:04:00 +09:00
categories: develop
keyword:
  - HeadlessCMS
  - ""
tags:
  - blog
  - jekyll
img-tag: github
---
# 목차

### 1. GitHub Pages 설정

### 2. Netlify Admin Page 생성

### 3. GitHub Page와 Netlify 연동

- - -

이번 포스트에서 다음과 같은 항목에 대해 배운다.

1. GitHub Page 설정
2. Netlify Admin Page 생성
3. GitHub Page와 Netlify 연동

## 1. GitHub Pages 설정

이글에서는 완전 초기 세팅은 생략하고 원하는 GitHub Pages를 Clone 받고\
자신의 레포지토리에 Push하는 것을 다룬다.

1. 아래 명령어를 터미널에 입력하여 원하는 GitHubPage Clone하기\
   필자는 코딩독학님의 블로그를 클론하였음

   ```bash
   $ git clone https://github.com/martinkang/martinkang.github.io.git
   ```
2. 해당 프로젝트 파일 내 .git 파일을 제외하고 모두 복사하여 자신의 GitHub Pages 레포지토리에 붙여넣는다.
3. 변경사항을 반영하기 위해 git push를 실행한다.

## 2. Netlify Admin Page 생성

1. 프로젝트 root에 admin 디렉토리 생성

   ![](/assets/img/admin.png)
2. admin 디렉토리 내 config.yml 생성


3. admin 디렉토리 내 index.html 생성
