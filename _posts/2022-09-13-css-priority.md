---
layout: single
title: "[CSS] 스타일 적용 방법 및 우선순위"
categories: study
tags: CSS, HTML
---

> 참고자료 : 유튜브(짐코딩의 CODING GYM) - HTML/CSS 강의
>
> **주소 : [유튜브 링크](https://youtube.com/playlist?list=PLlaP-jSd-nK-ponbKDjrSn3BQG9MgHSKv)**

<br>

# HTML 스타일 적용방법 3가지

## 1. 인라인 스타일

HTML의 태그 내부에 style 속성을 지정하여 적용하는 것
<br> 다수의 스타일을 적용하려면 세미콜론(;)을 이용하여 적용할 수 있다.

```html
<!-- 인라인 스타일 예시 -->

<h2 style="color: blue">파란색 제목</h2>
<div style="width: 100px; height: 100px; border: solid 1px red">
  빨간색 박스
</div>
```

> <h2 style="color: blue">파란색 제목</h2>
> <div style="width: 100px; height: 100px; border: solid 1px red">빨간색 박스</div>

---

## 2. 내부 스타일 (Internal style sheets)

HTML 문서에서 `<head>` 내부의 `<style>` 태그를 통하여 스타일 적용

```html
<head>
  <style>
    h2 {
      color: blue;
    }

    .contents {
      width: 100px;
      height: 100px;
      border: solid 1px red;
    }
  </style>
</head>
```

---

## 3. 외부 스타일 (External style sheets)

문서 외부에 `.css` 확장자를 갖는 스타일 시트를 생성한 후, `<link>`태그를 이용해 연결하여 스타일을 적용하는 방식
<br> 링크만 해주면 여러 문서(페이지)에서 공통으로 사용할 수 있다.
<br> 즉 관리가 쉽고, 용량이 적어 가장 많이 사용하는 방식이다.

### 적용방법

1. 외부에 `.css` 파일 생성
2. html 문서에서 링크

```html
<!-- html 문서 <head> 내 입력 -->

<link rel="stylesheet" href="./style.css" />
```

3. css 문서는 style 태그 없이 곧바로 스타일 규칙을 적용하면 된다.

---

# 스타일의 종류와 적용 우선순위

스타일은 출처에 따라 3가지로 분류가 가능하다.

## 1. 브라우저 스타일(Browser Style)

각 브라우저별로 기본적으로 지정하고 있는 스타일 시트
<br> URL을 강조하는 스타일이나, 버튼 형태 등 다양한 곳에 적용된다.
<br> 웹페이지 제작 시 `<button>`과 같은 태그 사용을 꺼리는 이유는 브라우저별로 모양이 다 달라지기 때문이다.

## 2. 사용자 스타일(User Style)

웹페이지를 방문하는 일반 사용자들의 설정에 따른 스타일 시트
<br> 예를 들어 저시력자는 글을 쉽게 읽기 위해 윈도우의 '고대비' 설정을 사용할 수 있다.

## 3. 제작자 스타일(Author Style)

웹페이지를 제작하는 제작자가 직접 작성한 스타일 시트
<br> 즉 HTML/CSS로 작성하는 모든 스타일은 제작자 스타일이다.

---

## 적용 우선순위

> **사용자 !important > 제작자 !important > 제작자 > 사용자 > 브라우저**

- !important는 속성값 뒤에 추가 작성함으로써 선언할 수 있다.

```css
/* a 태그에 지정된 스타일과는 상관없이 배경색이 빨강으로 출력된다. */

a {
  text-decoration: none;
  background: skyblue;
}

* {
  background: red !important;
}
```

- 적절치 않은 important 사용은 폭포(cascading)의 흐름을 깰 수 있으니 주의하여 사용하여야 한다.
