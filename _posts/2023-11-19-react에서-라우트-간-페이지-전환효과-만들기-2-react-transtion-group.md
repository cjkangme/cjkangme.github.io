

---
title: React에서 라우트 간 페이지 전환효과 만들기 (2) react-transtion-group
date: 2023-11-19 06:08:57.465 +0000
categories: [react]
tags: ['javascript', 'react']
description: react-transition-group을 통해 url을 이동할 때도 모바일 앱처럼 페이지 전환효과를 넣어보자.
image: /assets/posts/2023-11-19-react에서-라우트-간-페이지-전환효과-만들기-2-react-transtion-group/thumbnail.png

---

> [React에서 모바일 앱 같은 페이지 전환효과 만들기 (1) framer-motion](https://velog.io/@cjkangme/React%EC%97%90%EC%84%9C-%EB%AA%A8%EB%B0%94%EC%9D%BC-%EC%95%B1-%EA%B0%99%EC%9D%80-%ED%8E%98%EC%9D%B4%EC%A7%80-%EC%A0%84%ED%99%98%ED%9A%A8%EA%B3%BC-%EB%A7%8C%EB%93%A4%EA%B8%B0-1)

이전 게시글에서 `framer-motion` 라이브러리를 이용해 페이지 전환 효과를 만들었다.

그러나 이 방법으로는 뒤로가기(`useNavigate(-1)`)를 통해 이전 페이지로 이동했을 경우를 구분하지 못한다.

이를 개선하기 위해 `react-transition-group` 라이브러리를 이용해보자

## react-transition-group

![img](/assets/posts/2023-11-19-react에서-라우트-간-페이지-전환효과-만들기-2-react-transtion-group/img0.png)

[공식문서](https://reactcommunity.org/react-transition-group/)

`react-transition-group`은 DOM이 들어오거나 나갈 때의 상태 변화를 다루는 것을 통해 React 애플리케이션에서 애니메이션 전환 효과를 다루는 것을 도와준다.

`react-transition-group`가 제공하는 컴포넌트 중 아래의 두가지를 사용할 것이다.
- CSSTransition
- TransitionGroup

### CSSTransition

css className을 기반으로 자식 컴포넌트에 애니메이션을 적용해주는 컴포넌트이다.

```jsx
// 공식 예제
function App() {
  const [inProp, setInProp] = useState(false);
  const nodeRef = useRef(null);
  return (
    <div>
      <CSSTransition nodeRef={nodeRef} 
                     in={inProp} 
                     timeout={200} 
                     classNames="my-node">
        <div ref={nodeRef}>
          {"I'll receive my-node-* classes"}
        </div>
      </CSSTransition>
      <button type="button" onClick={() => setInProp(true)}>
        Click to Enter
      </button>
    </div>
  );
}
```

위와 같이 `div`를 자식으로 갖는`CSSTransition`은 다음과 같이 동작한다.

- `inProp`이 true가 될 때 `div`를 마운트한다. 
  - 이 때 `<클래스 이름>-enter` className을 붙여준다.
  - `timeout`에 명시된 시간동안 `<클래스 이름>-enter-active` className을 붙여준다.
  - `timeout`에 명시된 시간이 모두 지나면 `*-active` className을 제거하고, `<클래스 이름>-enter-done` className을 붙여준다.


- `inProp`이 false가 될 때 `<클래스 이름>-enter-done` className을 제거하고, `<클래스 이름>-exit`을 붙여준다.
  - `timeout`에 명시된 시간 동안 `<클래스 이름>-exit-active` className을 붙여준다.
  - `timeout`에 명시된 시간이 모두 지나면 `div`는 언마운트된다.


즉 css에서 `*-active`에 애니메이션을 정의하는 것을 통해 페이지 트랜지션 애니메이션을 구현할 수 있다.

### TransitionGroup

`TransitionGroup`은 `CSSTransition`을 목록으로 관리하며, 마운트 상태에 따른 `inProp`을 자동으로 전달해준다.

앞서 `framer-motion`로 적용했던 애니메이션을 `react-transition-group`으로 바꾸어 이들의 사용법을 알아보자

## 애니메이션 적용하기

```jsx
// ...
import { TransitionGroup, CSSTransition } from "react-transition-group";

function App() {
  const location = useLocation();

  return (
    <TransitionGroup>
      <CSSTransition 
        key={location.pathname} 
        timeout={5000}
        className="page-transition"
        > 
        <Routes location={location}>
          <Route path="/A" element={<A />} />
          <Route path="/B" element={<B />} />
          <Route path="/C" element={<C />} />
        </Routes>
      </CSSTransition>
    </TransitionGroup>
  );
}

export default App;
```

위와 같이 페이지 전환 효과를 적용할 라우트를 `CSSTransition`으로 묶고, 이를 다시 `TransitionGroup`으로 감싸주면 된다.

`timeout`에 5000ms를 준 이유는 개발자 도구에서 변화를 관찰하기 위해서이다.

![img](/assets/posts/2023-11-19-react에서-라우트-간-페이지-전환효과-만들기-2-react-transtion-group/img1.png)

![img](/assets/posts/2023-11-19-react에서-라우트-간-페이지-전환효과-만들기-2-react-transtion-group/img2.png)

위와 같이 `timeout`에 명시된 5000ms의 시간 동안
마운트 되는 요소에는 `*-enter`, `*-enter-active` className이 부여되고
언마운트 되는 요소에는 `*-exit`, `*-exit-active` className이 부여된다.

`timeout`에 명시된 시간이 지나면
`*-enter-done` className만 남게된다.

이제 className에 맞게 css를 작성하면 된다.

### css 작성하기

```css
.page {
  position: absolute; /* 필수 */
  width: 100vw;
  height: 100vh;
}

.page-transition-enter {
  z-index: 1;
  transform: translateX(100%);
}
.page-transition-enter-active {
  transform: translateX(0);
  transition: transform 300ms;
}
.page-transition-exit {
  z-index: 0;
  transform: translateX(0);
}
.page-transition-exit-active {
  transform: translateX(-100%);
  transition: transform 300ms;
}
```

간단하게 오른쪽으로 와서 왼쪽으로 밀려나는 애니메이션을 직접 지정해주었다.
주의해야할 점은 다음과 같다.

#### 렌더링 될 요소의 root에 `position: absolute` 명시하기
앞선 게시글에서 설명했 듯, 한 화면에 두 요소가 동시에 렌더링 되는 것이므로 이 두 요소가 겹쳐서 나오게 하려면 포지션을 고정해야 한다.

#### overflow로 인한 스크롤 생성
마찬가지로 한 화면에 두 요소가 동시에 렌더링 됨으로 인해, 뷰포트를 넘어서도록 요소가 렌더링이 될 수 있다.
이로 인해 생기는 스크롤을 필요하다면 적절히 처리해주어야 한다.

#### z-index 명시하기
애니메이션 의도에 따라 다르겠지만, 새로 마운트 되는 페이지는 일반적으로 언마운트 될 페이지 보다 앞에 표시되어야 하므로 `z-index`를 명시하는 것이 좋다.

### 적용 결과
![img](/assets/posts/2023-11-19-react에서-라우트-간-페이지-전환효과-만들기-2-react-transtion-group/img3.png)

`framer-motion`에서 만들었던 것과 동일한 페이지 트랜지션 효과를 적용하였다!

그러나 여전히 뒤로갈 때 똑같은 애니메이션지 재생된다는 문제가 존재한다.

## 뒤로가기 애니메이션 적용하기

뒤로가기 애니메이션을 위해서는 **유저가 현재 페이지에 어떻게 도달했는지에 따라 서로 다른 애니메이션을 적용**해야 한다.

### useNavigationType

`react-router-dom`이 제공하는 `useNavigationType` 훅은 현재 페이지에 어떻게 도달했는지에 대한 상태 정보를 반환한다.

```
declare function useNavigationType(): NavigationType;

type NavigationType = "POP" | "PUSH" | "REPLACE";
```

useNavigateType이 반환하는 값은 다음과 같다.
- PUSH: `history`에 새 스택이 쌓일 경우
- POP: `history` 스택을 변경한 경우 (뒤로가기, 앞으로가기)
- REPLACE: `history`의 현재 값을 변경한 경우

> 즉 이 방법으로는 '뒤로가기', '앞으로가기'를 동일하게 인식한다. 
>
> 보다 정교하게 동작하게 만들고 싶다면 별도의 커스텀 history 상태를 만들어 사용하거나 `popstate` 이벤트를 리스닝하여 전역에서 상태 관리를 하는 등, 별도의 로직이 필요하다.
>
> 일단 이번 게시글에서는 간단하게 `useNavigationType`을 이용해보았다.

### childFactory

`TransitionGroup` 컴포넌트가 제공하는 함수로, React의 `cloneElement`와 같이 자식 요소를 업데이트 하는데 사용된다.

`childFactory`의 특장점은 바로 언마운트 될 요소에도 접근하여 반응형 업데이트를 할 수 있다는 것이다.

```jsx
// ...
import { useLocation, useNavigationType } from "react-route-dom"
import { TransitionGroup, CSSTransition } from "react-transition-group";

function App() {
  const location = useLocation();
  const navigationType = useNavigationType()

  return (
    <TransitionGroup
      childFactory={(child) =>
          React.cloneElement(child, {
            className:
              navigationType === "POP"
                ? "page-transition--pop"
                : "page-transition--push",
          })
        }>
      <CSSTransition 
        key={location.pathname} 
        timeout={300}
        > 
        <Routes location={location}>
          <Route path="/A" element={<A />} />
          <Route path="/B" element={<B />} />
          <Route path="/C" element={<C />} />
        </Routes>
      </CSSTransition>
    </TransitionGroup>
  );
}

export default App;
```

기존 `CSSTransition`에서 className을 정의하는 대신, `childFactory`를 이용해 동적으로 className을 부여하였다.

이제 css만 잘 구분해서 만들어주면 된다.

```css
.page-transition--push-enter {
  z-index: 1;
  transform: translateX(100%);
}
.page-transition--push-enter-active {
  transform: translateX(0);
  transition: transform 300ms;
}
.page-transition--push-exit {
  z-index: 0;
  transform: translateX(0);
}
.page-transition--push-exit-active {
  transform: translateX(-100%);
  transition: transform 300ms;
}

.page-transition--pop-enter {
  z-index: 1;
  transform: translateX(-100%);
}
.page-transition--pop-enter-active {
  transform: translateX(0);
  transition: transform 300ms;
}
.page-transition--pop-exit {
  transform: translateX(0);
}
.page-transition--pop-exit-active {
  transform: translateX(100%);
  transition: transform 300ms;
}
```

### 결과
![img](/assets/posts/2023-11-19-react에서-라우트-간-페이지-전환효과-만들기-2-react-transtion-group/img4.png)

이제 뒤로가기를 할 때 다른 애니메이션이 적용된다.

이렇듯 `react-transition-group`은 css 애니메이션을 직접 만들어줘야 한다는 어려움이 있지만, 훨씬 더 유연하게 사용할 수 있다.

다음 개인 프로젝트를 한다면 이 라이브러리를 적극 이용해보아야 겠다.


        