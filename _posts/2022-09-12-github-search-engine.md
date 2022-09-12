---
layout: single
title: "GitHub 블로그에 검색엔진 등록하기"
categories: study
tags: GitHub
---

> ### **참고 자료 : 깃헙(Github) 블로그 만들기 (유튜브)**
>
> ### **주소 : [유튜브 링크](https://youtube.com/playlist?list=PLIMb_GuNnFwfQBZQwD-vCZENL5YLDZekr)**

<br>

# 구글 검색엔진 등록 (Google Analytics)

> [Minimal mistakes/configuration/#google-search-console](https://mmistakes.github.io/minimal-mistakes/docs/configuration/#google-search-console) 참조

## 1. 구글 웹마스터 툴즈 등록

1. 구글 웹마스터 툴즈 홈페이지 이동 (**[링크](https://search.google.com/search-console)**)
2. URL 접두어에 블로그 URL 입력 (예시 : `https://github/cjkangme.github.io`)
   ![webmaster-tools](/assets/images/github/webmaster-tools.png)
3. 소유권 확인 파일을 다운로드하여 클론된 프로젝트 폴더 루트 디렉토리에 저장
   - index.html 및 LICENSE 파일이 있는 디렉토리
4. commit 후 파일이 적용되는 것을 기다렸다가 인증 진행

<br>

## 2. 구글 검색엔진 설정

1. 좌측메뉴 Sitemaps에서 새 사이트맵 추가 - `sitemap.xml` 입력
   ![sitemap setting](/assets/images/github/sitemap-setting.png) - sitemap.xml : 웹사이트 내 모든 페이지를 목록 형태로 나열하여 검색엔진에 목차를 제공하는 역할을 수행한다. xml외에 다양한 사이트맵 형식이 존재한다.
2. [블로그 주소]/sitemap.xml 페이지로 이동했을 때 페이지 목록이 정상적으로 보이면 설정이 완료된 것

<br>

---

<br>

# 네이버 검색엔진 등록 (Search Advisor)

## 네이버 서치 어드바이저 등록

1. 네이버 서치 어드바이저 홈페이지 이동 (**[링크](https://searchadvisor.naver.com/)**)
2. 로그인 후 우측상단의 '웹마스터 도구' 클릭
3. 사이트 등록 입력란에 google 때와 동일하게 블로그 URL 입력
4. html 메타 태그값을 복사하여 `_config.yml` 파일의 `naver_site_verification`에 입력

![naver meta tags](/assets/images/github/naver-meta-tags.png)

<br>

```
- 입력예시
naver_site_verification: "2fb91a7b9dc00bbb31cb138f5909e4cd551e301e"
```

5. commit 후 파일이 적용되는 것을 기다렸다가 인증 진행

<br>

## 네이버 검색 엔진 설정

1. 좌측 메뉴 요청 → 사이트맵 제출에서 `sitemap.xml` 입력하여 제출
2. 좌측 메뉴 검증 → robots.txt → 수집요청

   - robots.txt : 검색엔진의 접근을 조절하고 제어하여 검색엔진의 접근을 최적화 해주는 역할 (설정을 통해 원하는 디렉토리의 접근을 차단하거나 하여 민감정보나 중복컨텐츠 등을 차단할 수 있다.)

   ```
   설정 예시

   User-agent: Yeti
   Disallow: /study/

   -> 네이버 검색엔진이 study 카테고리에 접근하는 것을 차단 (Yeti는 네이버 서치 로봇 이름)
   ```

3. 좌측 메뉴 검증 → 웹페이지 최적화 → 확인을 눌렀을 때 모든 정보가 잘 가져와지는지 확인
