---
layout: post
title: "Axios interceptors를 통해 JWT token 관리"
description: "axios interceptor를 사용해 요청을 보낼 때와 응답을 받을 때 두 경우 모두 데이터를 가로채서 원하는 인증 flow를 생성해주는 것"
date: 2023-01-01T16:47:00 +09:00
categories: develop
keyword:
  - JWT
tags: ["JWT","JavaScript","access-token","axios","refresh-token"]
img-tag: html
---
> **핵심** : axios interceptor를 사용해 요청을 보낼 때와 응답을 받을 때 
두 경우 모두 데이터를 가로채서 원하는 인증 flow를 생성해주는 것

# 1. JWT

JWT(JSON Web Token)는 당사자 간에 정보를 JSON 개체로 안전하게 전송하기 위한 간결하고 

독립적인 방법을 정의하는 개방형 표준(RFC 7519)이라고 한다. 

이 정보는 디지털 서명되어 있으므로 확인하고 신뢰할 수 있다고 한다. 

JWT는 비밀(HMAC 알고리즘 포함) 또는 RSA 또는 ECDSA를 사용하는 공개/개인 키 쌍을 사용하여 서명할 수 있다.

더 길게 자세히 이야기 할 수 있지만 이야기가 길어지므로 패스...

# 2. Refresh token을 사용하는 이유

결론부터 말하면, access token을 발급 받기 위해 존재한다. 

Refresh token이 없고 access token만 존재하는 경우, 

토큰 만료 시간이 5분과 같이 짧다고 하면 

5분마다 인증 만료 여부를 서버에서 확인해야 한다. 

웹페이지에서 5분 마다 로그인을 해야 한다면 얼마나 귀찮을지 생각해보자.

# 3. Axios interceptor

axios는 대표적인 node.js용 Promise 기반 HTTP 클라이언트이다.

백엔드 서버에 요청을 보낼때 axios를 사용한다.

rest api를 예로 들면, GET(조회), POST(생성), PUT/PATCH(수정), DELETE(삭제)가 있다.

예를 들어 관리자 페이지를 만든다고 하면, 거의 대부분의 backend 서버에서 frontend 서버로 

통신하는 데이터는 인증이 필요하다.  

인증서버에서 access token과 refresh token이 존재하는 경우,

access token은 항상 수명이 짧을 것이며 (5분 ~ 1시간)

refresh token은 항상 access token보다 월등이 수명이 길 것이다 (1주일~1달)

**이러한 경우 frontend 서버에서 backend 서버로 요청을 보내고 인증이 만료되었을 때**

**backend서버로 refresh token을 보내서 access token을 받고 다시 요청을 보내는 것이 필요하다.**

이를 위해 axios interceptor를 사용해 말 그대로 요청을 보낼 때와 응답을 받을 때 두 경우 모두 데이터를 가로채서 원하는 인증 flow를 생성해주는 것이 이 글의 가장 큰 핵심이자 목적이다.

아래 그림을 보면 조금 더 이해가 쉬울 것이다. 

![](/assets/img/46519095-8b97-4e9f-878b-855b4e819399-image.png)

## 3-1. Pre interceptor

```
instance.interceptors.request.use(
  (config) => {
    if (!config.headers) return config

    let token = null

    if (config.url === REFRESH_URL) {
      token = cookies.get('refresh_token')
    } else {
      token = cookies.get('access_token')
    }
    if (token !== null) {
      config.headers.Authorization = token
    }

    return config
  },
  (error) => {
    return Promise.reject(error)
  }
)
```

pre interceptor는 요청이 시작되기 전에 가로채는 곳이다. 

!config.url의 의미  false인 경우는 request에서 에러가 생겼다는 의미

REFRESH_URL은 백엔드 서버의 refresh token을 받는 url이다.

request.url이 REFRESH_URL이면 token을 refresh token으로 하고 

아닌 경우 access token으로한다.

그리고 에러가 없을 경우 header 'Authorization'에 token을 set해서 

서버에 요청으로 보낸다.

## 3-2. Post interceptor

아래와 같이 refresh token을 발급받는 것을 아래처럼 함수화하고

```
const getRefreshToken = async () => {
  try {
    const refreshToken = {
      refreshToken: cookies.get('refresh_token')
    }

    const data = await post(REFRESH_URL, refreshToken)

    cookies.set('access_token', data.data.data.accessToken, 0)
    cookies.set('refresh_token', data.data.data.refreshToken, 0)
    const newAccessToken = cookies.get('access_token')

    return newAccessToken
  } catch (e) {
    cookies.remove('access_token')
    cookies.remove('refresh_token')
    console.log(e)
  }
}
```

서버에서 오는 response가 401이고 

해당 rest api url이 REFRESH_URL이 아니면

refresh token을 서버에서 받아와서

header 'Authorization'에 token을 set한다.

```
instance.interceptors.response.use(
  (response) => {
    return response
  },
  async (error) => {
    const { config } = error

    if (
      config.url === REFRESH_URL ||
      error.response.status !== 401 ||
      config.sent
    ) {
      return Promise.reject(error)
    }

    config.sent = true
    const accessToken = await getRefreshToken()

    if (accessToken) {
      config.headers.Authorization = accessToken
    }

    return axios(config)
  }
)
```

아주 간략하게 정리해보았다.

끝 ~ 