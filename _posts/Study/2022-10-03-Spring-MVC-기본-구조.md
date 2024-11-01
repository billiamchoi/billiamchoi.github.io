---
layout: post
title: "Spring MVC 기본 구조"
description: "이 글에서는 Spring MVC에 대해 중점적으로 알아볼 예정이다."
date: 2022-10-03T16:47:00 +09:00
categories: study
keyword:
  - Spring
tags: ["Java", "MVC", "Spring"]
img-tag: study
---

이 글에서는 Spring MVC에 대해 중점적으로 알아볼 예정이다.

Spring MVC는 스프링의 서브 프로젝트이다.

즉 스프링은 하나의 기능을 위해서만 만들어진 프레임워크가 아니라 '코어'라고 할수 있는 프레임워크에 여러

서브 프로젝트를 결합해서 다양한 상황에 대처할 수 있도록 개발되었다.

## Spring Legacy Project의 구조

xml을 사용할 경우 아래와 같은 그림의 형태가 된다.

![](/assets/img/44e055db-9dd3-4347-a1a9-7a8537edba6d-image.png)

## Spring MVC Config files

스프링에서는 3가지 config file이 존재한다.

이에 대해 간략하게 알아보자.

1. servlet-context.xml
2. root-context.xml
3. web.xml

**1. servlet-context.xml**

- It is a Web Application Context Configuration
- 웹 어플리케이션에서 Spring bean을 구성하기 위한 설정 파일
- root-context.xml을 사용하는 경우
  - non-web bean을 root-context.xml에 설정
  - web bean을 servlet-context.xml에 설정

**2. root-context.xml**

- Non-web bean을 설정하기 위한 파일
- 필수 사항이 아님
- Spring Security 이나 OpenEntityManagerInViewFilter에 필요한 파일

**3. web.xml**

- Tomcat과 같은 서블릿 컨테이너를 구성하기 위한 파일
- servlet filter와 servlet을 구성하기 위한 파일
- *web.xml*이 먼저 로드된 다음 선택적으로 *root context*를 로드한 다음 *web context*를 로드함

## 모델 2와 Spring MVC

모델 2방식은 로직과 화면을 분리하는 스타일의 개발 방식입니다.

이 방식에서는 사용자의 Request는 특별한 상황이 아닌 이상 먼저 Controller를 호출하게 됩니다.

이유는?

- 나중에 View를 교체하더라도 사용자가 호출하는 URL 자체에 변화가 없게 만들어 주기 때문입니다.

Controller는 데이터를 처리하는 존재를 이용해서 데이터(Model)을 처리하고

Response 할 때 필요한 데이터(Model)을 view 쪽으로 전달하게 됩니다.

좀 더 자세하게 도식화해서 보여드리겠습니다.

![](/assets/img/960f973e-40c1-48cf-a709-9ee590ac8a84-image.png)

1. 클라이언트의 모든 요청은 DispatcherServlet이 받는다.

2. DispatcherServlet은 handlerMapping을 통해서 해당하는 Controller를 실행 시킨다.

3. Controller는 적절한 서비스 객체를 호출 시킨다.

4. Service는 DB처리를 위해 DAO를 이용하여 데이터를 요청한다.

5. DAO는 mybatis를 이용하는 Mapper를 통해 작업 처리를 한다.

6. 처리한 데이터가 mapper -> DAO -> Service -> Controller로 전달됨

7. Controller는 처리된 데이터를 View Resolver를 통해 전달 받을 View가 있는지 검색한다.

8. 전달 받은 View가 있다면 View에게 처리된 데이터를 전달한다.

9. View는 처리된 데이터를 다시 DispatcherServlet에게 전달한다.

10. DispatcherServlet은 처리된 데이터를 클라이언트에게 전달한다.

</br>
</br>
</br>

**출처** : https://min-it.tistory.com/7, 코드로 배우는 스프링 웹 프로젝트 개정판
