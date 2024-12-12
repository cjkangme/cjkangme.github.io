

        ---
        title: [JS] LexicalEnvironment vs VariableEnvironment
        date: 2023-11-05 13:32:24.525 +0000
        categories: [javascript]
        tags: []
        description: 렉시컬 환경에 존재하는 두 컴포넌트 LexicalEnvironment와 VariableEnvironment에는 무슨 차이가 있을까?
        image: /assets/img/posts/2023-11-05-js-lexicalenvironment-vs-variableenvironment/thumbnail.png
        
        ---

        # 서론

모던자바스크립트 Deep Dive를 공부하던 중, 렉시컬 환경에는 `LexicalEnvironment`와 `VariableEnvironment` 두 종류가 있는데, 책에서는 하나로 본다는 설명을 읽게 되었다.

그런데 이걸 왜 하나로 보는거지? 무슨 차이가 있는거지?가 궁금해져서 따로 글로 작성하게 되었다.

## 실행 컨텍스트 (Excution Context)

실행 컨텍스트란 실행 중인 또는 실행할 코드에 필요한 환경 정보를 모아 놓은 객체이다.

소스코드가 실행될 때 런타임 이전에 실행 컨텍스트가 먼저 생성되고, 이 실행 컨텍스트에 앞으로 코드 실행에 필요한 정보들을 추가한다.

## 환경 (environment)

실행 컨텍스트의 구성 요소인 `렉시컬 환경`은 식별자와 식별자에 바인딩된 값, 상위 스코프에 대한 참조로 구성 되어있다.

즉 소스코드를 실행하는 런타임에서, 어떤 변수나 함수를 마주치면 `렉시컬 환경`에 등록되어있는 식별자를 통해 그 값을 찾아 이용한다. 현재 스코프에 식별자가 존재하지 않는다면 상위 스코프 참조를 바탕으로 스코프 체인에서 식별자를 찾게 된다.

실행 컨텍스트에는 이 `렉시컬 환경` 컴포넌트가 2개 존재하는데, 각각 `Lexical Environment`와 `Variable Environment`이라는 이름으로 구분한다. 
이번 게시글에서는 이 둘의 차이점이 뭔지, 각각 어떤 역할을 하는지에 대해 알아보자

![](/assets/img/posts/2023-11-05-js-lexicalenvironment-vs-variableenvironment/img0.png)
<small>출처: https://meetup.nhncloud.com/posts/86</small>

## 렉시컬 환경
렉시컬 환경(`LexicalEnvironment` 컴포넌트와는 다른 개념이니 주의하자)을 구성하는 요소는 다음과 같다.
- **환경 레코드(Environment Record)**
  - 스코프에 포함된 식별자와 식별자에 바인딩 된 값을 저장하는 자료구조
  - 사실 환경 레코드는  선언적 환경 레코드와 객체 환경 레코드로 다시 나뉘는데 여기서는 따지지 않겠다
- **외부 렉시컬 환경에 대한 참조(outerLexicalEnvironment Reference)**
  - 상위의 스코프에 접근할 수 있는 참조 (스코프 체인)

`Variable Environment`와 `Lexical Environment`는 바로 이 환경 레코드와 외부 렉시컬 환경에 대한 참조가 조금 다르다.

정확히는 처음 실행 컨텍스트가 생성될 때는 둘 다 동일한 렉시컬 환경을 참조하지만, 소스코드를 평가하는 과정에서 새로운 렉시컬 환경이 생성되고, 둘의 참조가 달라지게 된다.

#### Variable Environment 컴포넌트
- 환경 레코드 : `var` 키워드로 선언되는 변수, 함수 선언문으로 생성된 함수가 저장된다.
- 외부 렉시컬 환경에 대한 참조 : outer Environment(상위스코프의 렉시컬 환경)

#### Lexical Environment 컴포넌트
- 환경 레코드 : `let`, `const` 키워드로 선언되는 변수
- 외부 렉시컬 환경에 대한 참조 : Variable Environment

