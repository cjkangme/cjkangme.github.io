## ๐ฆฅ `Minimal Mistakes theme customized by choiiis`

๐ **๋ธ๋ก๊ทธ ๋ฐ๋ก ๊ฐ๊ธฐ**
[`https://choiiis.github.io/`](https://choiiis.github.io/)

---

fork ํด์ ์ฌ์ฉํ์๊ธฐ ํธํ๊ฒ ๋ณ๊ฒฝํด์ ์๋ก ์ฌ๋ ค๋ด๋๋ค.  
ํธํ๊ฒ ์ฌ์ฉํ์๊ณ , fork ํ์ค ๋ `star` ํ๋๋ง ๋๋ฌ์ฃผ์ธ์ฉ :)

fork ํ ์ค์ ์ด ํ์ํ ์ฌํญ๋ค์ ์๋ ๋ด์ฉ์ ์ฐธ๊ณ ํ์ธ์!

### โช ๋ธ๋ก๊ทธ ๊ธฐ๋ณธ ์ ๋ณด ์ธํ

[_config.yml]

```yml
# plum skin ํ์ฉํ์ฌ ์์ ์ค์ ํจ. ๋ณ๊ฒฝํ๋ ค๋ฉด _sass/minimal-mistakes/skins/_plum.scss ์์ ๋ณ๊ฒฝํ๊ฑฐ๋
# ํด๋น ๋๋ ํ ๋ฆฌ ๋ด์ ๋ค๋ฅธ ํ๋ง๋ก ๋ณ๊ฒฝ ๊ฐ๋ฅ (minimal-mistakes ๊ธฐ๋ณธ ์ ๊ณต ํ๋ง)
minimal_mistakes_skin: "plum" # "default" "air", "aqua", ...

# Site Settings
locale: "ko-KR" #"en-US"
title: "Blog Name Here" # ์๋จ ํค๋์ ๋ณด์ด๋ ๋ธ๋ก๊ทธ ํ์ดํ
title_separator: "&#124;"
subtitle: # site tagline that appears below site title in masthead
name: "your name here" # ๋ธ๋ก๊ทธ ๋๋ค์ ์ค์ 
description: "OOOOO DevLog" # ๋ธ๋ก๊ทธ ์ค๋ช
url: "https://github-account.github.io" # ๋ธ๋ก๊ทธ URL
baseurl: # the subpath of your site, e.g. "/blog"
repository: "github-account/github-account.github.io" # GitHub Repo ์ด๋ฆ
# logo : # ์๋จ ํค๋์ ๋ธ๋ก๊ทธ ํ์ดํ ์์ ๋ก๊ณ  ์ถ๊ฐํ๊ณ  ์ถ์ ๊ฒฝ์ฐ ์ฌ์ฉ

---
# Site Author (Home์์ ํด๋น ๋ด์ฉ์ ์จ๊น ์ํ)
author:
  name: "your name here" # ๋ธ๋ก๊ทธ ๋๋ค์
  avatar: "/assets/images/meee.png" # ๋ธ๋ก๊ทธ ํ๋กํ ์ฌ์ง
  #   bio              : "hi all!"
  # location         : "Seoul, Korea"
  # email            : "youremailhere@xxxxxx.com"
```

### โช favicon ๋ณ๊ฒฝ

1. [https://www.favicon-generator.org/](https://www.favicon-generator.org/) ์ ์ํ์ฌ ์ํ๋ ์ด๋ฏธ์ง๋ฅผ favicon์ผ๋ก ์์ฑ
2. ์์ฑ๋ ํ์ผ `assets/images/favicon/` ๋๋ ํ ๋ฆฌ์ ์ ์ฅ  
   \*์ฃผ์) ๋ก์ปฌ ์คํ ์ ๋ณ๊ฒฝ ๋ด์ญ์ด ๋ฐ์๋์ง ์์ ์ ์์. push ํด์ ํ์ธ ํ์.
3. `_layouts/default.html`์ `github-account.github.io` ๋ถ๋ถ์ ๋ณธ์ธ ๋ธ๋ก๊ทธ URL ์๋ ฅ

