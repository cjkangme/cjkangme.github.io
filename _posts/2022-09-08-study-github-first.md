---
layout: single
title: "Github 블로그 만들기 1일차"
categories: study
tags: GitHub
---

### **참고 자료 : 깃헙(Github) 블로그 만들기 (유튜브)**

### <br>**주소 : [유튜브 링크](https://youtube.com/playlist?list=PLIMb_GuNnFwfQBZQwD-vCZENL5YLDZekr)**

<br><br>

# EP01. 개발환경 설치하기

## 준비물

- Github Desktop
- Typora
  - 강의 업로드 당시에는 무료였지만 현재 $14.99의 가격으로 구입해야한다.
  - 조금만 고생한다면 Visual Studio로 대체할 수 있을 것 같아 사용하지 않기로 하였다.
- Visual Studio Code
  <br><br>

# EP02. 이미지 매우 간단하게 추가하기

## Visual Studio Code에서 이미지 추가하기

※ 원 강좌는 Typora에서 이미지 추가하기였으나, 나에게 맞게 대체하였다.
<br>

1. GitHub Desktop에서 'Open in Visual Studio Code' 단추 클릭
   ![001](/images/2022-09-08-first/001.png)
   <br><br>
1. 개발환경 설치 당시 Clone을 만든 경로에 이미지를 저장할 별도의 폴더를 생성하여 이미지 삽입
1. VS Code의 EXPLORER에서 이미지가 확인이 되면 원하는 라인에 Shift+드래그
   ![002](/images/2022-09-08-first/002.png) - Visual Studio Code가 제공하는 미리보기 열기(Ctrl+Shift+V) 기능을 통해 이미지가 어떻게 삽입되었는지 확인 가능
   <br><br>
1. 코드 저장(Ctrl+S)
1. GitHub Desktop에서 파일이 변경된 것을 확인
1. Summary에 코드 변경 관련 내용을 간단히 요약하여 적고, Commit to master 단추 클릭
   ![003](/images/2022-09-08-first/003.png)
   <br><br>
1. GitHub Desktop에서 ‘Push origin’을 클릭하여 변경 내용 반영
   ![004](/images/2022-09-08-first/004.png)
   <br><br>

# EP03. 업데이트 내역을 실시간 확인하기 (로컬 개발환경 설정방법)

## 의의

### 기존에는 변경 사항 적용을 확인하려면 GitHub에 commit을 일일히 해야했으나, 로컬 개발환경을 설정하면 로컬서버에 연결된 브라우저에서 곧바로 확인할 수 있다.

<br><br>

## 설정 방법

1. jekyllrb 공식사이트의 Installation 접속 -> **[링크](https://jekyllrb.com/docs/installation/)**
1. Ruby 설치
   1. 운영체제에 맞는 설치 파일 클릭 (Windows의 경우 RubyInstaller)
   1. 최신버전의 WITH DEVKIT을 선택하여 다운로드
   1. 다운로드 된 setup 설치 완료 후 cmd가 실행되면 '3'을 입력하여 설치 진행
1. cmd를 실행하고 ‘**gem install jekyll**’ 입력하여 gem 설치 (Documents 디렉터리에)
1. 이어서 ‘**gem install bundler**’ 입력하여 gem 설치
1. 플러그인 설치
   1. GitHub 블로그가 연결된 폴더 경로를 Shitf+우클릭하여 터미널(PowerShell)로 열기
   1. github 블로그 폴더로 경로 이동한 뒤 ‘**bundle install**’을 입력하여 설치(명령어 : cd + 이동할 디렉토리명)
1. ‘**bundle exec jekyll serve**’ 입력
   - ‘webrick’을 load 할 수 없다는 LoadError 메세지가 출력되면 '**bundle add webrick**'을 입력하여 설치
   - cmd 결과 창에 서버주소가 나오면 구동에 성공한 것
1. 브라우저 주소창에 ‘localhost:포트번호’ 입력 - 자신의 github 블로그가 나오면 성공 - 해당 페이지에서는 commit을 하지 않아도 저장 시 수정사항이 실시간으로 반영
   <br><br>

## 후기

설정 방법대로 무난히 진행이 안되었다.
<br>원인을 파악해보니 계정명이 한글로 되어있어 cmd를 통한 install 시 경로를 찾지 못해 오류가 발생한 듯 하다.
![005](/images/2022-09-08-first/005.png)
<br><br>
windows 계정명을 변경하면 쉽겠지만, 내 계정명이 학교 이메일에 묶여있다보니 변경할 수 없었다.
<br> 대학교 졸업하고 한참 뒤에도 여전히 Edu 윈도우를 사용하고 있는 것에 대한 벌을 받은 것 같다.
<br> 다행히 실시간 확인을 못한다는 불편함만 감수하면 진행에는 문제가 없기 때문에 이대로 강의를 진행하기로 했다.
<br> 오늘 작성한 설정 방법은 향후 요긴하게 써먹도록 하자.
