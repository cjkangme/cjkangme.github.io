---
layout: single
title: "[HTML] 폼 태그"
categories: lecture
tags: HTML
---

> 참고자료 : 유튜브(짐코딩의 CODING GYM) - HTML/CSS 강의
>
> **주소 : [유튜브 링크](https://youtube.com/playlist?list=PLlaP-jSd-nK-ponbKDjrSn3BQG9MgHSKv)**

<br>

# form 태그

> 사용자의 정보를 입력받아 특정 서버 프로그램에 제출하기 위해 사용하는 태그

로그인, 페이지, 게시판 글쓰기 등 사용자로부터 입력받는 데이터들을 묶는 것을 '폼(Form)'이라 한다.
<br>그리고 이렇게 입력받은 데이터를 폼 데이터(Form Data) 또는 필드(Field)라고 한다.
<br>폼 태그는 폼과 폼 데이터를 감싸는 역할을 한다. (폼의 범위를 지정)

## `<form>`의 속성

- action : 양식 데이터를 처리할 서버 프로그램의 URL (예시: /signup)
- method : 양식을 제출할 때 사용할 HTTP 메서드
  - post : 양식 데이터를 요청 본문으로 전송
  - get : 양식 데이터를 URL 쿼리스트링으로 붙여서 전송

<br>→ 폼을 상세히 다루는 것은 이후 CSS 강좌와 함께 진행할 것이기 때문에 우선은 이런 속성이 있다 정도로 파악하는 정도로 진행한다.

<br><br>

# input & label 태그

> input 태그는 사용자로부터 데이터를 입력 받을 수 있는 태그
> <br>label 태그는 입력받는 필드를 설명할 때 사용하는 태그 (웹접근성 준수)

## `<input>`의 type 관련 속성

- type : 데이터를 입력 받는 방법을 지정
  - type의 종류는 **[MDN 사이트](https://developer.mozilla.org/ko/docs/Web/HTML/Element/input#%3Cinput%3E_%EC%9C%A0%ED%98%95)** 를 참조하는 것이 좋다.
  - 사용자의 편의성과 데이터 성격을 고려하여 type을 선언하는 것이 좋다.

<br>

type 예제는 다음과 같다. 일부 type에 대한 설명은 하단에 따로 공간을 내어 설명할 예정

> | type           | 예제                                                           | 코드                                                             |
> | -------------- | :------------------------------------------------------------- | ---------------------------------------------------------------- |
> | button         | <input type="button" name="button">                            | `<input type="button" name="button">`                            |
> | checkbox       | <input type="checkbox" name="checkbox">                        | `<input type="checkbox" name="checkbox">`                        |
> | color          | <input type="color" name="color">                              | `<input type="color" name="color">`                              |
> | date           | <input type="date" name="date">                                | `<input type="date" name="date">`                                |
> | datetime-local | <input type="datetime-local" name="datetime-local">            | `<input type="datetime-local" name="datetime-local">`            |
> | email          | <input type="email" name="email">                              | `<input type="email" name="email">`                              |
> | file           | <input type="file" name="file">                                | `<input type="file" name="file">`                                |
> | hidden         | <input type="hidden" name="hidden">                            | `<input type="hidden" name="hidden">`                            |
> | image          | <input type="image" name="image" src="example.jpg" alt="샘플"> | `<input type="image" name="image" src="example.jpg" alt="샘플">` |
> | month          | <input type="month" name="month">                              | `<input type="month" name="month">`                              |
> | number         | <input type="number" name="number">                            | `<input type="number" name="number">`                            |
> | password       | <input type="password" name="password">                        | `<input type="password" name="password">`                        |
> | radio          | <input type="radio" name="radio">                              | `<input type="radio" name="radio">`                              |
> | range          | <input type="range" name="range">                              | `<input type="range" name="range">`                              |
> | search         | <input type="search" name="search">                            | `<input type="search" name="search">`                            |
> | tel            | <input type="tel" name="tel">                                  | `<input type="tel" name="tel">`                                  |
> | text           | <input type="text" name="text">                                | `<input type="text" name="text">`                                |
> | time           | <input type="time" name="time">                                | `<input type="time" name="time">`                                |
> | url            | <input type="url" name="url">                                  | `<input type="url" name="url">`                                  |
> | week           | <input type="week" name="week">                                | `<input type="week" name="week">`                                |

<br>

- min, max : 입력가능한 범위 값을 지정하여, 이를 통해 유효성 검사나 선택 가능한 값을 조절
  - number, range, 날짜 및 시간관련 type에서 사용할 수 있다.
- mutifle : file 등의 type에서 다중 선택/첨부가 가능하게 함

<br>

## `<input>`의 form 데이터 태그 속성

- name : input 컨트롤의 이름으로 폼 데이터의 일부로서 양식과 함께 전송
- for : input 컨트롤의 고유 id를 정의, label 컨트롤과 매칭시키는데 사용
- required : 필수 값으로 지정하여 데이터가 입력되어 있지 않으면 form을 전송할 수 없음
- readonly : 필드를 읽기 전용으로 만듦 (수정불가, 데이터 전송은 가능)
- disabled : 비활성화 (수정불가 및 해당 필드를 서버로 전송하지 않음)
- autofocus : 초기에 해당 필드에 커서를 위치시킴
- placeholder : 입력 필드가 비어있을 때 표시되는 설명 또는 가이드 문구
- value : 초기값 지정

<br>

## Checkbox와 radio

- checkbox : 여러개의 체크박스 항목 중 2개 이상 선택 가능
- radio : 여러개의 라이도 항목중 하나만을 선택 가능
  - name 속성을 통해 묶어주어야 같은 radio로 인식

<br>
예시
      <fieldset>
        <legend>좋아하는 색상을 선택하세요</legend>
        <ul>
          <li>
            <label for="red">빨강</label>
            <input id="red" type="checkbox" value="red" />
          </li>
          <li>
            <label for="orange">주황</label>
            <input id="orange" type="checkbox" value="orange" />
          </li>
          <li>
            <label for="yellow">노랑</label>
            <input id="yellow" type="checkbox" value="yellow" />
          </li>
          <li>
            <label for="green">초록</label>
            <input id="green" type="checkbox" value="green" />
          </li>
          <li>
            <label for="blue">파랑</label>
            <input id="blue" type="checkbox" value="blue" />
          </li>
        </ul>
      </fieldset>
      <fieldset>
        <legend>배송방법은?</legend>
        <ul>
          <li>
            <label for="free">무료</label>
            <input id="free" type="radio" name="delivery" value="free" />
          </li>
          <li>
            <label for="pay">유료</label>
            <input id="pay" type="radio" name="delivery" value="pay" />
          </li>
          <li>
            <label for="discount">할인</label>
            <input
              id="discount"
              type="radio"
              name="delivery"
              value="discount"
            />
          </li>
        </ul>
      </fieldset>

<br>

## `<label>` 의 속성

- for : `<input>`의 'id'와 일치시켜서 연결짓는데 사용
- form : 레이블 태그가 `<form>` 내부에 있지 않을 때 form의 'id'와 일치시켜서 연결짓는데 사용

<br><br>

# fieldset & legend 태그

> fieldset : 폼의 여러 컨트롤과 레이블을 묶을 때 사용
> legend : fieldset 태그 콘텐츠의 설명 역할

<br>

예시

```html
<form action="">
  <fieldset style="display: inline">
    <legend>개인정보</legend>
    <label for="username">아이디</label>
    <input id="username" type="text" />
    <label for="password">비밀번호</label>
    <input id="password" type="password" />
  </fieldset>
  <br />
  <fieldset style="display: inline">
    <legend>사업자정보</legend>
    <label for="username">아이디</label>
    <input id="username" type="text" />
    <label for="password">비밀번호</label>
    <input id="password" type="password" />
  </fieldset>
</form>
```

<form action="">
      <fieldset style="display: inline">
        <legend>개인정보</legend>
        <label for="username">아이디</label>
        <input id="username" type="text" />
        <label for="password">비밀번호</label>
        <input id="password" type="password" />
      </fieldset>
      <br />
      <fieldset style="display: inline">
        <legend>사업자정보</legend>
        <label for="username">아이디</label>
        <input id="username" type="text" />
        <label for="password">비밀번호</label>
        <input id="password" type="password" />
      </fieldset>
    </form>

<br><br>

# input 외 데이터 입력 수단

## `<Textarea>`

> 여러줄의 텍스트를 입력받을 수 있는 태그

```html
<textarea id="story" name="story" rows="5" cols="33">
	contents...
</textarea>
```

- rows : 입력할 수 있는 행수 지정 (몇 줄)
- cols : 한 줄에 입력되는 글자 수
- 예시처럼 value 속성에 기본값을 넣지 않고 콘텐츠로서 기본값을 입력함

<br>

## `<select>` - `<option>`

> 여러 옵션을 드롭다운으로 표시하여 하나를 선택하는 태그

```html
<ul>
  <li>
    <select name="goods" id="goods">
      <option value="apple_10kg">사과 10kg</option>
      <option value="apple_20kg">사과 20kg</option>
      <option value="apple_30kg">사과 30kg</option>
      <option value="apple_40kg">사과 40kg</option>
      <option value="apple_50kg">사과 50kg</option>
    </select>
  </li>
</ul>
```

- select
  - mutiple : 여러개를 동시선택 가능 (단, shitf+클릭, ctrl+클릭과 같은 수단 동원 필요)
- option
  - selected : 기본값 설정
  - value : form이 전송될 때 실질적으로 입력되는 값

<br>

## `<datalist>` - `<option>`

> 기본적으로 select 태그와 유사하지만, input과 연결되어 선택지를 추천하는 기능 제공
> <br>사용자가 자유롭게 텍스트를 입력할 수 있지만, 드롭다운으로 선택지가 표시됨

```html
<ul>
  <li>
    <label for="ice-cream-choice">맛을 선택하세요</label>
    <input id="ice-cream-choice" list="ice-cream-flavors" type="text" />
    <datalist id="ice-cream-flavors">
      <option value="Chocolate"></option>
      <option value="Blueberry"></option>
      <option value="Cheese"></option>
      <option value="Coconut"></option>
      <option value="Coffee"></option>
    </datalist>
  </li>
</ul>
```

<br>

## `<button>`

> 클릭 가능한 버튼 생성

- type을 통해 클릭시 행동 방식 선언
  - submit : (기본값) form의 action에 선언된 서버 URL로 폼 양식 제출
  - reset : form 내의 모든 입력 필드를 초기화
  - button : 기본 행동이 없으며 주로 onclick 속성과 함께 자바스크립트 명령 실행 시 사용
