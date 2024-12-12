

---
title: [에이블스쿨] 빅프로젝트 회고 - React/Django 프로젝트 생성
date: 2023-06-26 16:12:54.666 +0000
categories: [에이블스쿨]
tags: ['aivle']
description: 회고라기보다는 공부 내용 정리


---

# React/Django 연동하기

## Anaconda 가상환경 생성

- 우선 모두의 개발 환경을 동일하게 맞추기 위해 Anaconda 가상 환경을 생성하였다.
- Anaconda를 선택한 이유는 에이블스쿨 과정 내내 사용했기 때문에 팀원 모두가 가장 익숙할 것이라 생각해서
- 최초에는 가상환경이 담긴 yaml 페이지를 만들어서 배포하는 식으로 사용했으나, 개발 중간중간에 패키지가 하나 둘 늘어나기 시작한 뒤로는 `pip freeze > requirements.txt`를 통해 현재 가상환경에 있는 패키지를 txt 파일에 담아 깃허브에 함께 push하는 식으로 개발 환경을 맞추었다.

## React 프로젝트 생성

`npx create-react-app <app-name>`

- 정말 고맙게도 react는 `create-react-app`(CRA)를 통해 순식간에 리액트 어플리케이션을 만들 수 있다.
- 내 기억에 바닐라 JS를 써야할 때는 `nodemon`, `babel` 등을 수동으로 설치해서 복잡한 설정과정을 거쳐야 했던 것으로 기억한다. 
- 그런데 CRA를 이용하면 저장시 즉시 반영(nodemon), 자바스크립트 문법 변환(babel), 모듈별로 쪼개진 파일들을 하나의 번들로 묶어주는 기능(webpack)을 한번에 설치해줘서 실행 가능한 어플리케이션을 만들어준다.

## Django 프로젝트 생성

- 하는 방법보다는 왜 이런 과정이 필요했는지에 집중해서 작성해보자
- 하는 방법은 아래 참고 자료에서 자세히 설명 되어있다.

> 참고 자료
> [[#. Django] Django(Rest Framework) + React로 프로젝트 시작하기](https://developer0809.tistory.com/92)
> [[DRF를 이용한 Django-React 프로젝트] 3. django-react 개발 환경 구축](https://velog.io/@in_g_ing__/django-react-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95)
> [리액트(react) 장고(Django)연동을 위한 기본 리액트 프로젝트 생성](https://losskatsu.github.io/frontend/react-basic-setup/#%EB%A6%AC%EC%95%A1%ED%8A%B8react-%EC%9E%A5%EA%B3%A0django%EC%97%B0%EB%8F%99%EC%9D%84-%EC%9C%84%ED%95%9C-%EA%B8%B0%EB%B3%B8-%EB%A6%AC%EC%95%A1%ED%8A%B8-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%83%9D%EC%84%B1)

### django 및 DRF(django rest framework) 설치

- React/Django 프로젝트가 돌아가는 방식은 다음과 같다.
  1. React(클라이언트)에서 백엔드 서버로 axios(http request) 요청 전송
  2. Django(서버)는 REST API를 제공하여 요청에 맞는 응답을 전송
      - GET : 데이터 조회 (Read)
      - POST : 데이터 생성 (Create)
      - PUT : 데이터 수정 (Update)
      - DELETE : 데이터 삭제 (Delete)
  3. React는 응답을 바탕으로 사용자에게 보여줄 요소를 처리
  
- DRF는 Django를 통해 RESTful API를 쉽게 만들수 있도록 해주는 라이브러리이다. Sereailizer를 통해 JSON, Dict 등을 목적에 맞게 쉽게 변환해주고, 각종 인증을 자동 처리할 수 있는 편리한 도구이다.

### (번외) django Webpack Loader

- django와 react를 연동하기 위해서는 django 쪽에서 react의 파일을 알아야 한다.
- react 쪽에서는 webpack을 이용해 번들 파일을 만들 때 번들 처리 정보를 기록하기 위해 `webpack-bundler-tracker`라는 플러그인을 이용한다.
- `django-webpack-loader`는 바로 이 webpack-bundler-tracker가 만들어낸 출력을 이용해 django가 읽을 수 있는 bundle 파일을 만들어 낸다.


- 사실 이 방법은 이번 프로젝트에서는 사용하지 않는다. react는 사용자에게 보여주는 화면을 출력하고, 모든 DB처리는 백엔드에 axios 요청을 전송하는 식으로 이루어지기 때문이다.
- 주로 vue.js와 django를 연동해서 사용할 때 `django-webpack-loader`를 이용한다고 하니 우선 알아두자.

# 코멘트

- 회고를 쓰려고했는데 어쩌다보니 공부한걸 요약하는 노트같이 되어버렸다.
- 다음 회고를 쓸 때는 다른 사람들이 어떻게 회고를 썼는지 봐야지..

        