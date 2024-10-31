---
layout: post
title: "[GitHub Blog-3] Netlify 배포 후 설정"
description: Netlify 배포 후 GitHub Pages  연동 및 초기 설정에 대해 배웁니다.
date: 2024-10-31T12:59:00 +09:00
categories: develop
keyword:
  - HeadlessCMS
  - Netlify
tags:
  - jekyll
  - blog
img-tag: github
---
# 목차


### 1. Netlify에 GitHub Pages 배포


### 2.


### 3.


- - -


## 1. Netlify에 GitHub Pages 배포


Netlify에 접속하여 로그인을 합니다.


<https://www.netlify.com>


Add new site를 클릭 후 Import an existing project를 클릭합니다.\
처음 계정 생성 시 바로 사이트 생성 페이지가 나옵니다.


![](/assets/img/netlify_add_new_site.png)


Git Provider 연결 화면에서 GitHub를 선택합니다.


![](/assets/img/connect_git_provider.png)


Select repository 화면에서 자신의 GitHub Pages를 선택합니다.


![](/assets/img/select_repo.png)






Configure site and deploy 화면에서 아래와 같이 작성하면 된다.


Team은 자신의 팀이름이고 


Branch to Deploy는 프로젝트 별로 다를 수 있다. (main or master)


작성을 하고 하단에 Deploy 버튼을 클릭


![](/assets/img/config_site_deploy.png)
