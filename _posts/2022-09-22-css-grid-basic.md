---
layout: single
title: "[CSS] 그리드(Grid) 기초 내용"
categories: study
tags: CSS
---

CSS의 Grid는 Flex와 함께 웹페이지의 레이아웃을 구성하기 위해 사용한다.<br>
Flex와 차이점이 있다면, Flex는 가로 또는 세로 중 한 방향으로만의 레이아웃 정렬에 사용되지만,<br>
Grid는 가로, 세로 모든 방향으로 레이아웃을 정렬할 수 있다.

<br>

# 그리드의 구성요소

그리드 컨테이너(Grid Container)는 레이아웃에 들어갈 요소를 모두 담는 부모 요소를 말한다.
그리드 아이템(Grid Item)은 레이아웃을 구성하는 각각의 자식 요소를 말한다.

```html
<div class="grid-container">
  <div class="grid-item">A</div>
  <div class="grid-item">B</div>
  <div class="grid-item">C</div>
  <div class="grid-item">D</div>
  <div class="grid-item">E</div>
  <div class="grid-item">F</div>
  <div class="grid-item">G</div>
</div>
```

<br>

# 그리드 컨테이너

## 그리드 생성

그리드 레이아웃의 시작은 그리드 컨테이너를 만드는 것으로 시작한다.<br>
부모 요소에 `display` 속성을 선언하면 된다.

```css
.grid-container {
  display: grid;
}
```

- 생성 가능 그리드
  - `grid` : block level container
  - `grid-inline` : inline level container

## 그리드 템플릿(grid-template-rows/grid-template-columns)

그리드의 행/열의 수, 셀의 크기를 선언하는 속성이다.

- grid-template-rows : 행의 배치를 결정한다.
- grid-template-columns : 열의 배치를 결정한다.

가장 기본적인 사용법은 배치된 아이템 수에 맞추어, 순서대로 각각의 아이템의 크기를 지정하는 것이다.

```css
/* A 아이템은 100px, B,C 아이템은 각각 남은 콘테이너 공간을 1:2로 나누어 차지함*/
#grid-container {
  display: grid;
  grid-template-columns: 100px 1fr 2fr;
}
```

이때, `linename`을 별도로 설정할 수 있다.<br>
`linename`은 추후 아이템의 배치와 셀 병합에 사용할 수 있으나, 별로 권장되지는 않는다.

```css
/* 공백을 활용하여 복수의 linename을 설정할 수도 있다.
당연히 linename에 공백이 들어가면 안된다.*/
#grid-container {
  display: grid;
  grid-template-columns: [col-start] 100px [col-2 col-middle] 1fr [col-end] 2fr;
}
```

### repeat 함수

그리드 템플릿 속성은 특별한 함수를 사용할 수 있다.<br>
그 중 하나가 바로 `repeat` 함수이다.

- `repeat(반복횟수, 아이템 크기)`

```css
/* 두 값은 동일하다 */
#grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-columns: 1fr 1fr 1fr;
}
```

```css
/* 두 값은 동일하다 */
#grid-container {
  display: grid;
  grid-template-columns: 2fr repeat(2, 50px) repeat(2, 1fr);
  grid-template-columns: 2fr 50px 50px 1fr 1fr;
}
```

또한 `repeat`는 두 가지 인자를 사용할 수 있다.

- auto-fill : grid container의 너비/높이를 초과하지 않도록 자동으로 트랙의 수(repeat 반복횟수)를 지정한다. 아이템을 채우고 남는 공간이 있어도 채우지 않는다.
- auto-fit : 기본적으로 auto-fill과 동일하다. 아이템을 채우고 남는 공간이 있으면 가능한만큼 아이템의 크기를 늘려 채운다.

![auto-fill](/assets/images/github/grid2.png)

```css
/* auto-fill */
#grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(50px, auto));
}
```

---

![auto-fit](/assets/images/github/grid3.png)

```css
/* auto-fit */
#grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(50px, auto));
}
```

이 두가지 인자는 주로 `minmax` 함수와 함께 사용된다.

### minmax 함수

- minmax(최소값, 최대값)

```css
/* 두 값은 동일하다 */
#grid-container {
  display: grid;
  grid-template-columns: repeat(3, minmax(50px, auto));
  grid-template-columns: minmax(50px, auto) minmax(50px, auto) minmax(50px, auto);
}
```

auto를 단순하게 설명하면 아이템의 콘텐츠 크기에 따라 아이템의 크기도 자동으로 늘어나게 해준다.
<br>또한 auto-fit과 함께 사용하면 빈공간에 맞춰 자동으로 늘어나기도 한다.<br>

[MDN 문서](https://developer.mozilla.org/en-US/docs/Web/CSS/minmax)에 의하면 다음과 같이 다양하게 설정할 수 있다.<br>
단, max값이 min값보다 작아지면 max값은 무시된다.

```css
/* <inflexible-breadth>, <track-breadth> values */
minmax(200px, 1fr)
minmax(400px, 50%)
minmax(30%, 300px)
minmax(100px, max-content)
minmax(min-content, 400px)
minmax(max-content, auto)
minmax(auto, 300px)
minmax(min-content, auto)

/* <fixed-breadth>, <track-breadth> values */
minmax(200px, 1fr)
minmax(30%, 300px)
minmax(400px, 50%)
minmax(50%, min-content)
minmax(300px, max-content)
minmax(200px, auto)

/* <inflexible-breadth>, <fixed-breadth> values */
minmax(400px, 50%)
minmax(30%, 300px)
minmax(min-content, 200px)
minmax(max-content, 200px)
minmax(auto, 300px)
```

<br>

## 그리드 셀의 이름(grid-template-areas)

각 그리드 셀에 이름을 지정한다. 생성한 이름은 아이템을 그리드에 배치할 때(`gird-area` 속성) 참조할 수 있다.<br>
이름을 지정하는 방법은 문자를 공백으로 구분하여 나열하고, 각 행은 큰 따옴표 또는 작은 따옴표로 구분하면 된다.<br>
이름이 없는 셀을 표현할때는 마침표(`.`)를 사용한다.

```css
#grid-container {
  grid-template-areas:
    "a b b b"
    "c d d d"
    "e e e e";
}
```

이 경우 `grid-area: b;`가 선언된 아이템은 상단의 3칸에 배치되고,<br>
`grid-area: c`가 선언된 아이템은 중앙 왼쪽의 1칸에 배치된다.

<br>

## 그리드 셀 사이의 간격(gap/row-gap/column-gap)

그리드의 각 셀 사이의 간격을 지정한다.

```css
#grid-container {
  /* 행 사이의 간격 */
  row-gap: 10px;

  /* 열 사이의 간격 */
  column-gap: 20;

  /* 행과 열 사이의 간격 */
  gap: 10px;

  /* 행 10px, 열 20px (행-열 순서) */
  gap: 10px 20px;
}
```

<br><br>

처음엔 생활코딩에서 가볍게 다루길래 나머지 내용을 공부하려 했는데,<br>
생각보다 배울 분량이 너무 방대해서 이 정도로 마무리 해야할 것 같다.<br>
웹 페이지를 배껴서 만들던지 해서 실전에서 써보고, 어려웠던 내용을 추가로 작성하기로 하자.
