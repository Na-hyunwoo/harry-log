---
title: Cookie, Session, Token, Storage
date: '2022-12-13'
tags: ['Cookie', 'Session', 'Token', 'Storage']
draft: false
summary: 'HTTP는 stateless라는 특징을 갖습니다. 이로 인해 서버는 요청에 대한 정보를 식별할 수 없고, 이는 사용자가 매번 번거로운 인가 절차를 진행하게 했습니다. 이를 보완하기 위해 나온 개념이 Cookie입니다. '
---

# Cookie, Session, Token, Storage

HTTP는 **stateless**라는 특징을 갖습니다. 이로 인해 서버는 요청에 대한 정보를 식별할 수 없고, 이는 사용자가 매번 번거로운 인가 절차를 진행하게 했습니다. 이를 보완하기 위해 나온 개념이 Cookie입니다.

> stateless: **서버가 클라이언트의 이전 상태를 보존하지 않음**

## Cookie

Cookie는 사용자가 어떤 웹사이트에 방문할 경우 사용자의 브라우저에 **저장되는 작은 데이터입니다.**

그러면 브라우저에 저장되는 작은 데이터가 어떻게 번거로웠던 인가 절차를 축소시킬 수 있었을까요 ?

![cookie-process](https://velog.velcdn.com/images/dusdjeks/post/3e2caab4-ba55-41f5-95aa-693424b9bd79/image.png)

쿠키는 주로 웹 서버에 의해 만들어집니다. 서버가 HTTP 응답 헤더의 Set-Cookie에 내용을 넣어 전달하면, 브라우저는 이 내용을 자체적으로 브라우저에 저장합니다. 이게 바로 쿠키입니다. **브라우저는 사용자가 쿠키를 생성하도록 한 동일 사이트에 접속할 때마다 쿠키의 내용을 Cookie 요청 헤더에 넣어서 함께 전달합니다.**

쿠키는 이렇게 기존에 번거로웠던 인가 과정을 없앨 수 있었습니다. 그러나, Cookie에는 치명적인 단점들이 존재했습니다. **바로 클라이언트에서 값을 수정할 수 있다는 것**입니다. 그리고 만약 cookie가 ID / PW와 같은 민감한 정보들을 가지고 있다면, **노출되기 쉬워 보안에 매우 취약**합니다. 이러한 배경 속에서 session이 등장하게 되었습니다.

## Cookie + Session

이 방식은 Cookie에 ID, PW와 같은 중요 정보들을 담는 것이 아니라, **인증을 위한 별개의 정보를 서버의 세션 저장소에 저장하는 방식**입니다. 클라이언트는 이 정보를 쿠키에 유니크한 키 형태로 담아서 서버에 요청합니다. 이 때, 서버는 세션 저장소에 있는 정보와 요청한 정보가 일치하는지 확인합니다.

Cookie + Session의 동작 방식은 다음과 같습니다.

1. 클라이언트가 ID/PW로 서버에 로그인 요청을 합니다.

2. ID/PW로 인증 후 사용자를 식별할 수 있는 session ID를 만들어 세션 저장소에 저장합니다.

3. 클라이언트는 session ID에 대한 쿠키를 저장하고 있습니다.

4. 클라이언트가 서버에 요청을 할 때, session ID를 함께 서버에 요청합니다.

5. 서버는 session ID를 전달받아 session ID의 정보를 응답합니다.

사용자에 대한 정보를 서버에 저장하기 때문에 쿠키보다 보안의 측면에서 뛰어나지만, 사용자가 많아질수록 서버 메모리를 많이 차지하게 됩니다.

이처럼 사용자가 증가함에 따라 **서버 메모리를 많이 사용하게 된다는 단점**과 http의 **stateless를 위배**하게 됩니다. stateless라면 서버는 클라이언트의 상태를 저장하지 않아야 하지만 **세션 저장소라는 곳에서 클라이언트의 상태를 저장하게 되므로 stateful하게 됩니다.** 이에 따라 Token이라는 개념이 등장하게 됩니다.

## Token

서버가 클라이언트를 정확히 구별할 수 있도록 **유니크한 정보를 담은 암호화 데이터.**

## JWT (JSON Web Token)

통신에 JSON을 이용하여 JSON 객체를 통해 두 당사자 간 정보를 보안이 적용된 안전한 방식으로 전달합니다.

JWT는 다른 인증방식과 비교했을 때 스스로 정보를 담고 있고 디지털 서명이 되어있기 때문에 정보에 안정성이 보장된다고 할 수 있습니다.

### 인증 과정

![jwt-process](https://velog.velcdn.com/images/dusdjeks/post/61adabf6-132b-4042-b0ae-128e4990960d/image.png)

### JWT 구조

![jwt-architecture-1](https://velog.velcdn.com/images/dusdjeks/post/e5dd6a62-7dc4-4673-ac01-a81dc0e2eec9/image.png)
![jwt-architecture-2](https://velog.velcdn.com/images/dusdjeks/post/11e95785-db28-4aa0-b0cf-d902a83f0082/image.png)

- Header
  - typ - 토큰의 타입
  - alg - 해싱 알고리즘
- Payload
  - 토큰에 담을 정보의 조각들인 클레임이 담겨있습니다.
- Signature
  - 토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 고유한 암호화 코드입니다.

JWT는 인증에 필요한 정보가 토큰에 들어있기 때문에 별도의 저장소가 필요하지 않습니다. 또한 Cookie - Session 방식의 문제점인 stateful한 특성을 **stateless하게 가져갈 수 있습니다.**

## Web Storage

Web Storage는 브라우저에서 키/값 쌍을 **쿠키보다 훨씬 직관적으로 저장할 수 있는 방법**을 제공합니다.

### Local Storage

브라우저를 닫았다 열어도 데이터가 남아있습니다.

브라우저에서 직접 삭제하지 않는 이상 데이터가 남아있습니다.

### Session Storage

브라우저 또는 탭이 닫힐 때까지만 데이터를 저장합니다.

데이터를 서버로 전송하지 않습니다.

저장 공간이 쿠키보다 큽니다. (쿠키: 4KB, 스토리지: 5MB)

### 여기서 들었던 의문

storage는 cookie의 단점들을 상쇄해서 나온 것처럼 보인다. 사용하기 훨씬 직관적이고 저장할 수 있는 데이터의 용량도 훨씬 크다. **그런데 왜 인증 정보는 storage가 아닌 cookie에 저장하라고 하는 것일까 ?** 그 이유는 다음과 같다. ([출처](https://www.pivotpointsecurity.com/local-storage-versus-cookies-which-to-use-to-securely-store-session-tokens/))

![question](https://velog.velcdn.com/images/dusdjeks/post/e5146f9b-7f16-481b-87df-0b9667711bc8/image.png)

정리해서 말하자면, storage는 cookie의 단점들을 상쇄하면서 나온 것이 아니라, 각각은 서로 다른 역할이 있고 그에 따라 장점과 단점이 다르다는 말이다. **쿠키는 서버에서 읽혀지길 의도한 녀석인 반면에 localStorage는 브라우저에서만 읽을 수 있다는 의미이다.** 그니까, 로그인 인증에 대한 정보는 항상 서버에 header에 담겨서 요청하기 때문에 **cookie에 존재하는 것이 맞다.**

## 레퍼런스

[Local Storage Versus Cookies: Which to Use to Securely Store Session Tokens](https://www.pivotpointsecurity.com/local-storage-versus-cookies-which-to-use-to-securely-store-session-tokens/)

[쿠키와 document.cookie](https://ko.javascript.info/cookie#ref-177)

[JWT란? JWT , Session, Cookie 비교](https://velog.io/@znftm97/JWT-Session-Cookie-%EB%B9%84%EA%B5%90-sphsi9yh)
