---
layout: single
title: "[HTML] 이미지 & 멀티미디어 태그"
categories: study
tags: HTML
---

> ### 참고자료 : 유튜브(짐코딩의 CODING GYM) - HTML/CSS 강의
>
> ### **주소 : [유튜브 링크](https://youtube.com/playlist?list=PLlaP-jSd-nK-ponbKDjrSn3BQG9MgHSKv)**

<br>

# 이미지 태그

### 이름 그대로 페이지에 이미지를 삽입하는 역할의 태그

<br>

## `<img>`의 속성

- src : **(필수)** 이미지의 경로를 지정
- alt : 이미지를 설명하는 텍스트
  - 스크린 리더에서 이미지 대신 읽어주거나 이미지 로드 실패시 출력되는 텍스트로, 웹 접근성 향상에 기여한다. 선택이지만 되도록 입력하는 것이 좋다.
- height : 픽셀 단위의 이미지 높이를 지정 - 단위 없는 정수
- width : 픽셀 단위의 이미지 너비를 지정 - 단위 없는 정수

<br>

height, width의 경우 둘 중 하나만 입력하면 나머지는 이미지 비율에 맞춰 알아서 지정된다.
<br>단, 둘 다 지정할 경우 비율에 상관 없이 이미지 크기가 고정된다. (찌그러짐 발생)

<br>

## 절대경로 vs 상대경로

### 이미지나 URL 등 경로를 지정할 때, 경로에는 크게 두 종류가 있다.

<br>

1. 절대경로 : 현재 디렉토리에 상관이 없는 절대적인 고유한 경로
   - URL : https://www.naver.com
   - 루트디렉토리 부터의 경로 (`/` - 슬래시는 루트디렉토리를 의미, 예외로 윈도우는 `\` - 역슬래시 사용)

<br>

2. 상대경로 : 현재 디렉토리를 기준으로 한 경로
   - `.` : 현재 디렉토리를 의미
   - `..` : 상위 디렉토리를 의미

```
예시

image.jpg 또는 ./image.jpg - 동일 디렉토리
../images/image.jpg - 상위 디렉토리
images/image.jpg - 하위 디렉토리
```

<br><br>

# 오디오 태그

### HTML 문서에 소리 콘텐츠를 넣는 역할의 태그

<br>

## `<audio>`의 속성

- src : **(필수)** 오디오 소스의 경로를 지정
- type : 오디오 소스의 코덱 종류를 브라우저에 알려주는 역할
- source : 다수의 오디오 소스를 지정할 때 사용
  - 브라우저마다 지원하는 코덱이 다르기 때문에, 여러 종류의 소스를 지정하면 브라우저가 가장 적절한 소스를 선택하여 재생한다.
- controls : 페이지에 재생, 일시정시, 볼륨조절 등이 가능한 재생바를 추가
- autoplay : 페이지 실행시 오디오 소스 자동 재생
  - 크롬 등 일부 브라우저에서는 사용자가 별도로 설정하지 않으면 자동재생을 허용하지 않는다.
- loop : 오디오 소스 반복 재생
- muted : 오디오 소스의 기본값을 음소거 상태로 함
- preload : 사용자가 오디오 소스를 재생하기 전에 미리 소스를 다운로드

<br>
예시
```HTML
<audio muted>
    <source src="../assets/audio/audio.mp3" type="mp3" />
    <source src="../assets/audio/audio.wav" type="wav" />
    지원하지 않는 브라우저입니다.
</audio>
```

<br><br>

# 비디오 태그

### HTML 문서에 영상 콘텐츠를 넣는 역할의 태그

<br>

## `<video>`의 속성

- audio에서 사용하는 속성 일체
- width, height : 비디오 콘텐츠의 너비와 높이 지정
- poster : 비디오가 재생되기 전까지 화면에 사용될 포스터 이미지 지정 (일종의 섬네일)
  - poster="`경로명`"

<br>
예시
```HTML
<video>
    width="720"
    controls
    loop
    poster="../assets/images/swan.jpg"
    src="../assets/videos/video.mp4"
    type="video/mp4"
</video>
```

<br><br>

# 하이퍼링크 태그(앵커 태그)

### 다른 페이지로의 이동, 같은 페이지의 특정 위치 이동, 파일 다운로드 링크 연결, 이메일보내기 등 다른 URL로 연결할 수 있는 태그

<br>

## `<a>`의 속성

- href : **(필수)** 앵커 태그가 가리키는 URL 지정
- target : 하이퍼링크를 클릭했을 때 URL을 표시할 위치 정의
  - \_self : (기본값) 현재 창에서 URL 표시
  - \_blank : 새 탭에서 URL 표시
  - \_parent : 현재 브라우저 부모 페이지에 연결된 URL 표시, 부모 페이지가 없는 경우 \_self와 동일
  - \_top : 최상단 브라우저에 표시, 부모 페이지가 없는 경우 \_self와 동일
- rel : 연결된 URL과 현재 문서간의 관계를 정의
  - null : (기본값) 아무것도 정의 되지 않은 상태
  - noreferrer : 이동할 URL에 현재 페이지의 정보를 보내지 않음 (악용 방지)
  - 그 외 관계속성은 **[MDN 홈페이지](https://developer.mozilla.org/ko/docs/Web/HTML/Link_types)** 참조

<br>

### 외부페이지 이동

```HTML
<a rel="noreferrer" target="_blank" href="https://www.naver.com">네이버</a>와 <a href="https://www.google.com">구글</a>은 포털 사이트 이다.
```

> <a rel="noreferrer" target="_blank" href="https://www.naver.com">네이버</a>와 <a href="https://www.google.com">구글</a>은 포털 사이트 이다.

<br>

### 내부페이지 이동

```HTML
<a href="#target">target으로 이동</a>
```

> <a href="#target">target으로 이동</a>

→ ID명이 target으로 설정된 같은 페이지의 콘텐츠로 이동한다.

<br>

### 이메일 전송

```HTML
<a href="mailto: nureongi0214@gmail.com">이메일 보내기</a>
```

> <a href="mailto: nureongi0214@gmail.com">이메일 보내기</a>

→ mailto를 통해 이메일 전송이 가능하다.
