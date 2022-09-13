---
layout: single
title: "GitHub 블로그 사이드바 추가 및 수정하기"
categories: study
tags: GitHub
---

# Author Profile 활성화/비활성화

기본적으로 github 블로그에서는 다음과 같은 Author Profile을 제공한다.

<br>

![author-profile](/assets/images/github/github-author.png)

<br>

이를 비활성화하기 위해서는 posts 작성시 다음과 같이 입력해야 한다.

```YAML
---
layout: single
title: "GitHub 블로그 사이드바 추가 및 수정하기"
categories: study
tags: GitHub
author_profile: false ← 이렇게
---
```

<br>

또는 `_config.yml`의 Default를 수정함으로써 기본적으로 비활성화 시킬 수 있다.

```YAML
# Defaults
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: false ← 이렇게
```

<br><br>

# Sidebar Navigation 수정

다음과 같이 Author Profile 대신 카테고리별 및 태그별로 묶어주는 사이드바를 추가할 수 있다.

<br>

![sidebar navigation](/assets/images/github/sidebar-navigation.png)

<br>

1. `/_data/navigation.yml` 수정

```YAML
docs:
  - title: "메뉴"
    children:
      - title: "Category"
        url: /categories/
      - title: "Tag"
        url: /tags/
```

<br>

2. 각 posts 작성시 입력 또는 `_config.yml`의 Default 수정

```YAML
sidebar:
        nav: "docs"

-> navigation.yml 파일의 "docs"를 불러오겠다는 의미이다.
```

<br>

필자는 카테고리/태그 뿐 아니라 카테고리별, 태그별로 이동하고 싶어서 다음과 같이 작성하였다.
<br> 카테고리가 추가될 때마다 수동으로 navigation.yml을 수정해야한다는 것이 단점이다..

```YAML
docs:
  - title: "카테고리"
    children:
      - title: "Category"
        url: /categories/
      - title: "코딩 공부"
        url: /study/

docs:
  - title: "태그"
    children:
      - title: "Tag"
        url: /tags/
      - title: "GitHub"
        url: /tags/#github
      - title: "HTML"
        url: /tags/#html
```
