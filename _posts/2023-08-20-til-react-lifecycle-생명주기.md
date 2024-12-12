

---
title: [TIL] React Lifecycle (생명주기)
date: 2023-08-20 12:12:11.857 +0000
categories: [TIL]
tags: ['til']
description: 프론트엔드 면접 단골질문 React Lifecycle
image: /assets/posts/2023-08-20-til-react-lifecycle-생명주기/thumbnail.png

---

> 참고 자료들
- [한빛미디어 | [리액트 초보 / 입문] 07-1강_훅](https://youtu.be/_eq5h9JzN4U)
- [ZeroCho | React의 생명 주기(Life Cycle)](https://www.zerocho.com/category/React/post/579b5ec26958781500ed9955)
- [93jm | 리액트 shouldcomponentupdate](https://velog.io/@93jm/%EB%A6%AC%EC%95%A1%ED%8A%B8-shouldcomponentupdate)
- [ahsy92 | [기술면접] Function형 React의 라이프 사이클](https://velog.io/@ahsy92/%EA%B8%B0%EC%88%A0%EB%A9%B4%EC%A0%91-React%EC%9D%98-%EB%9D%BC%EC%9D%B4%ED%94%84-%EC%82%AC%EC%9D%B4%ED%81%B4-09q2s7uw)
- [minbr0ther | [React.js] 리액트 라이프사이클(life cycle) 순서, 역할, Hook](https://velog.io/@minbr0ther/React.js-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4life-cycle-%EC%88%9C%EC%84%9C-%EC%97%AD%ED%95%A0)

# 서론

리액트 컴포넌트는 생성될 때 부터 제거될 때 까지 여러가지의 이벤트를 발생시키며, 이를 생명주기(Lifecycle)이라고 한다.

클래스 컴포넌트를 주로 사용할 때에는 각 컴포넌트를 중심으로 생명 주기를 따졌기 때문에, 매우 중요한 개념이었다.

사실 Hook이 추가된 React 16.8 이후에는 함수형 컴포넌트를 주로 사용하게 되었고, `useEffect`를 사용하게 되면서 중요성이 비교적 줄었다고 한다.

하지만 지금까지 본 2번의 면접에서 모두 React 생명주기에 대한 질문이 들어왔던 만큼 면접 대비로는 꼭 알고 가야할 지식인 것 같아 정리해보려 한다.

# 리액트 컴포넌트의 생명주기

![img](/assets/posts/2023-08-20-til-react-lifecycle-생명주기/img0.png)

출처 : [wojtekmaj/react-lifecycle-methods-diagram](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

## Mounting

- 리액트 컴포넌트가 처음 생성되는 시기를 말한다.

### componentWillMount

먼저 컴포넌트가 처음 실행 될 때 정의된 클래스 내의 constructor(생성자)를 통해 JS 객체(리액트 컴포넌트)를 생성한다.

이 때 context, defaultProps, state가 저장되며 이 과정이 끝난 후에 `componentWillMount` 메소드를 호출한다.

이 시점에서는 아직 DOM에 컴포넌트가 부착되지 않았기 때문에 DOM을 통한 접근이 불가능하고, state를 변경하면 안된다.

### componentDidMount

다음으로는 생성된 리액트 컴포넌트를 렌더링한다.

렌더링이 완료되면 `componentDidMount` 메소드를 호출한다.

이 시점에서는 DOM을 통해 컴포넌트에 접근이 가능하고, state 변경이 가능하다.

## Updating

- props의 변경, `setState` 호출로 인한 state 변경 등으로 인해 컴포넌트를 재렌더링하는 시기를 말한다.

### Props Update

> `componentWillReceiveProps` →  `getDerivedStateFromProps` → `shouldComponentUpdate` → `componentWillUpdate` → (render) → `componentDidUpdate`

props가 변경되었을 때, 차례로 호출되는 메서드 들이다.
`componentDidUpdate`를 제외하면 첫번째 인자로 업데이트 될 props에 대한 정보를 갖고 있다.

#### getDerivedStateFromProps

React 16에 추가된 생명주기로, props의 변경에 따라 state가 바뀌어야 할 경우 이를 바꿔주는 단계이다.

#### shouldComponentUpdate

만약 부모 컴포넌트의 props 변경이 자식 컴포넌트에 영향을 주지 않을 경우, 불필요한 렌더링을 방지하기 위해 `shouldComponentUpdate` 단계에서 렌더링을 막는 등의 목적으로 사용할 수 있다. (return이 true면 렌더링, false면 렌더링 안함)

이후 props의 렌더링이 완료되면 `componentDidUpdate` 메소드가 호출된다.

### State Update

> `shouldComponentUpdate` → `componentWillUpdate` → (render) → `componentDidUpdate`

기본적으로 Props Update와 동일하게 동작하지만, `componentWillReceiveProps` 과정이 빠진다.

### componentDidUpdate

변경된 상태를 화면에 표시하는 것(렌더링)까지 완료한 후 실행되는 메소드이다.

이미 DOM에서는 바뀐 state가 반경되었기 때문에 `componentDidUpdate`의 인자는 변경 전의 props/state에 대한 정보를 갖고 있다.

## Unmounting

더는 컴포넌트를 사용하지 않을 때, 컴포넌트를 제거하면서 발생하는 이벤트이다.

### componentWillUnmount

컴포넌트가 사라지기 직전에 호출된다.
주로 연결되었던 `eventListener`나 `setTimeout`을 제거하는 코드가 들어간다.

# useEffect

![img](/assets/posts/2023-08-20-til-react-lifecycle-생명주기/img1.png)

> 출처 : [wavez/react-hooks-lifecycle](https://wavez.github.io/react-hooks-lifecycle/)

React 17 이후에는 함수형 컴포넌트에서 `useEffect` 훅을 사용해 라이프사이클을 대체한다.

기본적으로 `useEffect`는 컴포넌트가 렌더링 된 직후 실행된다. 
`componentDidMount`, `componentDidUpdate`을 합쳐놓은 역할을 한다고 생각하면 된다.

`useEffect`는 함수를 return할 수 있는데 이 때 반환하는 함수가 `componentWillUnmount` 단계에서 호출할 함수이다.

또한 `useEffect`는 두번째 인자 **deps(dependencies)**를 설정하는데, 여기에는 state를 array 형태로 넣는다. deps로 선언된 state들이 변경되어 리렌더링 될 때마다 해당 `useEffect`의 코드가 재실행된다.

`useEffect`가 `componentDidMount` 역할만 하는 것을 원할 경우 deps에 빈 배열을 넣어주면된다.

반대로 `useEffect`가 `componentDidUpdate` 역할만 하는 것을 원할 경우 `useRef` Hook과 함께 사용한다.



        