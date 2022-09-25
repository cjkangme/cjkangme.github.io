---
layout: single
title: "[Github 블로그] 폰트 변경하기"
categories: lecture
tags: GitHub
---

> **참고 자료 : 깃헙(Github) 블로그 만들기 (테디노트)** > <br>**주소 : [유튜브 링크](https://youtube.com/playlist?list=PLIMb_GuNnFwfQBZQwD-vCZENL5YLDZekr)**

<br>

# 현재 폰트 확인 방법

자신의 블로그에서 개발자 도구(크롬 기준 F12)에서 style을 확인하면 된다.
![font family](/assets/images/github/font-family.png)

- font-family에 있는 폰트가 현재 블로그에 적용되고 있는 폰트이다.

<br>

# 폰트 변경 방법 (Google Fonts 이용)

1. [Google Fonts 사이트](https://fonts.google.com/)에 접속하여 마음에 드는 폰트 검색
2. 마음에 드는 폰트와 굵기를 Style에서 선택한 뒤, @import의 본문을 복사 (복사범위 참조)
   - `/_sass/minimal-mistakes.scss`에 복사한 본문을 추가
3. CSS rules to specify families의 폰트명을 복사
   - `/_sass/minimal-mistakes/_variable.scss`에서 'system typefaces'에 복사한 폰트명 추가

---

![goggle fonts](/assets/images/github/google-fonts.png)

<br>

```scss
   # /_sass/minimal-mistakes.scss에 추가

/* Goggle Fonts */
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+KR&display=swap");
```

<br>

```scss
   # /_sass/minimal-mistakes/\_variable.scss 수정 ("Noto Sans KR" 추가)

/* system typefaces */
$serif: Georgia, Times, serif !default;
$sans-serif: -apple-system, BlinkMacSystemFont, "Noto Sans KR", "Roboto",
  "Segoe UI", "Helvetica Neue", "Lucida Grande", Arial, sans-serif !default;
```

---
