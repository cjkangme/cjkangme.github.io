

---
title: [TIL] Execute Context & Closure
date: 2023-08-15 15:04:50.849 +0000
categories: [TIL]
tags: ['javascript', 'til']
description: 이고잉님의 생활코딩 강의로 알아보는 Execute context와 Closure
image: /assets/posts/2023-08-15-til-execute-context-closure/thumbnail.png

---

# Execute context

> 생활코딩 강의 : https://youtu.be/QtOF0uMBy7k

## Execute context

Execute context란 코드의 실행 문맥, 코드가 실행되고있는 환경을 의미하는 추상적인 개념이다.
모든 실행 중인 javascript 코드는 어떤 Execute context에서 실행되고 있다.

debugger를 이용해서 Execute context와 Scope 개념을 알아보자.


## Scope

자바스크립트에서 Scope는 변수에 접근할 수 있는 범위라고 한다.
현재 문맥이 접근할 수 있는 데이터의 범위를 나타낸다.

![img](/assets/posts/2023-08-15-til-execute-context-closure/img0.png)

크롬 개발자 도구의 debugger 기능을 이용해 Scope를 쉽게 확인할 수 있다.
이들이 어떻게 동작하는지는 차차 알아보자

```jsx
<script>
    n0='n0';
    var v0='v0';
    let l0='l0';
    const c0 = 'c0';
</script>
```

![img](/assets/posts/2023-08-15-til-execute-context-closure/img1.png)

- 먼저 위코드에서 break point를 걸어 `n0`, `v0`를 선언한 직후 멈춘 상태이다. 
`n0`, `v0`를 선언 했음에도 Script Scope에 보이지 않는다.
  - 확인해보면 Global Scope에 선언된 것을 알 수 있다.
  - 키워드를 붙이지 않거나, `var`로 선언하면 Global에 변수가 선언된다는 것을 알 수 있다.


- Script Scope에는 없지만 `console.log`로에서 `n0`를 출력하면 저장된 값이 나온다. 
어떻게 불러오는 걸까?
  - 이는 javascript의 Scope Chaining 덕분이다.
  - 먼저 현재 실행 문맥인 Script Scope에서 `n0`를 찾고, 없을 경우 부모 Scope인 Global Scope로 넘어와 `n0`를 가져온다.


![img](/assets/posts/2023-08-15-til-execute-context-closure/img2.png)


- 반면 `let`, `const` 키워드로 선언한 `c0`, `l0` 변수는 Script Scope에 저장된다.
- console에서 `c0`, `l0`를 검색하면, Script Scope에서 찾을 수 있기 때문에, Global Scope까지 접근하지 않는다.

### window 객체

- 웹 브라우저에서 동작하는 자바스크립트에서는 window객체가 곧 Global Scope이다.
- Global Scope인 window는 어디서든 Scope Chaing을 통해 접근이 가능하다.
- window를 생략해도 `confirm`, `alert` 등이 작동하는 이유이다.

```jsx
console.log(window.v0, window.n0, window.l0, window.c0);
// Output: v0 n0 undefined undefined
```

- 위처럼 `window`를 명시할 경우 Global Scope에서만 변수를 조회하기 때문에 `let`과 `const`에 의해 Script에 저장된 변수는 찾지 못하게 된다.

## callstack

![img](/assets/posts/2023-08-15-til-execute-context-closure/img3.png)

- callstack은 함수 호출에 따라 쌓이는 스택을 말한다.
- 여기서 `anonymous` 는 최초로 생긴 execute context로, 전역에서 접근할 수 있어 Global Excute Context라고 한다.
- `anonymous`에서 함수 `fn1`을 실행할 경우 callstack에 `fn1`이 쌓이는 것을 확인할 수 있다. 이렇게 함수가 호출되어 생긴 execute context는 Function Excute Context라고 한다.

```jsx
function fn1(){
        n1='n1';
        var v1='v1';
        let l1='l1';
        const c1='c1';
        fn2();
    }
fn1();
```

![img](/assets/posts/2023-08-15-til-execute-context-closure/img4.png)

- `f1` 함수 내에서 `f2` 함수를 호출한 경우이다.


- 각 call stack을 눌러보면 Scope의 상태가 다른걸 알 수 있다.
    - fn1에는 `Local`이라는 Scope가 추가되었다. 여기에는 함수 내에서 선언된 지역 변수들이 위치한다.
    - 앞서 Script → Global 순으로 탐색했던 것 처럼, Local → Script → Global 순으로 사용자가 불러온 변수를 찾는다.