그렇다면 이 둘을 왜 구분해야할까?
그 이유는 `var`로 선언된 변수와 `let, const`로 선언된 변수는 스코프 범위가 다르며, 동작 방식 역시 조금 다르기 때문이다.

## `var`, `let`, `const`의 차이

### 함수 스코프와 블록 스코프

#### var
`var`로 선언된 변수는 함수 스코프를 따른다.

```javascript
// 예제
function foo() {
  if (true) {
    var a = 'Hello'
  }
  console.log(a); // Hello
}
```

위와 같이 변수를 if문 코드 블록 안에서 생성했음에도, 블록 스코프가 아닌 함수 스코프를 따르기 때문에 함수 스코프에 변수가 저장되었따.

#### const, let
`const`, `let`으로 선언된 변수는 블록 스코프를 따른다.

```javascript
// 예제
function foo() {
  if (true) {
    const a = 'Hello'
  }
  console.log(a); // Uncaught ReferenceError: a is not defined
}
```

위와 같이 if문 코드 블록 바깥을 빠져나오는 순간 if문의 블록 스코프가 종료되기 때문에 바깥에 있는 함수 스코프에는 변수가 저장되어 있지 않다.

이렇게 `var`와 `const, let`으로 선언한 변수의 스코프가 다르기 때문에

한 함수 안에 여러개의 블록 스코프가 존재할 때, 각 블록 스코프 별로 렉시컬 환경과 `Lexical Environment` 컴포넌트를 생성할 필요가 있다.

반면 함수 스코프를 따르는 `var` 변수와 함수 선언문으로 정의된 함수는 처음 실행 컨텍스트가 생성될 때 `Variable Environment`에 저장되어 사용되는 것이다.

### TDZ (Temporal Dead Zone)
TDZ에 대한 자세한 설명은 [더 잘 설명된 블로그 글](https://www.google.com/search?q=Temporal+Dead+Zone&oq=Temporal+Dead+Zone&gs_lcrp=EgZjaHJvbWUyCQgAEEUYORiABDIHCAEQABiABDIHCAIQABiABDIHCAMQABiABDIGCAQQABgeMgYIBRAAGB4yBggGEAAYHjIGCAcQABgeMgYICBAAGB4yBggJEAAYHtIBBzMwMWowajSoAgCwAgA&sourceid=chrome&ie=UTF-8#ip=1)을 참조하자

결론만 말하자면 

- `let, const`로 선언된 변수는 변수 선언문에 도달하기 이전에 참조할 경우 참조 에러가 발생한다. (TDZ)
- `var`로 선언된 변수는 변수 선언문에 도달하기 이전에 참조할 경우 `undefined`로 평가된다.

이렇게 동작 방식이 다르기 때문에 별도의 렉시컬 환경으로 관리하는 것이다.

# 코멘트

사실 찾아보다보니 생각보다 딥한 개념이어서 이것까지 꼭 알아야 하나 싶었다.

하지만 TDZ, 스코프, 실행 컨텍스트 등 자바스크립트의 핵심 동작 원리를 모두 관통하는 개념이었고 이에 대해 더 자세히, 그리고 더 확실하게 배울 수 있었던 것 같다.

# 참고자료
> [Pranav - Advanced JavaScript Series - Part 4.2: Scope Chains and their working, Lexical and Variable Environments](https://dev.to/pranav016/advanced-javascript-series-part-42-scope-chains-and-their-working-lexical-and-variable-environments-19d5)
[NHN Cloud - 자바스크립트 함수 (2) - 함수 호출](https://meetup.nhncloud.com/posts/123)
[NHN Cloud - 자바스크립트의 스코프와 클로저](https://meetup.nhncloud.com/posts/86)
[임우찬 - [JavaScript] ES6의 Execution Context(실행 컨텍스트)의 동작 방식과 Lexical Nesting Structure(Scope chain)](https://m.blog.naver.com/dlaxodud2388/222655214381)

        