

---
title: [TIL] CORS 에러와, sop 개념
date: 2023-07-20 12:48:10.547 +0000
categories: [TIL]
tags: ['cs', 'til', '네트워크']
description: 프로젝트 내내 우리를 괴롭힌 CORS에 대해 배워보자
image: /assets/posts/2023-07-20-til-cors-에러와-sop-개념/thumbnail.png

---

> 참고 강의 : https://youtu.be/j2Q2Ev6CZzQ

# COSR (Cross Origin Resource Sharing)
CORS 에러는 웹 개발을 하다보면 꼭 한번 씩 마주치게 되는 문제이다.
CORS 에러를 이해하기 위해서는 가장 먼저 SOP에 대해 알아야 한다.

## SOP : same origin policy

![img](/assets/posts/2023-07-20-til-cors-에러와-sop-개념/img0.png)

- origin : URL에서 프로토콜과 호스트까지의 주소를 합친 것
    - **https://naver.com:8080/**search?… → bold 처리된 부분이 origin
- sop : **사용자가 서버에서 정보(페이지 등)를 수신할 때, 내가 요청한 오리진에서 송신된 것이 맞는지 검증하는 정책**이 바로 sop이다. (네이버에 요청을 보내면 네이버에서 와야 함)
- 만약 Sop에서 요청을 보낸 곳과 다른 주소에서 응답이 온다면 CORS Error가 발생한다. (Cross-origin, 요청을 보낸 곳과 다른 서버에서 응답을 보냄으로써 발생하는 에러)

## CORS 에러와 해결책

- 개발 시에는 클라이언트와 서버의 포트번호가 일반적으로 다르기 때문에 CORS Error가 발생한다. 

### 해결책 1 - 프론트엔드에서

```javascript
// 프론트에서
axios.get("<url>")
module.exports = {
	devServer: {
		proxy: {
			"^/<마지막 서브주소>": {
			target: "http://127.0.0.1:<포트번호>"
			}
		}
	}
}
```

위와 같이 프록시서버 환경에서 target을 서버의 포트번호로 바꾸어주면 된다.

### 해결책 2 - 서버에서
```javascript
// 또는 서버에서 (node.js express)
const cors = require("cors")
app.use(cors("<cors를 허용할 주소(화이트 리스트)>")) // 아무것도 안넣거나 "*" 넣으면 모두 허용
```

express를 이용하면 굉장히 쉽게 cors를 설정할 수 있다.
특히 배포시에는 운영환경, 테스팅 환경이 모두 존재해야 하기 때문에 서버에서 설정하는 것이 좋다.

## Preflight request

### Preflight request란

- Simple Request
    - HTTP 메서드 : GET, HEAD, POST
    - Content-Type 헤더 : application/x-www-form-urlencoded, multipart/form-data, text/plain
    - 이 두가지를 모두 만족해야함
- Simple Request에 해당하지 않는 요청을 모두 Preflight Request라고 한다.


- Preflight Request를 할 때 브라우저가 자동으로 **option 메서드로 이 요청이 cors인지 사전 요청을 하게 된다.** (PUT, DELETE 등의 요청은 위험할 수 있기 때문)
- 이때 요청에 대한 응답으로 허용하는 origin, method 종류를 받아와서 이 요청이 유효한지 확인하게 된다.
- 한번 확인이 되면 캐싱이 되어 캐싱이 살아있는 동안에는 Preflight Request를 하더라도 사전 요청과정이 생략된다.
- 이 과정에서 에러가 발생한다면 cors 에러가 발생한 것이 아닌지 확인해 보아야 한다.

## Access-Control-Allow-Credentials : true

- 토큰 기반 인증 시 cors를 허용하기 위해 프론트엔드, 백엔드에서 모두 허용해주어야 하는 설정

```jsx
// 프론트엔드
axios.get("<url>", {
	headers: {
		Authorization: `Bearer {토큰}`
		withCredentials: true
	}
})

// 서버
app.use(cors({
	origin: true,
	credentials: true
}))
```

- 이 때 cors에 와일드카드(*)를 넣으면 에러 발생 (쿠키나 헤더 인증은 민감하다!)

        