```html
<link
  rel="apple-touch-icon"
  sizes="180x180"
  href="https://github-account.github.io/assets/images/favicon/apple-touch-icon.png"
/>
<link
  rel="icon"
  type="image/png"
  sizes="32x32"
  href="https://github-account.github.io/assets/images/favicon/favicon-32x32.png"
/>
<link
  rel="icon"
  type="image/png"
  sizes="16x16"
  href="https://github-account.github.io/assets/images/favicon/favicon-16x16.png"
/>
<link
  rel="manifest"
  href="https://github-account.github.io/assets/images/favicon/site.webmanifest"
/>
<link
  rel="mask-icon"
  href="https://github-account.github.io/assets/images/favicon/safari-pinned-tab.svg"
  color="#5bbad5"
/>
```

### โช ์๋จ ํค๋ ์ฐ์ธก ๋ค๋น๊ฒ์ด์ ๊ด๋ฆฌ

[_data/navigation.yml]

```yml
# main links
main:
  - title: "Home"
    url: https://your-blog-url-here/ # ๋ธ๋ก๊ทธ HOME ๋ฐ๋ก๊ฐ๊ธฐ

  - title: "About"
    url: /about/ #_pages/about.md ์ฐ๊ฒฐ

  - title: "GitHub"
    url: https://github.com/github-account # ๊นํ๋ธ ๋ฐ๋ก๊ฐ๊ธฐ (๋ณธ์ธ ๊นํ๋ธ๋ก ๋ณ๊ฒฝ)


  # ์นดํ๊ณ ๋ฆฌ ๊ธฐ๋ฅ์ด ํ์ํ๋ฉด ํ์ฑํ ํ๊ธฐ (_pages/categories-archive.md ์ฐ๊ฒฐ)
  # - title: "Categories"
  #   url: /categories/
```

### โช ์นดํ๊ณ ๋ฆฌ ์์ 

์นดํ๊ณ ๋ฆฌ์ ํญ๋ชฉ์ ์ถ๊ฐํ๊ณ  ์ถ์ ๊ฒฝ์ฐ, `_pages/categories/` ํ์์ md ํ์ผ ์ถ๊ฐ

`_pages/categories/category-categories1.md` ํ์ผ ์์ฑ ์์ (ex. category-algorithm.md)

```markdown
title: "Categories1" # ์นดํ๊ณ ๋ฆฌ ์ด๋ฆ
layout: category
permalink: /categories/categories1/ # url
author_profile: true
taxonomy: Categories1
sidebar:
nav: "categories"
```

์นดํ๊ณ ๋ฆฌ ์ด๋ฆ๊ณผ url์ `_data/navigation.yml`์ ์ถ๊ฐ

```yml
# sidebar navigation (categories)
categories:
  - title: "Categories1"
    url: /categories/categories1/
  - title: "Categories2"
    url: /categories/categories2/
  - title: "Categories3"
    url: /categories/categories3/
  - title: "Categories4"
    url: /categories/categories4/
```

2022.09.24 Update : ํ์ ์นดํ๊ณ ๋ฆฌ ํฌํจ ๋ฉ๋ด (categories-ver2 branch)  
ver2.0 ์นดํ๊ณ ๋ฆฌ ํํ ๋ฌธ์๊ฐ ์์ด์ categories-ver2 ๋ธ๋์น์ ์๋ฐ์ดํธ ํ์ต๋๋ค.  
์์๋ ํ์ด์ง ํ๋จ '๊ฐ๋ฐ ๊ธฐ๋ก' ๋ถ๋ถ์์ ํ์ธํ์ค ์ ์์ด์!

์ฐธ๊ณ ) `_data/navigation.yml`๋ง ์๋์ ๊ฐ์ด ๋ณ๊ฒฝํด์ฃผ์๋ ๋ฉ๋๋ค.

```yml
categories:
  - title: "Title1"
    children:
      - title: "Categories1"
        url: /categories/categories1/
      - title: "Categories2"
        url: /categories/categories2/
      - title: "Categories3"
        url: /categories/categories3/
      - title: "Categories4"
        url: /categories/categories4/

  - title: "Title2"
    children:
      - title: "Categories5"
        url: /categories/categories5/
      - title: "Categories6"
        url: /categories/categories6/

  - title: "Title3"
    children:
      - title: "Categories7"
        url: /categories/categories7/
```

### โช ํฌ์คํธ ์์ฑ

1. `_posts/YYYY-MM-DD-post-name-here.md` ํ์ผ ์์ฑ
2. ํฌ์คํธ์ ์ฌ์ฉํ  ์ด๋ฏธ์ง๋ `assets/images/posts_img/post-name-here/` ํ์์ ์ ์ฅ
3. ํฌ์คํธ front matter ์์ฑ

