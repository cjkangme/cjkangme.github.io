---
layout: single
title: "[CSS] Box Model"
categories: lecture
tags: CSS
---

<div style="font-size: 25px">
참고자료 : 유튜브(짐코딩의 CODING GYM) - HTML/CSS 강의<br>
<a href="(https://youtube.com/playlist?list=PLlaP-jSd-nK-ponbKDjrSn3BQG9MgHSKv)">[유튜브 링크]</a>
</div>
{: .notice--info}

# Box Model이란?

![box model example](/assets/images/github/box-model-custom.png){: .align-center}

모든 HTML 요소는 웹페이지에서 일정한 공간을 차지하는데, Box Model은 CSS에서 이를 정의한 형태이다.<br>
다음과 같은 구성요소를 갖는다.<br>

|  명칭   | 설명                                                        |
| :-----: | ----------------------------------------------------------- |
| content | 텍스트, 이미지 등 HTML 요소의 실질적인 내용이 위치하는 영역 |
| padding | 테두리 안쪽의 내부 여백 영역                                |
| border  | 테두리 영역                                                 |
| margin  | 테두리 바깥의 다른 요소간의 외부 여백 영역                  |

위 사진에 요소 스타일은 다음과 같다.

```css
/* content, padding, margin, border */

div {
  width: 200px;
  height: 200px;
  padding: 20px;
  border: 10px solid #000;
  margin: 40px;
}
```

---

# Box Model 각 구성 요소의 속성

## 1. Content

- width : 콘텐츠의 가로 너비
- height : 콘텐츠의 세로 높이

> inline level element는 콘텐츠 만큼의 영역만을 가지므로, width와 height 속성이 적용되지 않는다.<br>
> 만약 inline level element에 크기를 지정하고 싶다면 `display: inline-block`으로 속성을 변경해야 한다.<br> > `inline-block` : inline 요소이므로 다음 요소가 가로로 배열되지만, 내부는 block 처럼 표시된다.

<br>

## 2. Padding

HTML 요소의 테두리 안쪽 여백을 지정하며, 각 방향별로 모두 다르게 지정할 수 있다.

> `padding-top`, `padding-right`, `padding-bottom`, `padding-left`

`padding`이라는 단축 속성이 존재하여 간단하게 지정할 수 있다.

```css
/* 상하좌우 동일한 크기로 지정 */

div {
  padding: 20px;
}
```

```css
/* 상하(20px) → 좌우(10px) 순서로 지정*/

div {
  padding: 20px 10px;
}
```

```css
/* 상(20px) → 좌우(30px) → 하(10px) 순서로 지정*/

div {
  padding: 20px 30px 10px;
}
```

```css
/* 상(10px) → 우(20px) → 하(30px) → 좌(40px) 순서로 지정*/

div {
  padding: 10px 20px 30px 40px;
}
```

<br>

## 3. Margin

HTML 요소의 테두리 바깥쪽 여백을 지정하며, padding과 동일한 방법으로 사용 가능하다.<br>

> margin만의 특이한 성질로 margin 중첩이란 것이 존재한다.<br>
> HTML요소를 세로로 배치할 경우 위쪽 요소의 margin-bottom과 아래쪽 요소의 margin-top이 중첩될 때, 둘 중 큰쪽의 margin만이 적용된다.<br>
> 가로로 배치된 요소간에는 margin 중첩이 발생하지 않는다.

<br>

## 4. Border

HTML 요소의 테두리를 설정한다.<br>
다음과 같은 속성이 존재한다.

### `border-width`

테두리의 두께를 지정한다.<br>
padding, margin과 같은 방법으로 방향별 두께를 지정할 수 있다.

### `border-style`

테두리의 형태를 지정한다.<br>
padding, margin과 같은 방법으로 방향별 두께를 지정할 수 있다.

- none : 테두리를 표시하지 않음. 주변 칸에 테두리가 있다면 테두리 표시 (우선순위 낮음)
- hidden : 테두리를 표시하지 않음. 주변 칸에 테두리가 있어도 테두리 미표시 (우선순위 높음)
- solid : 실선
- dashed : 점선
- double : 평행한 두 줄선
- 외 여러가지 속성값은 [MDN](https://developer.mozilla.org/ko/docs/Web/CSS/border-style) 참조

### `border-color`

테두리의 색상을 지정한다.<br>
padding, margin과 같은 방법으로 방향별 두께를 지정할 수 있다.

### `border`

단축 속성으로 style, width, color를 한번에 지정할 수 있다.<br>
단, 이 경우 방향별로 지정은 할 수 없다.

```css
div {
  width: 100px;
  height: 100px;
  margin: auto;
  border: 3px dashed orange;
}
```

<div style="width: 100px; height: 100px; border: 3px dashed orange; margin: auto;"></div>

### `border-radius`

테두리의 둥근 정도를 지정한다. (모든 단위 사용 가능)<br>
속성값이 50% 이상일 경우 원이 된다.

```css
div {
  width: 100px;
  height: 100px;
  margin: auto;
  border: 3px orange;
  border-radius: 50%;
}
```

<div style="width: 100px; height: 100px; border: 3px dashed orange; border-radius: 50%; margin: auto;"></div>

<br>

---

# Box Sizing

`box-sizing` 속성은 HTML 요소의 너비와 높이를 계산하는 방법을 지정한다.

## 1. content-box

`width`와 `height`의 속성값이 콘텐츠의 크기가 되며, 여기에 `padding`, `border` 속성값이 더해져 요소의 크기가 된다.<br>
`box-sizing` 속성을 정하지 않았을 때 기본값이다.

## 2. border-box

`width`와 `height`의 속성값이 요소의 크기가 되며, 콘텐츠의 크기는 여기에 맞추어 조절된다.<br>
웹페이지에서 컨텐츠를 배치할 때 계산하기 편하다는 장점이 있어 사용한다.

```css
.content-box {
    width: 100px;
    height: 150px;
    border: 10px solid blue;
    padding: 20px;
    margin: 20px;
    box-sizing: content-box;
}
/* 가로 : 100px (width) + 10px (border) + 20px (padding) = 130px */
/* 세로 : 150px (height) + 10px (border) + 20px (padding) = 180px */

.border-box {
    width: 100px;
    height: 150px;
    border: 10px solid red;
    padding: 20px;
    margin: 20px;
    box-sizing: border-box;
}
/* 가로 : 총 100px = 10px (border) + 20px (padding) + 70px (콘텐츠 가로 너비) */
/* 세로 : 총 150px = 10px (border) + 20px (padding) + 120px (콘텐츠 세로 너비) */
width, height 크기에 맞춰 콘텐츠가 조절된다
```