- 특이한 점은 `f1` 함수 내에서 `f2`를 호출했음에도 `f1` Scope에서 `f2` Local Scope로 접근할 수 없다는 점이다.
- 이는 자바스크립트가 정적 스코프를 사용하기 때문인데, 이는 조금 있다가 알아보도록 하자.

```jsx
function fn1(){
        n1='n1';
        var v1='v1';
        let l1='l1';
        const c1='c1';
        fn2();
    }
fn1();
```

![img](/assets/posts/2023-08-15-til-execute-context-closure/img5.png)


- `n1 = ‘n1’`
    - 다시 함수로 돌아와서 `n1` 변수를 선언하면 여전히 Global Scope에 저장되는 것을 확인할 수 있다.
    - 즉 키워드 없이 어떤 context 상태이든 Global Scope에 들어가게 된다. → 별로 바람직하지 못하다.
- `var v1=’v1’`
    - 반면에 `var` 키워드는 Global execute context때와 다르게 Local Scope에 변수가 저장된다.

## 정리


|키워드|Global execute context|Funtion execute context|
|:---:|:---:|:---:|
|a = 1|Global|Global|
|var a = 1|Global|Local|
|let a = 1|Script|Local|
|const a = 1|Script|Local|

# Closure

> 생활코딩 Closure 강의 : https://youtu.be/bwwaSwf7vkE

- 앞서 Execute Context에서 살펴봤듯이, 함수 내에서 Scope는 Scope Chaining을 따라 Local > Script > Global로 내려가며 변수를 찾는다.
- Local Scope는 function execute context 별로 독립적으로 존재한다.

## Dynamic Scope, Lexical Scope

### Dynamic Scope (동적 스코프)

- 변수, 함수 등을 어디에서 호출했는지에 따라 접근 유효범위가 달라지는 Scope를 말한다.
- 하지만 `fn1` 안에서 실행된 `fn2`의 Scope를 `fn1`에서 접근하지 못했듯이, Javascript에서는 Dynamic Scope를 채택하지 않고 있다.

### Lexical Scope (Static Scope; 정적 스코프)

- **Lexical Scope는 변수, 함수 등을 어디에서 정의 되었는지에 따라 접근 유효범위가 달라지는 Scope를 말한다.**
- Javascript에서 채택한 방식이다.

### 예시

```jsx
function fn2 () {
	let l2 = 'l2'
	console.log(l1, l2)
}

function fn1 () {
	let l1 = 'l1'
	fn2()
}

fn1() // 에러 발생
```

- 이 코드의 경우 `fn1`, `fn2`가 동등한 동일한 Scope에서 선언되었기 때문에 서로 독립적인 Local Scope를 갖는다. 
즉 `fn2`가 `fn1`에서 호출되었어도, `fn2`는 `l1` 값을 알 수 없어 에러가 발생한다. (javascript가 동적 Scope를 따르지 않는다는 의미)

```jsx
function fn1 () {
	function fn2 () {
		let l2 = 'l2'
		console.log(l1, l2)
	}
	
	let l1 = 'l1'
	fn2()
}

fn1() // l1 l2
```

![img](/assets/posts/2023-08-15-til-execute-context-closure/img6.png)

- 이 코드의 경우 `fn2` 함수가 `fn1` 함수 내에서 선언이 되었다.
- Scope를 살펴보면 `Closure (fn1)` 이라는 Scope가 생성되었고, `fn1` 함수의 Local Scope가 저장되어있는 것을 알 수 있다.
- 이렇게 단 한번 정의되는 함수 등이 어디서 정의 되었는지에 따라 Scope가 결정되는 것을 Lexical Scope(or Static Scope)라고 한다.
- 함수를 함수 안에서 정의하면 부모 함수의 Scope에 접근할 수 있다.

## 정리

- Clouser는 ‘동봉’이란 뜻을 갖고 있다.
- 함수가 정의 될때 해당 함수의 부모 함수 Scope를 Clouser에 동봉하여 가지고, 언제 호출되든지 Scope Chaining을 통해 접근할 수 있다.

# 코멘트

지난번 `this`에 대해 글을 정리하다가, `lexical context`, `scope` 와 같은 말들을 이해하기 너무 어려워서 관련 개념을 찾아보게 되었다.

그런데 이고잉님이 너무 쉽고 좋은 강의를 올려주신게 있어서 이를 바탕으로 정리해보았다.
이고잉님은 신이다..

        