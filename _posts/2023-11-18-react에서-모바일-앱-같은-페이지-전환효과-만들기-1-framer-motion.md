

        ---
        title: React에서 모바일 앱 같은 페이지 전환효과 만들기 (1) framer-motion
        date: 2023-11-18 09:41:22.811 +0000
        categories: [react]
        tags: ['javascript', 'react']
        description: 사용자 경험을 좋게 만들어주는 페이지 전환효과 만들기
        image: /assets/img/posts/2023-11-18-react에서-모바일-앱-같은-페이지-전환효과-만들기-1-framer-motion/thumbnail.png
        
        ---

        # 페이지 전환 효과

<img src="https://www.yasinilhan.com/page_transition/transition.gif"  width="240"/>

<small>출처 : [플러터 Page Transition 라이브러리](https://pub.dev/packages/page_transition)</small>

PC 웹이든 모바일 웹이든 적절한 화면 전환 효과는 사용자가 화면 전환 맥락을 이해하는 것을 돕고, 자연스럽게 집중을 유도하는 등 사용자 경험을 향상시키는 것에 도움을 준다.

특히 웹에서도 앱과 같은 사용자 경험을 제공하는 웹앱을 만드는 것이 목표라면, 페이지 전환 효과는 꼭 고려해봐야 할 요소인 것 같다.

그럼 어떻게 페이지 전환 효과를 만들어 낼 수 있을까?
페이지 전환 효과를 위해서는 다음과 같은 요소를 고려해야 한다.

1. 새 페이지가 추가되는 애니메이션이 종료되기 전 까지는 이전 페이지의 렌더링이 유지되어야 한다.
2. 새 페이지와 이전 페이지의 애니메이션은 각각 별도로 동작해야 하며, (사용자 입장에서) 싱크가 맞아야 한다.

문제는 1번인데, 리액트에서는 생명주기에 대한 메서드 지원이 부족하기 때문이다.

즉 이전 페이지의 렌더링을 유지하기 위해서는 `setTimeout`과 같은 비동기 함수를 넣거나 해서 렌더링을 유지해주어야 한다.

다행히도 어렵게 구현하지 않아도 이러한 부분을 지원하는 라이브러리가 많이 존재한다.

여기서는 대표적으로 2개만 알아보도록 하자.

# Framer-motion

![](/assets/img/posts/2023-11-18-react에서-모바일-앱-같은-페이지-전환효과-만들기-1-framer-motion/img0.png)

[공식 홈페이지](https://www.framer.com/motion/)

`Framer-motion`은 복잡한 애니메이션에 대해 잘 알지 못해도 컴포넌트에 속성만 잘 전달하면 이에 맞는 애니메이션을 구현해주는 강력한 API를 제공한다.

즉, 많은 코딩이 필요하지 않고 쉽고 빠르게 애니메이션을 적용할 수 있다는 것이 가장 큰 장점이다.

## 사용법
### AnimatePresence

React 요소가 unmount될 때를 감지해서, 애니메이션과 같은 작업이 완료될 때까지 unmount를 연기해주는 역할을 하는 컴포넌트이다.

`AnimatePresence`는 **바로 아래 자식(direct children) 요소의 마운트 상태를 감지**해서 unmount되기 전에 exit 애니메이션을 재생할 수 있다.

만약 자식이 여러명이라면 각 요소를 구분할 수 있는 키값을 전달해주어야 한다.

```jsx
function App() {
  const location = useLocation();

  return (
    <AnimatePresence>
      <Routes location={location} key={location.pathname}>
        <Route path="/A" element={<A />} />
        <Route path="/B" element={<B />} />
        <Route path="/C" element={<C />} />
      </Routes>
    </AnimatePresence>
  );
}
```

`Route`를 이용한 페이지 이동이라면, `Routes` 또는 `Switch`의 부모에 `useLocation()`훅을 이용해 `location.pathname`을 키값으로 갖게 하면 된다.

이 키값은 최종적으로 렌더링 될 컴포넌트에 전달되어 exit 애니메이션이 동작하도록 해준다.

### 자식 요소 속성 지정
```jsx
// jsx
import { motion } from "framer-motion";

const Page = ({children}) => {
  return (
    <motion.div
      className="page"
      initial={{ x: "100%" }}
      animate={{ x: 0 }}
      exit={{ x: "-100%" }}
      transition={{ duration: 0.5 }}
    >
      {children}
    </motion.div>
  );
};

export default Page;
```

그리고 렌더링 될 자식 요소를 `motion.div`로 애니메이션이 적용되도록 한 뒤, 실행할 애니메이션에 맞게 속성을 설정해주면 된다.

위 예제에서는 오른쪽에서 페이지가 날아와서 기존 페이지를 밀어내고, 현재 페이지가 언마운트(exit)될 때는 왼쪽으로 벗어나게 하고 있다.

자세한 사용법은 공식문서를 참조하자.

### css 설정

```css
/* css */
.page {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
}
```

**`position: absolute` 속성을 꼭 지정해야 한다.**

![](/assets/img/posts/2023-11-18-react에서-모바일-앱-같은-페이지-전환효과-만들기-1-framer-motion/img1.png)

위 그림처럼 새 페이지가 마운트되어 애니메이션이 재생되는 동안에는 두 페이지 모두가 렌더링 되고 있는 상태이다.

즉 `position: absolute`을 정해주지 않는다면, 새로 렌더링 되는 페이지는 기존 페이지 밑에 붙어서 렌더링이 이루어지게 된다. (우리가 보는 뷰포트 밖을 벗어남)

때문에 다른 요소를 무시하고 정해진 위치에서 애니메이션이 재생될 수 있도록 `position`을 정해주어야 한다.

### 결과
![](/assets/img/posts/2023-11-18-react에서-모바일-앱-같은-페이지-전환효과-만들기-1-framer-motion/img2.png)

### 현재 문제점

그런데 만약 뒤로갈때는 렌더링이 반대로 이루어지게 하고 싶다면 어떡할까?

현재 우리가 만든 페이지 전환 효과는 버튼을 누르든 뒤로가든 상관 없이 항상 새로운 페이지를 오른쪽에서 불러온다.

뒤로가기 페이지 전환을 구현하기 위해서는 사용자의 뒤로가기를 감지하여, 다른 애니메이션을 적용시켜주어야 한다.

아쉽게도 `framer-motion`에서 이를 구현할 방법을 찾지 못했다.

`framer-motion`은 쉽고 편하게 높은 수준의 애니메이션을 적용할 수 있다는 장점이 있다.

그러나 공식문서가 다소 불친절하다고 생각되고, 커스텀 범위가 제한적이라 '뒤로가기 구현'과 같이 `framer-motion`만으로는 해결이 힘든 문제들이 있다.

### 해결책

`framer-motion` 대신 `react-transition-group`이라는 라이브러리가 있다.

이 라이브러리르는 css를 통해 애니메이션을 직접 구현해야 한다는 단점이 있으나 `framer-motion`보다 훨씬 더 유연하게 사용할 수 있다.

'뒤로가기 구현' 역시 가능하다.

다음 게시글에서`react-transition-group`을 적용해 페이지 전환 애니메이션을 구현하는 방법을 알아보자.

        