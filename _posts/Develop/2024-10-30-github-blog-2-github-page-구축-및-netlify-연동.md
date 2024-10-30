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

   * 위 그림과 같이 프로젝트 root에 admin 디렉토리를 생성한다.
2. admin 디렉토리 내 config.yml 생성

   * 아래 코드와 같이 config.yml을 생성
   * backend repo는 자신의 레포지토리에 맞게 수정한다.\
     "GitHub 사용자명/저장소명"와 같이 변경한다\
     ex) "billiamchoi/billiamchoi.github.io"
   * backend branch는 자신의 레포지토리 브랜치에 맞게 수정한다.\
     오래된 브랜치일 경우 master일 것이고 새로 설정한 브랜치일 경우 main일 것이다.\
     ex) master

   ```yml
   backend:
     name: git-gateway
     repo: "GitHub 사용자명/저장소명" # GitHub 사용자명과 저장소명
     branch: 콘텐츠가 저장될 브랜치 # 콘텐츠가 저장될 브랜치

   media_folder: "assets/img" # Folder where images will be uploaded
   public_folder: "/assets/img" # Path to access images in posts

   collections:
     - name: "bugs"
       label: "Bugs"
       folder: "_posts/Bugs"
       create: true
       slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
       fields:
         - { label: "Layout", name: "layout", widget: "hidden", default: "post" }
         - { label: "Title", name: "title", widget: "string" }
         - { label: "Description", name: "description", widget: "string" }
         - {
             label: "Date",
             name: "date",
             widget: "datetime",
             dateFormat: "YYYY-MM-DD",
             timeFormat: "HH:mm:ss Z",
             default: "{{now}}",
           }
         - {
             label: "Categories",
             name: "categories",
             widget: "hidden",
             default: bugs,
           }
         - { label: "Keyword", name: "keyword", widget: "list", allow_add: true }
         - {
             label: "Tags",
             name: "tags",
             widget: "select",
             multiple: true,
             options:
               [
                 github,
                 blog,
                 Front-End,
                 Back-End,
                 jekyll,
                 html,
                 css,
                 SAPUI5,
                 CAP,
                 Java,
                 JavaScript,
               ],
           }
         - {
             label: "Image Tag",
             name: "img-tag",
             widget: "select",
             options: [blog, github, jekyll, html, css, study],
           }
         - { label: "Body", name: "body", widget: "markdown" }

     - name: "develop"
       label: "Develop"
       folder: "_posts/Develop"
       create: true
       slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
       fields:
         - { label: "Layout", name: "layout", widget: "hidden", default: "post" }
         - { label: "Title", name: "title", widget: "string" }
         - { label: "Description", name: "description", widget: "string" }
         - {
             label: "Date",
             name: "date",
             widget: "datetime",
             dateFormat: "YYYY-MM-DD",
             timeFormat: "HH:mm:ss Z",
             default: "{{now}}",
           }
         - {
             label: "Categories",
             name: "categories",
             widget: "hidden",
             default: develop,
           }
         - { label: "Keyword", name: "keyword", widget: "list", allow_add: true }
         - {
             label: "Tags",
             name: "tags",
             widget: "select",
             multiple: true,
             options:
               [
                 github,
                 blog,
                 Front-End,
                 Back-End,
                 jekyll,
                 html,
                 css,
                 SAPUI5,
                 CAP,
                 Java,
                 JavaScript,
               ],
           }
         - {
             label: "Image Tag",
             name: "img-tag",
             widget: "select",
             options: [blog, github, jekyll, html, css, study],
           }
         - { label: "Body", name: "body", widget: "markdown" }

     - name: "study"
       label: "Study"
       folder: "_posts/Study"
       create: true
       slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
       fields:
         - { label: "Layout", name: "layout", widget: "hidden", default: "post" }
         - { label: "Title", name: "title", widget: "string" }
         - { label: "Description", name: "description", widget: "string" }
         - {
             label: "Date",
             name: "date",
             widget: "datetime",
             dateFormat: "YYYY-MM-DD",
             timeFormat: "HH:mm:ss Z",
             default: "{{now}}",
           }
         - {
             label: "Categories",
             name: "categories",
             widget: "hidden",
             default: study,
           }
         - { label: "Keyword", name: "keyword", widget: "list", allow_add: true }
         - {
             label: "Tags",
             name: "tags",
             widget: "select",
             multiple: true,
             options:
               [
                 github,
                 blog,
                 Front-End,
                 Back-End,
                 jekyll,
                 html,
                 css,
                 SAPUI5,
                 CAP,
                 Java,
                 JavaScript,
               ],
           }
         - {
             label: "Image Tag",
             name: "img-tag",
             widget: "select",
             options: [blog, github, jekyll, html, css, study],
           }
         - { label: "Body", name: "body", widget: "markdown" }
   ```
3. admin 디렉토리 내 index.html 생성

   * 아래 코드와 같이 index.html을 생성

     ```html
     <!DOCTYPE html>
     <html>
       <head>
         <meta charset="utf-8" />
         <meta name="viewport" content="width=device-width, initial-scale=1.0" />
         <title>Content Manager</title>
       </head>
       <body>
         <!-- Include the script that builds the page and powers Netlify CMS -->
         <script src="https://unpkg.com/decap-cms@^3.0.0/dist/decap-cms.js"></script>
         <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
         <script>
           if (window.netlifyIdentity) {
             window.netlifyIdentity.on("init", (user) => {
               if (!user) {
                 window.netlifyIdentity.on("login", () => {
                   document.location.href = "/admin/";
                 });
               }
             });
           }
         </script>
       </body>
     </html>

     ```
