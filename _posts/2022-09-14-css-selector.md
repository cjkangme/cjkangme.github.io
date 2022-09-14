---
layout: single
title: "[CSS] CSS 속성 - 선택자"
categories: study
tags: CSS
---

> 참고자료 : 유튜브(짐코딩의 CODING GYM) - HTML/CSS 강의
>
> **주소 : [유튜브 링크](https://youtube.com/playlist?list=PLlaP-jSd-nK-ponbKDjrSn3BQG9MgHSKv)**

<br>

# 기본선택자

## 1. 전체선택자 `*`

문서 내 모든 요소를 선택한다.
<br> 주로 아래와 같이 브라우저 스타일을 초기화 하는데 주로 사용한다.

```css
/* 문서 내 모든 요소에 스타일 적용 */

* {
  font-family: "Noto Sans KR", sans-serif;
  text-decoration: none;
  color: black;
  list-style: none;
  margin: 0px;
  padding: 0px;
  border-collapse: collapse;
}
```

## 2. Type 선택자 `<요소명>`

요소 이름으로 요소를 선택한다.

```css
/* 모든 <h1> 태그에 스타일 적용 */

h1 {
  font-size: 48px;
  font-weight: 100;
}
```

## 3. Class 선택자 `.<Class명>`

시작 태그에 선언된 class명으로 요소를 선택한다.

```css
/* class="contents"인 태그 콘텐츠에 해당 스타일 적용 */

.contents {
  color: blue;
  font-weight: bold;
}
```

## 4. Id 선택자 `#<Id명>`

시작 태그에 선언된 id를 통해 요소를 선택한다.<br>

- id는 class와 달리 전 문서에서 단 하나의 요소에만 사용할 수 있다. (고유값)

```css
/* id="title"인 태그 콘텐츠에 해당 스타일 적용 */

#title {
  font-size: 40px;
  color: black;
  background: yellowgreen;
}
```

---

# 속성 선택자 `[속성명]`

요소 내의 속성값을 바탕으로 요소를 검색하여 선택한다.<br>
다양한 조건으로 요소를 검색할 수 있다.

```css
/* target 속성을 갖는 모든 요소에 적용 */
[target] {
  font-size: 30px;
}

/* <h2> 태그의 class 속성을 갖는 요소에 적용 */
h2[class] {
  font-size: 30px;
}

/* <a> 태그의 href 속성값이 "https://cjkangme.github.io"과 일치하는 요소에 적용 */
a[href="https://cjkangme.github.io"]
{
  font-size: 30px;
}

/* <a> 태그의 href 속성값이 "https://cjkangme.github.io"과 일치하는 요소에 적용 */
a[href="https://cjkangme.github.io"]
{
  font-size: 30px;
}

/* <a> 태그의 href 속성값이 "github"를 포함하는 요소에 적용 */
a[href*="github"] {
  font-size: 30px;
}

/* <a> 태그의 href 속성값이 "https"로 시작하는 요소에 적용 */
a[href^="https"] {
  font-size: 30px;
}

/* <a> 태그의 href 속성값이 ".io"로 끝나는 요소에 적용 */
a[href$=".io"] {
  font-size: 30px;
}

/* <div> 태그의 class 속성값 중 "logo"를 단어로 갖는 요소에 적용 */
/* 검색할 단어가 띄어쓰기로 분리되어 있어야 한다. - "main-logo" (X) "main logo" (O) */
div[class~="logo"] {
  font-size: 30px;
}
```

---

# 그룹선택자

선택자를 쉼표로 구분하면 여러 선택자를 선택하여 한꺼번에 스타일을 정의할 수 있다.

```scss
/* tt, code, kbd, samp, pre 요소를 한꺼번에 선택하여 적용 */

tt,
code,
kbd,
samp,
pre {
  font-family: $monospace;
  color: $lighter-purple;
}
```

---

# 결합자

## 1. 자손 결합자 ` ` : `A B`

A 선택자 하위에 있는 모든 B 선택자를 선택한다.

```css
div span{
    color: pink;
}

<div>
    <span></span> /* 선택 */
    <div></div>
    <p>
        <span></span> /* 선택 */
        <span></span> /* 선택 */
    </p>
</div>
```

### 2. 자식 결합자 `>` : `A > B`

A 선택자의 직접적 자식인 B 선택자만을 선택한다. (보다 하위태그는 선택하지 않음)

```css
div > span{
    color: pink;
}

<div>
    <span></span> /* 선택 */
    <div></div>
    <p>
        <span></span>
        <span></span>
    </p>
</div>
```

### 일반 형제 결합자 `~` : `A ~ B`

A 선택자보다 아랫줄에 위치하는 형제 B 선택자를 모두 선택한다.

```css
/* 일반 형제 결합자 */

p ~ span {
  color: red;
}

<div>
    <span>span1</span>
    <p>p1</p>
    <code>code1</code>
    <span>span2</span> /* 선택 */
    <code>code2</code>
    <p>p2</p>
    <span>span3</span> /* 선택 */
</div>
```

### 인접 형제 결합자 `+` : `A + B`

A 선택자 바로 다음줄에 위치하는 형제 B 선택자만을 선택한다.

```css
/* 인접 형제 결합자 */

p + code {
  color: red;
}

<div>
    <span>span1</span>
    <p>p1</p>
    <code>code1</code> /* 선택 */
    <span>span2</span>
    <code>code2</code>
    <p>p2</p>
    <code>code3</code> /* 선택 */
    <span>span3</span>
</div>
```

---

# 선택자간 우선순위

결합자를 사용할 경우에는 선택자 방식의 정해진 점수를 합산하여 우선 순위를 결정한다.
<br>우선순위가 같을 경우 아랫줄에 선언된 스타일이 우선 적용된다.

| 선택자        | 적용 예시                |      우선순위       |      점수 산정      |
| ------------- | ------------------------ | :-----------------: | :-----------------: |
| !important    | 속성="속성값" !important | 속성 기준 가장 우선 | 속성 기준 가장 우선 |
| 인라인 스타일 | style=""                 |          1          | 요소 기준 가장 우선 |
| id 선택자     | #이름                    |          2          |         100         |
| class 선택자  | .이름                    |          3          |         10          |
| 속성 선택자   | 요소명[속성명]           |          3          |         10          |
| 가상 클래스   | a:hover                  |          3          |         10          |
| 가상 요소     | span::after              |          3          |         10          |
| Type 선택자   | 태그명                   |          4          |          1          |
| 전체 선택자   | \*                       |          5          |          0          |
