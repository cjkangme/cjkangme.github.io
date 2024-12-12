

---
title: React에서 key prop을 요구하는 이유
date: 2023-12-02 13:24:29.908 +0000
categories: [react]
tags: []
description: map 함수를 사용할 때 key를 부여하지 않으면 오류 메세지가 나오는 이유를 알아보자
image: /assets/img/posts/2023-12-02-react에서-key-prop을-요구하는-이유/thumbnail.png

---

![](/assets/img/posts/2023-12-02-react에서-key-prop을-요구하는-이유/img0.png)

리액트에서 `map` 고차 함수를 통해 여러개의 컴포넌트를 렌더링할 때 각 컴포넌트의 root에 `key` prop을 입력하지 않을 경우 다음과 같은 에러가 발생한다.

하지만 콘솔에서만 경고메세지가 뜰 뿐 렌더링에는 아무 문제가 없는 것처럼 보인다.

`key`는 대체 왜 필요할까?

## React의 가상 DOM(VDOM)

바닐라 JS에서는 직접 DOM에 접근하여 조작하지만, React에서는 자바스크립트 객체인 가상 DOM(VDOM)을 통해 변경된 요소를 리렌더링한다는 차이가 있다.

조금 더 자세히 설명하자면 다음과 같은 재조정(Reconciliation) 과정을 거친다.

1. state나 props이 변경될 때 `render()` 함수가 실행된다.
2. `render()` 함수는 새로운 VDOM 트리를 반환한다.
3. **새로운 VDOM과 기존 VDOM를 비교해 변경점을 찾아낸다.**
4. 변경된 부분을 실제 DOM에 반영해 리렌더링한다.

리액트 공식문서에 의하면 3번 과정에서 두 트리를 비교하기 위해서는 아무리 좋은 알고리즘이라도  `O(N^3)`이라는 높은 시간 복잡도를 필요로 한다.

React는 두 가지 가정을 통해 `O(N)`으로도 두 트리를 비교할 수 있는 알고리즘을 구현했는데, 이 가정을 유지하는데 필요한 것이 바로 `key` prop이다.

## React의 변경점 비교

React가 두 VDOM을 비교할 때 다음과 같은 과정을 거친다.

1. VDOM의 루트 노드 부터 비교한다.
2. 두 노드의 타입 또는 `key`가 다르다면 이전 VDOM의 노드는 모두 버리고 새 VDOM의 노드로 교체한다.
	- 타입이 다르다는 것은 `div`, `button`, `a` 등이 바뀌었다는 것을 의미한다.
    - `key`는 반복적인 요소의 렌더링이 필요할 때 개발자가 제공하는 속성으로, `key` prop이 존재할 때만 타입과 함께 노드 식별에 사용한다.
3. 타입이 같다면 두 노드의 속성을 비교하면서 변경된 점만 갱신한다.
4. 자식 노드로 이동해서 재귀적으로 위 과정을 반복한다.

위와 같이 `key`는 반복적인 요소의 렌더링에서 요소의 교체 여부를 판정하는 것을 돕는 역할을 한다.

`key`가 없다면 어떤 일이 발생할지에 대해 알아보자

## key prop의 역할

### key가 없을 경우

```jsx
// 변경 전
<ul>
  <li>Duke</li> // 0
  <li>Villanova</li> // 1
</ul>

// 변경 후 (1)
<ul>
  <li>Duke</li> // 0
  <li>Villanova</li> // 1
  <li>Connecticut</li> // 2
</ul>

// 변경 후 (2)
<ul>
  <li>Connecticut</li> // 0
  <li>Duke</li> // 1
  <li>Villanova</li> // 2
</ul>
```

위 상황은 기존 자식 트리에 `<li>Conneticut</li>` 요소가 새롭게 추가된 것이다.

(1)의 경우 자식 리스트의 마지막 요소가 추가된 것이기 때문에 React는 기존 요소를 유지하면서 마지막에 새롭게 추가된 요소만 추가하면 된다.

그러나 (2)의 경우 리스트의 구조가 바뀐 것이기 때문에 React는 기존 요소를 통채로 삭제하고, 새 요소로 교체한다.

이는 (1)에 비해 훨씬 비효율적인 방법이 된다.

### key가 있다면

```jsx
// 변경 전
<ul>
  <li key="first">Duke</li>
  <li key="second">Villanova</li>
</ul>

// 변경 후 (1)
<ul>
  <li key="first">Duke</li>
  <li key="second">Villanova</li>
  <li key="third">Connecticut</li>
</ul>

// 변경 후 (2)
<ul>
  <li key="third">Connecticut</li>
  <li key="first">Duke</li>
  <li key="second">Villanova</li>
</ul>
```

이제 React는 key를 비교하는 것으로 변경 전, 변경 후를 비교할 수 있다.

(1), (2) 모두 `key="first"`, `key="second"`인 요소는 그대로 유지되고 있고, `key="third`인 요소만 새로 추가되었다는 것을 알 수 있기 때문이다.

이렇듯 `key` prop을 통해 React에서 리렌더링 과정을 더욱 효율적으로 수행하고, 의도치 않은 동작을 예방할 수 있다.

## index를 key로 사용하면 안되는 이유

`key`와 관련해서 가장 많이 찾아 볼 수 있는 이야기가, index를 key로 사용하면 안된다는 것이다.

왜그럴까?
공식문서에서 제공하는 [CodePen 예시](https://ko.legacy.reactjs.org/redirect-to-codepen/reconciliation/index-used-as-key)를 확인해보자

![](/assets/img/posts/2023-12-02-react에서-key-prop을-요구하는-이유/img1.png)

CodePen 예시와 같이 React에서 자식 리스트를 다룰 때, 자식리스트의 인덱스 순서가 변경되면 기존 VDOM과 비교하는 과정에서 잘못된(의도하지 않은) 컴포넌트를 재사용하거나 맵핑하게 될 수 있다.

때문에 React 공식문서에서는 사용할 데이터(상태)에 별도로 `id` 값을 부여하여 key로 사용할 것을 권장하고 있다.

# 참고 자료들

[React 공식 문서 legacy - 재조정](https://ko.legacy.reactjs.org/docs/reconciliation.html#recursing-on-children)
[React 공식 문서 legacy - 리스트와 Key](https://ko.legacy.reactjs.org/docs/lists-and-keys.html)
[React Virtual DOM 비교 원리와 얕은 비교](https://babycoder05.tistory.com/entry/React-Virtual-DOM-%EA%B3%BC-%EB%B9%84%EA%B5%90-%EC%9B%90%EB%A6%AC%EC%99%80-%EC%96%95%EC%9D%80-%EB%B9%84%EA%B5%90)
[[리액트] Virtual DOM과 diffing](https://joong-sunny.github.io/react/react2/)
[Index as a key is an anti-pattern](https://robinpokorny.medium.com/index-as-a-key-is-an-anti-pattern-e0349aece318)

        