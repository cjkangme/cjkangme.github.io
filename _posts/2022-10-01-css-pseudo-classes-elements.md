---
layout: single
title: "[CSS] Pseudo-classes와 Pseudo-elements"
categories: til
tags: CSS
---

# Pseudo Classes(의사 클래스)

pseudo classes는 선택자에 추가하는 키워드로, <br>
선택한 요소가 특정 상태일 때 스타일을 적용하도록 할 수 있다. <br>

## 구문

아래와 같이 선택자 뒤에 콜론(`:`)을 함께 붙여 사용할 수 있다.

```css
selector:pseudo-class {
  property: value;
}
```

## 자주 사용하는 pseudo-classes

### :active

해당 요소를 마우스로 눌렀을 때부터, 떼는 순간까지 스타일을 적용한다.

### :hover

해당 요소 위에 마우스가 위치하고 있을 때 스타일을 적용한다.

### :focus

해당 요소가 키보드로 포커스 되었을 때 스타일을 적용한다.<br>
※ input 등에서 키보드 커서가 활성화 되었거나, Tab 키를 통해 요소를 선택할 경우 적용된다.

### :focus-within

포커스 된 요소를 포함한 부모요소에 스타일을 적용한다.

### :target

링크와 id 속성을 이용해서 페이지 내부를 이동할 때, 이동한 요소에 스타일을 적용한다.<br>
주로 활성화된 콘텐츠 영역 등을 표시할 때 사용한다.

### :visitied

링크에 적용되는 pseudo-class로, 이미 방문한 적 있는 링크에 스타일을 적용한다.<br>

<br>

pseudo-class 종류에 따라 적용되는 스타일이 제한 될 수도 있고, 훨씬 더 많은 종류가 존재하니 자세한 내용은 MDN을 참조하는 것이 좋다.<br>
[Pseudo-classes MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)

## 응용

### 자식 요소에 스타일 적용

자신의 스타일을 변경하는 것이 아니라, 자식 요소의 스타일을 변경하는 것이 가능하다.

```css
form:hover input {
  border: 2px solid red;
}
```

이 경우 form 요소에 마우스가 올라가면, form 내부에 있는 input의 스타일에 적용된다.

### 복합 조건 적용

여러개의 상태를 동시에 만족할 때 스타일이 적용되도록 할 수 있다.

```css
form:hover input:focus {
  background-color: greenyellow;
}
```

이 경우 form 요소에 마우스가 올라가 있고, input이 포커스 되어있는 상태 두가지를 모두 만족해야 스타일이 적용된다.

# Pseudo Elements (의사 요소)

Pseudo Classes와 유사하게 선택자에 추가하는 키워드이다. <br>
하지만 선택한 요소의 일부분에만 스타일을 적용할 수 있다는 것이 차이점이다.

## 구문

아래와 같이 선택자 뒤에 더블 콜론(`::`)을 붙여서 사용할 수 있다.

```css
selector::pseudo-element {
  property: value;
}
```

## 자주 사용하는 pseudo-elements

### ::placeholder

`<input>`의 placeholder 텍스트에 스타일을 적용한다.

### ::selection

텍스트 하이라이트(드래그나 마우스 더블클릭 등으로 텍스트를 선택했을 때) 스타일을 적용할 수 있다.

### ::first-letter

각 블록요소(`<div>`, `<p>` 등) 콘텐츠의 첫 글자에 스타일을 적용할 수 있다.<br>
첫 글자가 아닌 첫 줄에 적용할 수 있는 ::first-line도 있다.
