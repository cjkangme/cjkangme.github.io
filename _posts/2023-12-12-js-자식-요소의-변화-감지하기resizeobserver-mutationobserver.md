

        ---
        title: [JS] 자식 요소의 변화 감지하기(ResizeObserver, MutationObserver)
        date: 2023-12-12 11:11:31.075 +0000
        categories: [react]
        tags: ['javascript', 'react']
        description: 동적 UI는 너무 어려워..
        image: /assets/img/posts/2023-12-12-js-자식-요소의-변화-감지하기resizeobserver-mutationobserver/thumbnail.png
        
        ---

        ![](/assets/img/posts/2023-12-12-js-자식-요소의-변화-감지하기resizeobserver-mutationobserver/img0.png)

# 문제

React로 바텀시트를 구현하게 되었다.

바텀시트의 요구 사항은 다음과 같다.
- 마운트 및 언마운트시 애니메이션 실행
- 유저의 드래그 액션에 반응하는 애니메이션이 존재
- `children` props를 받아 자식으로 렌더링하는 공통 컴포넌트 역할 수행

그런데 `children`의 높이가 동적으로 변하는 경우
높이 변화가 반영되지 않는 문제가 발생했다.

`children`에서 변화가 일어나도 부모인 바텀시트는 변화를 모르기 때문에 아무런 동작을 수행하지 않는 것이었다.

즉 `children`에 요소의 추가/제거 등이 발생할 때 이를 캐치하고 DOM 요소에 접근해 변경된 높이 값을 가져올 수 있는 수단이 필요했다.

그렇게 한창 삽질하고, 여기저기 찾아본 결과
Web API로 제공되는 기능을 통해 간단히 문제를 해결할 수 있다는 것을 알게 되었다.

# MutationObserver

[MDN - MutationObserver](https://developer.mozilla.org/ko/docs/Web/API/MutationObserver)

```javascript
useEffect(() => {
  // 참조안정성을 위해(childrenRef가 변경될 가능성을 고려) observer 등록 시 childrenRef를 저장하는 변수
  let current;
  
  // 옵저버 선언
  const observer = new MutationObserver((mutations) => {
    for (const mutation of mutations) {
      console.log(mutation);
    }
  });
  
  // 자식요소(childrenRef)를 observer에 등록
  if (childrenRef.current) {
    observer.observe(childrenRef.current, {
      childList: true, // 자식 트리의 변화 감지 (노드 추가/제거 등)
      attributes: true, // 자식 요소의 속성 변경 감지
      subtree: true // 모든 자식 요소 변화 감지 (false일 경우 루트만)
    });
    current = childrenRef.current;
  }
  
  // 컴포넌트 언마운트시 observer에서 제거
  return () => {
    if (current) {
      observer.disconnect(current);
    }
  }; 
}, []);
```

![](/assets/img/posts/2023-12-12-js-자식-요소의-변화-감지하기resizeobserver-mutationobserver/img1.png)

자식 요소를 추가/제거 할 때마다 [MutationRecord](https://developer.mozilla.org/en-US/docs/Web/API/MutationRecord) 객체를 콜백함수에 전달하여 호출한다.

이를 통해 변화의 종류(타입)를 알 수 있고, `mutation.target`을 통해 변화된 노드에 직접 접근하여 DOM 정보에 접근할 수 있다.

예를 들어 `mutation.target.offsetHeight`로 추가/제거된 노드의 높이를 가져올 수 있다.

다만 이번 경우에는 해결책으로 적합하진 않았는데, 추가/제거 되는 노드가 여러개일 때 모두 Listen되어 중복되는 값을 여러번 들고오는 경우가 생겼다.

때문에 추가적으로 해결책을 찾다 `ResizeObserver`라는 것을 알게 되었다.

# ResizeObserver

[MDN - ResizeObserver](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver)

```javascript
  let current
  useEffect(() => {
    const observer = new ResizeObserver((entries) => {
      for (const entry of entries) {
        console.log(entry);
      }
    });

    if (childrenRef.current) {
      observer.observe(childrenRef.current, {
        box: "border-box" // css의 border-box 크기를 가져옴 (기본값: content-box)
      });
      current = childrenRef.current;
    }

    return () => {
      if (current) {
        observer.disconnect(current);
      }
    };
  }, []);
```

![](/assets/img/posts/2023-12-12-js-자식-요소의-변화-감지하기resizeobserver-mutationobserver/img2.png)

`ResizeObserver`는 Observing하고 있는 요소의 크기 변화를 감지하고, 해당 요소의 크기 정보를 담은 [ResizeObserverEntry](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserverEntry) 객체를 콜백함수에 인자로 전달한다.

자식 요소가 얼마나 있든 상관 없이 크기 변화만을 감지하기 때문에 중복 실행될 우려가 `MutationObserver`보다 적다.

또한 자식의 크기에 상관없이 자식의 추가/삭제 후 변화된 높이 값을 가져오기 때문에 원하는 로직을 수행하기 훨씬 수월하다.

```javascript
const observer = new ResizeObserver((entries) => {
  for (const entry of entries) {
    const newHeight = entry.contentRect.height;
    
    // newHeight을 사용한 바텀시트 높이 조절 로직
  }
});
```

썸네일로 쓰인 바텀시트의 경우 위와 같이 높이 값을 받아 사용했다.

# 결론

![](/assets/img/posts/2023-12-12-js-자식-요소의-변화-감지하기resizeobserver-mutationobserver/img0.png)

아직 완벽하지는 않아서 로직 개션이 필요하지만
성공적으로 자식요소의 높이 변화를 감지하여 바텀시트에 적용할 수 있게 되었다.

그냥 정적인 바텀시트였다면 문제가 없을 텐데, 애니메이션과 함께 사용하다보니 많이 난이도가 올라간겄 같다..

애니메이션쪽은 좀 더 많은 공부가 필요할 것 같다.

        