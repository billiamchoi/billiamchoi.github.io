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

### 2. Netlify 설정

### 3. 사용자 설정

### 4. 주의 사항

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

Site overview 페이지에서 사이트 링크를 클릭하여 Netlify에 GitHub Pages가 배포되었는지 확인하자.

![](/assets/img/visit.png)

## 2. Netlify 설정

### Identity 활성화

메뉴에서 Identity을 활성화한다.\
이 기능을 사용함으로써, 가입 등의 인증 관련 기능을 사용 할 수 있다.

![](/assets/img/enable_identity.png)

### Git Gateway 활성화

Netlify CMS 에서 Git 을 처리하기 위해 사용 설정을 진행한다.\
Settings - Identity 메뉴에 Services - Git Gateway 에 있는 Enable Git Gateway 버튼을 클릭

![](/assets/img/enable_gitgateway.png)

## 3. 사용자 설정

### 회원가입을 초대전용으로 변경

Settings - Identity - Registration 에서 Edit 버튼을 눌러서 Invite only로 변경\
이 설정으로 누구나 아이디를 생성할 수 없게 된다.

![](/assets/img/invite_onlypng.png)

### 아이디 추가

invite users를 클릭하여 관리자로 사용할 이메일을 입력하고 Send를 클릭

![](/assets/img/invite_user.png)



해당 이메일로 인증메일이 도착해 있을 것입니다. Accept the invite 를 클릭하면 블로그 사이트로 이동됨

URL이 아래와 비슷할텐데, 중간에 admin을 넣어서 CMS 페이지로 바뀜



변경 전

```
https://somethingsweet.netlify.app/#invite_token=-W5a_7Eao-GCIMVEpr97Vw
```



변경 후

```
https://somethingsweet.netlify.app/admin#invite_token=-W5a_7Eao-GCIMVEpr97Vw
```



그러면, 비밀번호를 설정하는 화면이 나옴.

![](/assets/img/change_password.png)

비밀번호를 설정하고 나면 자동으로 로그인이 되고 아래와 같은 페이지가 나옴

![](/assets/img/done.png)



이제 New Develop 를 눌러서 글을 쓰는 등의 Decap CMS를 사용 할 수 있음



## 4. 주의 사항

Free 버전은 1달에 100GB 대역폭, Build Time이 300분 주어진다.

대역폭은 걱정안되나.. CMS를 자주 사용한다면 빌드타임 300분은 그냥 넘을 것 같다.
