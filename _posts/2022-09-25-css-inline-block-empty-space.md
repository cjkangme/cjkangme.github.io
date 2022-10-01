---
published: true
layout: single
title: "[CSS] inline-block의 여백 제거"
categories: til
tags: CSS
---

# inline-block 요소 사이에 발생하는 여백

block 요소를 가로로 정렬하는 방법 중 하나는 바로 display 속성을 inline-block으로 지정하는 것이다.<br>
그러나, inline-block으로 레이아웃을 만들면 다음과 같은 문제가 발생한다.<br>

```css
/* css */
div {
  display: inline-block;
  border: 1px solid black;
  width: 50px;
  height: 50px;
  margin: 0;
  text-align: center;
  line-height: 50px;
}
```

```html
<!-- HTML -->
<div>A</div>
<div>B</div>
<div>C</div>
```

![inline-block-여백](/assets/images/2022-09/inline-block-1.png){: .text-center}

분명히 margin을 0으로 설정했는데 요소 사이에 정체불명의 여백이 생긴다.<br>
놀라운 것은, HTML 코드를 다음과 같이 작성하면 여백이 생기지 않는다.

```html
<!-- HTML -->
<div>A</div>
<div>B</div>
<div>C</div>
```

![inline-block-여백없음](/assets/images/2022-09/inline-block-2.png){: .text-center}

이는 코드상의 윗줄과 아랫줄 사이에 엔터를 쳤기 때문에, 여기에 해당하는 font-size만큼 공간을 차지하기 때문에 발생한다.<br>
이걸로 엄청 골머리를 썩었는데 참으로 어이가 없는 이유였다...<br>
이를 방지하기 위해서는 크게 2가지 방법이 있다.

## 1. 음수 margin

기본 font-size인 16px을 기준으로 발생하는 여백은 4px이다.<br>
때문에 font-size의 변경이 없다면 margin-right 속성을 음수로 설정하여 여백을 지울 수 있다.

```css
/* css */
div {
  display: inline-block;
  border: 1px solid black;
  width: 50px;
  height: 50px;
  margin: 0;
  margin-right: -6px;
  text-align: center;
  line-height: 50px;
}
```

```html
<!-- HTML -->
<div>A</div>
<div>B</div>
<div>C</div>
```

![inline-block-margin](/assets/images/2022-09/inline-block-3.png)

그런데 이 방법은, 글씨크기에 따라 달라지기도 하고, 정렬할 요소의 width가 서로 다르면 정렬이 틀어질 수 있다.<br>
말그대로 임시조치인 셈이다.<br>
조금 더 나은 방법으로 font-size를 조절하는 방법이 있다.

## 2. font-size 조절

정렬할 inline-block요소를 부모 태그로 한번 감싸고, 부모태그에 font-size를 0으로 설정하는 방법이 있다.<br>
이렇게하면 엔터가 들어가도 공간을 전혀차지하지 않기 때문에 여백이 지워지게 된다.<br>
물론 자식요소에는 다시 font-size를 설정해주어야 한다.

```CSS
      main {
        font-size: 0;
      }
      div {
        display: inline-block;
        border: 1px solid black;
        width: 50px;
        height: 50px;
        margin: 0;
        text-align: center;
        line-height: 50px;
        font-size: 1rem;
      }
```

```HTML
<main>
  <div>A</div>
  <div>B</div>
  <div>C</div>
</main>
```

![inline-block-font-size](/assets/images/2022-09/inline-block-4.png)

다소 뒤늦게 안 사실인데, 사실 inline-block은 반응형 디자인을 전혀 지원하지 않기 때문에 레이아웃을 디자인하는데는 별로 좋지 않다고 한다. (구닥다리 방식이라고...)<br>
아직 flexbox 속성을 배우질 않았는데, 관련 강의를 찾아보고 활용해봐야겠다.
