---
layout: single
title: "Github 블로그 만들기 2일차"
categories: lecture
tags: GitHub
---

### **참고 자료 : 깃헙(Github) 블로그 만들기 (유튜브)**

### <br>**주소 : [유튜브 링크](https://youtube.com/playlist?list=PLIMb_GuNnFwfQBZQwD-vCZENL5YLDZekr)**

# EP03 ~ 04. 블로그 설정 변경

### 모든 블로그 관련 설정은 '\_config.yml'에서 변경 가능

### 미니멀 미스테이크 홈페이지 (**[링크](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)**)의 Configuration을 통해 다양한 설정 및 설정 방법 확인 가능

<br>

## 1. 게시글 작성일 표시

### 기본값은 read_time으로 되어있어 게시글 작성일이 아닌, 읽기 소요 시간이 블로그에 표시된다.

1. '\_config.yml'문서 최하단의 Defaults 옵션 수정
   - show_data : true (게시글 작성일 표시)
   - read_time : false (읽기 소요시간 미표시) → 원할 경우
2. 문서에 'data_format'을 입력하여 설정
   - 예시) `date_format: "%Y-%m-%d"` → '2022-09-09'
   - date_format 입력 방법 : **[치트시트 링크](https://www.shortcutfoo.com/app/dojos/ruby-date-format-strftime/cheatsheet)**

- 읽기 소요시간을 조절하고 싶다면 words_per_minute 값을 수정할 수 있다. (기본값 : 200자 당 1분)

### <br>설정 화면 예시

```
# Defaults
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: false
      show_date: true
      comments: true
      share: true
      related: true
```

<br><br>

## 2. 댓글 기능

### 미니멀 미스테이크 홈페이지 Configuration의 'Comments' 문단 참조

<br>

### 2-1. Disqus 가입 및 설정

1. Disqus(**[홈페이지 링크](https://disqus.com/)**) 가입
2. 페이지 우상단의 초상화 클릭하여 'Settings' 선택
3. 페이지 우상단의 톱니바퀴 아이콘 클릭하여 'Add Disqus to site' 선택
4. 페이지 최하단 'GET STARTED' 버튼 클릭 및 'I want to install Disqus on my site' 선택
5. 정보 입력 및 라이센스 선택 (무료버전 존재)
6. 플랫폼 선택 화면에서 'Jekyll' 선택 후 블로그 URL 입력
   - 예) `https://www.cjkangeme.github.io`

### 2-2. 웹사이트 적용

1. '\_config.yml'에서 Defaults 문단의 comments 주석처리 제거
2. comments 설정 변경
   - provider: "disqus"
   - disqus shortname : disqus의 Admin → Select a specific site → 적용한 사이트 클릭 → shortname 확인(복사)

### <br>설정 화면 예시

```
comments:
  provider: "disqus"
  disqus:
    shortname: "cjkangme"
```

<br><br>

## 3. 구글 애널리틱스

### 미니멀 미스테이크 홈페이지 Configuration의 'Analytics' 문단 참조

<br>

1. 구글 애널리틱스(**[홈페이지 링크](https://analytics.google.com/analytics/web/provision/#/provision)**)등록
   - 웹 → URL 및 이름 등 정보 입력 → 측정 ID 발급
2. '\_config.yml'에서 Anaytics 문단 변경
   - provider: "google-gtag"
   - tracking_id: 측정 ID 복사

### <br>설정 화면 예시

```
# Analytics
analytics:
  provider: "google-gtag" # false (default), "google", "google-universal", "google-gtag", "custom"
  google:
    tracking_id: "G-3JEE7MF1RM"
    anonymize_ip: false
```

<br><br>

# 소감

Configuration 페이지를 읽어보았지만 아직 어떤 설정이 어떤 기능을 하는지 명확히 모르겠다.
<br>평소 잘 만들어진 페이지를 보고 도입하고 싶은 기능이 있으면 그때그때 구글링해서 적용하는 것이 좋을 것 같다.
<br>그런 평소에 잘 만들어진 페이지에 들어가보는 습관을 가져야겠다.
