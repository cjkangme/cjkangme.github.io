---
layout: single
title: "[HTML] head 태그"
categories: lecture
tags: HTML
---

> 참고자료 : 유튜브(짐코딩의 CODING GYM) - HTML/CSS 강의
>
> **주소 : [유튜브 링크](https://youtube.com/playlist?list=PLlaP-jSd-nK-ponbKDjrSn3BQG9MgHSKv)**

<br>

# HTML의 head 태그

head 태그는 웹페이지의 타이틀과 파비콘, 웹페이지 서명, URL 이미지, 스타일 등 문서의 정보 **(메타데이터)** 를 모두 담고 있는 태그를 말한다.

## `<head>`에 포함되는 요소

### - `<title>`

브라우저 탭에 표시되는 웹페이지의 제목을 선언한다.

---

### - `<base>`

문서의 **기본 URL**을 선언한다.
<br> 선언된 URL이 문서 내 **모든 상대 경로에 대한 기준**이 된다.
<br> 기본값은 문서가 위치한 디렉토리이며, 한 문서에는 하나의 base만 선언할 수 있다.

#### 주요 사용 속성

- href : 선언할 URL
- target : 문서 내 모든 하이퍼링크와 폼에 대한 기본 target 설정

#### 예제

```html
<head>
  <base href="/study/" />
</head>
```

---

### - `<link>`

외부 리소스와의 관계를 명시한다.
<br>주로 style.css를 외부에 만들어 연결할 때 가장 많이 사용하며, 사이트 아이콘 등 문서정보에 멀티미디어가 필요할 경우 사용한다.

#### 주요 사용 속성

- rel : **(필수)** 현재 문서와 외부 리소스 사이의 연관관계를 명시
- href : 링크할 외부 리소스의 URL 명시

<br>

#### 예제

```html
<!-- Style 시트 연결 -->
<link href="/style.css" rel="stylesheet" />
```

```html
<!-- 파비콘 설정 -->
<link href="./nureogni.ico/" rel="shortcut icon"
```

---

### `<style>`

style 규칙을 담고 있는 요소이다.
<br>문서의 head 내에서 사용할 수 있지만, 편의성 등의 이유로 주로 외부 stylesheet를 이용한다.

---

### `<meta>`

다른 요소로 나타낼 수 없는 메타데이터를 정의할 때 사용한다.
<br>이러한 메타데이터는 검색 엔진, 타 웹서비스에서 URL 정보 표시 등 다양한 곳에 사용된다.
<br> 그중 `오픈그래프`가 가장 많이 사용되며 이는 하단에 따로 다룰 것이다.

---

### `<script>`

주로 자바스크립트 같은 외부 코드 문서를 불러와 웹 문서에 포함할 때 사용한다.

#### 주요 사용 속성

- src : 링크할 외부 스크립트 파일의 URL 명시

#### 예제

```html
<script src="javascript.js></script>
```

---

<br><br>

# 오픈 그래프 (Open Graph)

메타 태그의 일종으로, 콘텐츠의 요약 내용이 정보를 요청한 페이지(SNS 등)로 최적화되어 전송될 수 있도록 정해진 **일종의 표준 프로토콜**을 의미한다.
<br> 메타데이터가 입력되어 있어도, 어떤 문자열이 무엇을 나타내는지 100% 자동으로 판별하기 어렵기 때문에 주로 사용한다.

## SNS에 웹사이트를 게시할 때 설정해야하는 og 메타태그

```html
<!-- 기본적으로 웹에 설정해주어야 하는 og 메타태그 -->

<meta property="og:type" content="website" />
<meta property="og:url" content="https://example.com/page.html" />
<meta property="og:title" content="Content Title" />
<meta property="og:image" content="https://example.com/image.jpg" />
<meta property="og:description" content="Description Here" />
<meta property="og:site_name" content="Site Name" />
<meta property="og:locale" content="en_US" />
<!-- 다음의 태그는 필수는 아니지만, 포함하는 것을 추천함 -->
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
```

```html
<!-- 네이버 블로그, 카카오톡 미리보기 설정 -->

<meta property="og:title" content="콘텐츠 제목" />
<meta property="og:url" content="웹페이지 URL" />
<meta property="og:type" content="웹페이지 타입(blog, website 등)" />
<meta property="og:image" content="표시되는 이미지" />
<meta property="og:title" content="웹사이트 이름" />
<meta property="og:description" content="웹페이지 설명" />
```

```html
<!-- 트위터 미리보기 설정 -->

<meta name="twitter:card" content="트위터 카드 타입(요약정보, 사진, 비디오)" />
<meta name="twitter:title" content="콘텐츠 제목" />
<meta name="twitter:description" content="웹페이지 설명" />
<meta name="twitter:image" content="표시되는 이미지 " />
```

```html
<!-- 모바일 앱 미리보기 설정 -->

<--iOS-->
<meta property="al:ios:url" content=" ios 앱 URL" />
<meta property="al:ios:app_store_id" content="ios 앱스토어 ID" />
<meta property="al:ios:app_name" content="ios 앱 이름" />
<--Android-->
<meta property="al:android:url" content="안드로이드 앱 URL" />
<meta property="al:android:app_name" content="안드로이드 앱 이름" />
<meta property="al:android:package" content="안드로이드 패키지 이름" />
<meta property="al:web:url" content="안드로이드 앱 URL" />
```
