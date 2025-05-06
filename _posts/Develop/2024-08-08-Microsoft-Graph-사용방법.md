---
layout: post
title: Microsoft Graph로 팀즈 연동하는 방법
description: Microsoft Graph로 팀즈 연동하는 방법에 대해 알아보자
date: 2024-08-08T11:17:00 +09:00
categories: develop
keyword:
  - Graph
tags:
  - Back-End
img-tag: study
---

# Microsoft Graph Manual

# 개요

### Microsoft Graph란 무엇인가?

- 여러 서비스와 장치(ex. Outlook, Teams, etc)를 연결하는 Microsoft API 개발자 플랫폼입니다.

### 목표

- 이 메뉴얼에서 Microsoft Graph를 통해 Microsoft에서 제공하는 서비스들(Outlook, Teams, etc)를 어떻게 연동하는지를 배웁니다.
- 해당 메뉴얼을 통한 배울점
  1. Microsoft 서비스를 Microsoft OAuth 2.0  Token 발급하기
  2. Token을 사용하여 Microsoft Graph를 통한 Outlook 내 메일 조회하기

### 작성일

- 2024년 08월 08일



# 목차

#### 1. Tenant 하위에 App 등록하기

#### 2. Client secret 생성하기

#### 3. Microsoft OAuth 2.0  Token 발급하기

#### 4. Microsoft Graph를 통한 Outlook 내 메일 조회하기

#### 5. Troubleshooting

#### 6. References
---

## 1. Tenant 하위에 App 등록하기

#### 1.1. 아래 링크를 눌러 Microsoft Azure에 접속

### [Microsoft Azure](https://azure.microsoft.com/ko-kr)

#### 1.2. 우측 상단 로그인 버튼을 클릭하여 Azure Portal로 이동

![azure_login.png](/assets/img/azure_login.png)

#### 1.3. Azure Portal에서 하단에 Microsoft Entra ID 클릭하여 이동

![azure_main.png](/assets/img/azure_main.png)

#### 1.4. Microsoft Entra ID에서 Applications 오른쪽 숫자 클릭하여 이동

![app](/assets/img/app.png)

#### 1.5. 좌측 상단 New registration 버튼클릭하여 새로운 앱 등록 페이지로 이동

![newregi](/assets/img/newregi.png)

#### 1.6. 앱등록 페이지에서  앱 이름을 기입하고  Register를 클릭

![registerAnApplication](/assets/img/registerAnApplication.png)



## 2. Client secret 생성하기

#### 2.1.  App registration 페이지로 다시 이동 후, All applications 탭클릭 & 새로생성한 App 클릭하여 이동

![backtoappmain](/assets/img/backtoappmain.png)

#### 2.2. Application ID와 Tenant ID를 메모장에 기입하고 Add a certificate or secret 버튼 클릭

![rememberTenentId](/assets/img/rememberTenentId.png)

1. 앱 연동에 사용할 Application ID는 중요하므로 별도 메모한다.
2. 앱 연동에 사용할 tenant ID는 중요하므로 별도 메모한다.
3. Add a certificate or secret을클릭하여 Client Secret 생성 페이지로 이동한다.

#### 2.3. Client secret 생성

![clientScretDetail](/assets/img/clientScretDetail.png)

1. New client secret 버튼을 클릭하여 client secret 생성 탭 열기
2. Client secret 설명 작성 후, Add 버튼클릭하여 client secret 생성
3. 생성된 client secret value는 한번 밖에 복사 되지 않으니 꼭 메모할 것




##  3. Microsoft OAuth 2.0  Token 발급하기

#### 3.1. 앱 관리자 승낙하기

![ts1_s](/assets/img/ts1_s.png)

1. 해당 Application으로 이동하여, 왼쪽 탭의 API permissons를 클릭
2. Grant admin consent for MSFT 버튼을 눌러 해당 앱을 관리자가 승낙하도록 함
3. 승낙이 성공적으로 이루어졌으면, 상단에 Successfully granted admin consent for requested permisions.라고 나옴

#### 3.2. Postman에서 Bearer token 발급하기

- Method: POST

- URL: https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token

  - ex) https://login.microsoftonline.com/68d57784-9201-42cb-a506-52c2c1ffea70/oauth2/v2.0/token
  - tenant_id: 앱의 tenant id

- Headers: Content-Type: application/x-www-form-urlencoded
- Body (x-www-form-urlencoded)

| Value                 | Key           |
| --------------------- | :------------ |
| 앱의 Application ID     | client_id     |
| 앱의 Client secret      | client_secret |
| password              | grant_type    |
| openid offline_access | scope         |
| 사용자 이메일               | username      |
| 사용자 비밀번호              | password      |

![post](/assets/img/post.png)

- response로 받은 access_token을 메모장에 별도 기입한다.




##  4. Microsoft Graph를 통한 Outlook 내 메일 조회하기

#### 4.1. 아래 링크를 방문하여 Microsoft Graph Explorer로 이동

링크: https://developer.microsoft.com/en-us/graph/graph-explorer

![microsoft graph](/assets/img/microsoftGraph.png)

1. my mail 클릭

2. Modifiy permissons 클릭

3. 해당 탭에 모든 Consent 클릭하여 팝업이 뜨면 모두 확인한다
  - 만약 Admin consent required일 경우 전역 관리자의 별도 승인이 필요함

#### 4.2. Postman에서 Microsoft Graph를 통한 내 메일 조회하기

- Method: GET
- URL: https://graph.microsoft.com/v1.0/me/messages
- Headers: Authorization: Bearer {3.2. Postman에서 Bearer token 발급하기에서 받은 access token}
  - ex) Bearer EwB4A8l6BAAUbDba3x2OMJElkF7gJ4z/VbCPEz0....

**실행결과**

![postmandone](/assets/img/postmandone.png)

## 5. Troubleshooting

------

#### 5.1. The user or administrator has not consented to use the application with ID

![ts1](/assets/img/ts1.png)

#### 해결방법 

![ts1_s](/assets/img/ts1_s.png)

1. 해당 Application으로 이동하여, 왼쪽 탭의 API permissons를 클릭
2. Grant admin consent for MSFT 버튼을 눌러 해당 앱을 관리자가 승낙하도록 함
3. 승낙이 성공적으로 이루어졌으면, 상단에 Successfully granted admin consent for requested permisions.라고 나옴



## 6. References

- https://en.wikipedia.org/wiki/Microsoft_Graph
- https://developer.microsoft.com/en-us/graph/graph-explorer
- https://github.com/microsoftgraph/msgraph-sdk-javascript