```txt
---
title: "[ํฌ์คํ ์์] ์ด๊ณณ์ ์ ๋ชฉ์ ์๋ ฅํ์ธ์"
excerpt: "๋ณธ๋ฌธ์ ์ฃผ์ ๋ด์ฉ์ ์ฌ๊ธฐ์ ์๋ ฅํ์ธ์"

categories: # ์นดํ๊ณ ๋ฆฌ ์ค์ 
  - categories1
tags: # ํฌ์คํธ ํ๊ทธ
  - [tag1, tag2]

permalink: /categories1/post-name-here/ # ํฌ์คํธ URL

toc: true # ์ฐ์ธก์ ๋ณธ๋ฌธ ๋ชฉ์ฐจ ๋ค๋น๊ฒ์ด์ ์์ฑ
toc_sticky: true # ๋ณธ๋ฌธ ๋ชฉ์ฐจ ๋ค๋น๊ฒ์ด์ ๊ณ ์  ์ฌ๋ถ

date: 2020-05-21 # ์์ฑ ๋ ์ง
last_modified_at: 2021-10-09 # ์ต์ข ์์  ๋ ์ง
---
```

4. front matter ํ๋จ์ ํฌ์คํ ๋ด์ฉ ์์ฑ

- ์ฐธ๊ณ  (\_config.yml์์ ํฌ์คํ ๊ธฐ๋ณธ ์ธํ) : comment, author_profile ๋ฑ์ ์ํ๋ฅผ ๋ณ๊ฒฝ ๊ฐ๋ฅ. ํฌ์คํ ๋ํดํธ๊ฐ

```yml
# Defaults
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: #true
      show_date: true
      comments: true
      # share: true
      related: true
      sidebar:
        nav: "categories"
```

### โช ๋๊ธ ๊ธฐ๋ฅ (utterances ์ฌ์ฉ)

utterances ๊ด๋ จํด์ ๊ตฌ๊ธ๋ง ํด๋ณด๊ณ  ์งํํ๊ธฐ๋ฅผ ์ถ์ฒ.  
๊ธฐ๋ณธ์ ์ธ ์ธํ ๋ฐฉ๋ฒ์ ์ค๋ชํ์๋ฉด,

