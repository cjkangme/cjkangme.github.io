---
layout: single
title: "[Github 블로그] 개발블로그 개선하기"
categories: etc
tags: GitHub
---

이 글은 GitHub 개발블로그를 조금 더 보기좋게 개선한 과정을 담은 글이다.
{: .notice--info}

# 개선에 앞서...

## 기술블로그 개선을 마음먹은 이유

그동안 들었던 github 블로그 강의가 끝났지만, 내눈에 아직 불만족스러운 부분이 많았다.<br>
나는 여전히 개발에 문외한이기 때문에 블로그를 뜯어 고칠 자신은 없지만 사소한 변경 정도라면 찾아보고, 이것저것 건드려보면서 가능하지 않을까 하는 생각에 마음에 안드는 사소한 것부터 고쳐보기로 하였다.

## 개선 과정을 굳이 글로 작성하는 이유

잔디는 심고 싶은데 오늘 작성할 글이 없었다 ㅋㅋㅋ<br>
사실 그동안 듣던 HTML/CSS 기초 강의를 드랍했다. 최근 인프런에서 [비전공자를 위한 개발자 취업 올인원 가이드 [통합편]](https://inf.run/Ydbq)을 듣기 시작했는데, 여기서 듣고 싶은 강의들을 많이 추천받아 듣기 시작했기 때문이다.<br>
그런데 아직 제대로 진도를 나가지 않아서 기술블로그에 마땅히 쓸 글이 없었다.<br>
주말이면 모를까 평일에 잔디를 빵꾸내고 싶지 않아서 이거라도 작성하게 되었다.

---

# 블로그 폰트 스타일 수정

블로그에 있는 거의 모든 텍스트 요소의 스타일은 `/sass/minimal-mistakes/_base.scss`에 정의되어있다.<br>
그리고 폰트의 크기나 블로그에서 사용하는 색상 등은 `/sass/minimal-mistakes/_variables.scss`에 정의되어 있고, 대부분의 스타일은 여기에 정의된 값을 참조하는 형식으로 작성되어있다.

## 링크(link) 폰트 스타일 변경

`_base.scss`를 보면 링크를 담당하는 `<a>`의 스타일은 다음과 같이 정의 되어있다.

```scss
a {
  /* 하이퍼링크 밑줄 수정 */
  text-decoration: underline $lighter-purple;
  /* 하이퍼링크 bold 삭제 */
  font-weight: 400;

  &:focus {
    @extend %tab-focus;
  }

  &:visited {
    color: $link-color-visited;
  }

  &:hover {
    color: $link-color-hover;
    outline: 0;
  }
}
```

주석에 써있듯이, 기본 스타일에는 링크 텍스트에 bold만 되어있는데, 강조표시를 제거하였다.<br>
또한 underline을 추가하고 내가 원하는 색상을 지정하였다.<br>

첫문단에 언급했듯, 폰트의 색상 등은 거의 모두 `_variables.scss`의 값을 참조하므로 이쪽을 수정해주어야 한다.
<br> 먼저 원하는 색상을 추가해주었다.

```scss
/*
   Colors
   ========================================================================== */

$gray: #7a8288 !default;
$dark-gray: mix(#000, $gray, 50%) !default;
$darker-gray: mix(#000, $gray, 60%) !default;
$light-gray: mix(#fff, $gray, 50%) !default;
$lighter-gray: mix(#fff, $gray, 90%) !default;
/* lighter-purple 색상 추가 */
$lighter-purple: #9166fc;
```

이어서 링크 텍스트가 사용하는 폰트 색상을 정의해둔 부분을 찾아 기존 정의된 것을 주석처리하고, 원하는 색상을 추가해주었다.

```scss
/* links */

/* 링크 폰트 색상 변경 */

/* $link-color: mix(#000, $info-color, 20%) !default;
$link-color-hover: mix(#000, $link-color, 25%) !default;
$link-color-visited: mix(#fff, $link-color, 15%) !default;*/
$link-color: $text-color !default;
$link-color-hover: $lighter-purple !default;
$link-color-visited: mix(#fff, $link-color, 15%) !default;
$masthead-link-color: $primary-color !default;
$masthead-link-color-hover: mix(#000, $primary-color, 25%) !default;
$navicon-link-color-hover: mix(#fff, $primary-color, 75%) !default;
```

[그러면 이렇게 보라색 밑줄과 함께 마우스를 올리면 보랗게 물드는 링크 텍스트가 완성된다.](#)

## 코드(code) 폰트 스타일 변경

link 폰트 변경때와 별 다를바 없다.<br>
이미 원하는 색상을 추가해주었으므로, `_base.scss`만 수정해주면 된다.

```scss
/* code */

tt,
code,
kbd,
samp,
pre {
  font-family: $monospace;
  /* code 폰트색상 변경 */
  color: $lighter-purple;
}
```

`그러면 이렇게 보라색의 코드가 완성된다.`

## 인용문(blockquote) 폰트 스타일 변경

사실 blockquote를 쓰는 방법은 몰라서 그냥 넣고 싶을 때 막 넣고 있다.<br>
그래도 이탤릭체는 안어울리는 것 같아 다음과 같이 `_base.scss`를 수정했다.

```scss
/* blockquotes */

blockquote {
  margin: 2em 1em 2em 0;
  padding-left: 1em;
  padding-right: 1em;
  /* 이탤릭체 제거 */
  /* font-style: italic; */
  font-style: normal;
  border-left: 0.25em solid $primary-color;
```

> 그러면 이렇게 이탤릭체가 제거된다.

## 사이드바 폰트 스타일 변경

사이드바를 추가하는 방법은 **[GitHub 블로그 사이드바 추가 및 수정하기](https://cjkangme.github.io/study/github-sidebar-navigation/)**참고<br>
{: .notice--info}

사이드바를 추가하고 보니, 사이드바의 글씨가 작은 편이어서 이부분도 고치고자 하였다<br>
`_sass/minimal-mistakes/_sidebar.scss`를 수정하면 된다.

```scss
p,
li {
  font-family: $sans-serif;
  /* 사이드바 폰트 사이즈 변경 */
  font-size: $type-size-5;
  line-height: 1.5;
}
```

기본값은 $type-size-6으로 되어있으며, `_variables.scss`에서 값을 확인할 수 있다. (0.75em - 12px)<br>
$type-size-5는 1em - 16px이다. <br>
조금 많이 커진감이 있으나, 작은 것보다는 나은 것 같아 별도로 수정하지는 않기로 했다.
