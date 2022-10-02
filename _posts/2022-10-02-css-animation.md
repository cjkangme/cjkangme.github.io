---
published: true
layout: single
title: "[CSS] 애니메이션(animation)"
categories: til
tags: CSS
---

# CSS의 애니메이션

---

CSS의 애니메이션은 어떤 요소에 적용된 스타일을 다른 스타일로 부드럽게 전환되도록 하는 효과를 갖는다.<br>
Javascript를 모르더라도 간단한 애니메이션을 만들 수 있다는 장점이 있으며, 간단한 효과의 경우 렌더링에서 이점이 있다고 한다.

# 애니메이션 설정하기(@keyframes)

---

CSS 애니메이션을 사용하기 위해서는 우선 `@keyframes`를 통해 애니메이션 과정의 절차를 설정해 주어야 한다.

## 구문

### 1.from-to

```css
/* animation-name에 원하는 애니메이션 이름을 지정하면 된다. */
@keyframes animation-name {
  from {
    transform: translateX(0px);
  }
  to {
    transform: translateX(50px);
  }
}
```

from은 애니메이션의 초기상태, to는 애니메이션이 끝날 때의 상태를 지정한다.

### 2.percent

```css
@keyframes animation-name {
  0% {
    transform: translateX(0px);
  }
  50% {
    transform: translateX(50px) translateY(50px);
  }
  100% {
    transform: translateX(100px);
  }
}
```

지정한 `@keframes`가 유효하려면 최소한 0%와 100%의 값은 포함해야하며, 중간값은 어떤값을 몇 번이고 지정해도 상관이 없다.<br>

### 주의사항

- 동일한 퍼센트 값을 여러번 지정할 경우 가장 최근에 선언된 값만 유효하게 적용된다.
- `!important` 속성은 모두 무시된다.

# 애니메이션 사용하기

---

애니메이션을 적용할 요소에 `animation` 속성을 지정하면 된다.

<br>

## animation 속성의 하위 속성

### animation-name **(필수)**

`@keyframes`에서 지정한 애니메이션의 이름을 입력한다.
<br><br>

### animation-duration **(필수)**

애니메이션의 한 사이클이 얼마에 걸쳐 일어날지 지정한다.<br>
`5s`, `50ms`와 같은 시간 단위를 입력할 수 있다.<br>
값은 0 또는 양수여야 하며, 값이 0일 경우 애니메이션이 작동하지 않는다.
<br><br>

### animation-delay

웹페이지가 로드되고 처음 애니메이션이 재생될 때 까지 얼마나 시간을 지연시킬지 지정한다.<br>

> 주의 : `animation-delay`는 애니메이션이 반복될 때의 지연시간을 말하는 것은 아니다.
> 애니메이션 반복간의 지연시간을 설정하기 위해서는 `@keyframes`를 조절하는 것이 좋은 방법이다.
>
> ```css
> @keyframes rotate {
>   0% {
>     transform: rotateZ(0deg);
>   }
>   50% {
>     transform: rotateZ(180deg);
>   }
>
>   100% {
>     transform: rotateZ(180deg);
>   }
> }
> ```
>
> 위와 같은 경우 `animation-duration`이 2초일 경우, 실제 애니메이션은 1초만 재생되며, 나머지 1초는 지연시간을 갖는 것과 같다.

### animation-timing-function

애니메이션의 중간 재생 속도를 조절한다.<br>
css가 제공하는 기본값(`ease`, `ease-in`, `ease-out` 등)을 이용하는 방법이 있고, 사용자가 직접 설정할 수 있는 방법이 있다.<br>
사실 직접 설정하는 방법은 내가 다루기엔 아직 많이... 어렵기도 하다.<br>
그리고 사실 이미 능력자들이 다양한 종류의 ease-function을 많이 구현해 놓았다. 이를 참고하면 보다 쉽게 `animation-timing-function`을 조절할 수 있을 것이다.<br>
[Ceaser(CSS EASING ANIMATION TOOL)](https://matthewlein.com/tools/ceaser)<br>
[Easing Functions Cheat Sheet](https://easings.net/)

<br>

예시

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="OJZZvjO" data-user="cjkangme" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/cjkangme/pen/OJZZvjO">
  Untitled</a> by cjkangme (<a href="https://codepen.io/cjkangme">@cjkangme</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### animation-iteration-count

애니메이션의 반복 횟수를 지정한다.<br>
`infinite`를 통해 애니메이션이 무한하게 반복하도록 할 수 있다.

### animation

위와 같은 하위 속성을 한번에 설정할 수 있는 단축 속성이다.<br>
다음과 같이 사용 가능하다.

```css
div {
  animation: rotate 2s 1s infinite ease-in;
}
```

이때 처음으로 선언하는 시간이 `animation-duration`이 되고, 나중에 선언하는 시간이 `animation-delay`가 된다.

<br>

## 애니메이션 예시

마지막으로 현재 수강하고있는 노마드코더 스터디의 과제를 수행한 것을 첨부해 보려한다.<br>

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="rNvvdRN" data-user="cjkangme" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/cjkangme/pen/rNvvdRN">
  animation example</a> by cjkangme (<a href="https://codepen.io/cjkangme">@cjkangme</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>