1. ๋ณธ์ธ GitHub์ utterances์ฉ repository ์์ฑ
2. [https://github.com/apps/utterances](https://github.com/apps/utterances)์ ์ ์ํ์ฌ ์์ฑํ repo ์ ํ ํ install
3. `_config.yml` ํ์ผ ๋ณ๊ฒฝ (theme ๋ณ๊ฒฝ ์์๋ง)

```yml
comments:
  provider: "utterances"
  utterances:
    theme: "github-light" # "github-dark"
    issue_term: "pathname" # pathname์ post์ markdown ํ์ผ ์ด๋ฆ์ผ๋ก ์ฐ๊ฒฐ๋จ
```

4. `_includes/comments-providers/utterances.html` ํ์ผ ์์ฑ

```yml
# ๋ณธ์ธ ๊นํ๋ธ ์์ด๋์ ์์ฑํ ๋ ํ์งํ ๋ฆฌ ์๋ ฅ
script.setAttribute('repo', 'github-account/repository-name');
# ์ ํํ ๊นํ๋ธ ํ๋ง ์๋ ฅ
script.setAttribute('theme', '{{ site.comments.utterances.theme | default: "github-light" }}');
```

### โช Google Analytics ์ฐ๊ฒฐ

[https://analytics.google.com/analytics/web/](https://analytics.google.com/analytics/web/)์์ ์ ์ํ์ฌ ์ฐ๊ฒฐ

```yml
# Analytics
analytics:
  provider: "google-gtag"
  # false (default), "google", "google-universal", "google-gtag", "custom"
  google:
    tracking_id: "your tracking id here" # ๋ณธ์ธ์ tracking id ์๋ ฅ
    anonymize_ip: # true, false (default)
```

### โช Goolge Search Console ์ฐ๊ฒฐ

๊ตฌ๊ธ์ ๋ด ๊ฒ์๋ฌผ์ด ๋ณด์ด๊ฒ ํ๋ ค๋ฉด search console๊ณผ ์ฐ๊ฒฐ์ด ํ์
[https://search.google.com/search-console/about](https://search.google.com/search-console/about)์์ ์ ์ํ์ฌ ๋๋ฉ์ธ ๋ฑ๋ก

1. ๋๋ฉ์ธ ๋ฑ๋ก ์ ๊ตฌ๊ธ์์ ์ ๊ณตํ๋ `google~~.html` ํ์ผ ๋ฃจํธ ๋๋ ํ ๋ฆฌ์ ์๋ก๋
2. `jekyll-sitemap` ํ๋ฌ๊ทธ์ธ ์ค์น (๊ตฌ๊ธ๋ง ์ถ์ฒ)

```bash
sudo gem install jekyll-sitemap
```

3. `_config.yml` ํ์ผ์ plugins์ jekyll-sitemap ์์ผ๋ฉด ์ถ๊ฐ

```yml
# Plugins (previously gems:)
plugins:
  - jekyll-sitemap
```

4. ๋ฃจํธ ๋๋ ํ ๋ฆฌ์ `robots.txt` ์์ฑ

```txt
User-agent: *
Allow: /

Sitemap: https://github-account.github.io/sitemap.xml
```

### โช ๋ค์ด๋ฒ ๊ฒ์ ๋ฑ๋ก (์์น์ด๋๋ฐ์ด์ )

[https://searchadvisor.naver.com/](https://searchadvisor.naver.com/)์ ์ ์ํ์ฌ ์ฌ์ดํธ ๋ฑ๋ก  
๋ฃจํธ ๋๋ ํ ๋ฆฌ์ `naver~~~~.html` ์ถ๊ฐ

- ์ฐธ๊ณ ํ ๋งํ ๋ธ๋ก๊ทธ๊ฐ ์์ด์ ๋งํฌ ๊ฑธ์ด๋๊ฒ ์ต๋๋ค.
  [https://yenarue.github.io/tip/2020/04/30/Search-SEO/#%EB%84%A4%EC%9D%B4%EB%B2%84-naver](https://yenarue.github.io/tip/2020/04/30/Search-SEO/#%EB%84%A4%EC%9D%B4%EB%B2%84-naver)

### โช ํฐํธ ๋ณ๊ฒฝ

1. `assets/css/main.scss`์ import๋ font-face ๋ฐฉ์ ์ค ์ ํํ์ฌ ํฐํธ ์ถ๊ฐ

```scss
@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");

@font-face {
  font-family: "RIDIBatang";
  font-weight: normal;
  src: url(/assets/css/fonts/RIDIBatang.otf);
}
```

2. `_sass/minimal-mistakes/_variables.scss`์์ ํฐํธ ์ค์ 

```scss
$serif: Georgia, Times, serif !default;
$sans-serif: -apple-system, BlinkMacSystemFont, "Apple SD Gothic Neo",
  "Montserrat", "Pretendard", "Merriweather", sans-serif !default;
$monospace: "Fira Mono", "Pretendard", Monaco, Consolas, "Lucida Console",
  monospace !default;
```

### โช About ํ์ด์ง ์์ฑ

์๋จ ๋ค๋น๊ฒ์ด์์ `About` ํญ์ `_pages/about.md`๋ก ์ฐ๊ฒฐ. ํด๋น ํ์ผ์ ๋ด์ฉ ์์ฑ

```txt
---
title: "Hi all! I'm OOOOOO๐๐ป"
permalink: /about/
layout: single
comments: false
---

๋ณธ์ธ ์๊ฐ ์ฌ๊ธฐ์ ์๋ ฅ
```

_๋ฌธ์์ฌํญ ๋๋ ์์  ์์ฒญ์ ๋ธ๋ก๊ทธ์ ๋๊ธ ๋จ๊ฒจ์ฃผ์๊ฑฐ๋ ์ด๋ฉ์ผ๋ก ์ฐ๋ฝ์ฃผ์ธ์!_

---

### ๊ฐ๋ฐ ๊ธฐ๋ก

[VER1.0]
![choiiis github blog main](/assets/images/posts_img/readme/blog-main-ver1.png)

[VER2.0]
![choiiis github blog main](/assets/images/posts_img/readme/blog-main-ver2.png)

- logo ๋ณ๊ฒฝ
- ์นดํ๊ณ ๋ฆฌ ๋์์ธ ๋ณ๊ฒฝ
- font family, size ๋ณ๊ฒฝ
- ๋ฉ์ธ ์ปฌ๋ฌ ๋ณ๊ฒฝ

[VER2.1]
![choiiis github blog main](/assets/images/posts_img/readme/ver2_1_main.png)
