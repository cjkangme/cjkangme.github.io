

---
title: [three.js] Depth Buffering(Z-buffering) 개념
date: 2023-11-05 01:47:49.466 +0000
categories: [three-js]
tags: ['three.js']
description: 3D 객체를 렌더링 할 때 화면에 가까운 물체만을 렌더링하는 기법을 알아보자
image: /assets/posts/2023-11-05-threejs-depth-bufferingz-buffering-개념/thumbnail.png

---

참고 강의: [GIS Developer 유튜브 강의](https://youtu.be/TF1Bs6-vIAw?si=GIWL-h5nEKVa1Y48)



Three.js가 제공하는 다양한 Materials에 대해 배우기 전에 Depth Buffering이라는 개념을 알아두는 것이 좋다.

![](/assets/posts/2023-11-05-threejs-depth-bufferingz-buffering-개념/img0.png)

<small>출처: https://ko.wikipedia.org/wiki/Z_%EB%B2%84%ED%8D%BC%EB%A7%81</small>

## Depth-buffering이란?

카메라의 시점에서 여러개의 물체가 겹쳐보일 때, **카메라에 가까운 물체의 픽셀만 렌더링하기 위해, 가까운 물체를 판정하는 기법**을 말한다.

이를 위해 Z값이란 개념을 사용하는데, Z값을 저장하는 저장소(버퍼가) 바로 `Z-buffer`이다. 

Z값은 `[0, 1]`의 정규화된 범위를 가진다.
Z값이 작을수록 카메라에 가깝다는 것을 의미한다. 
가장 가까운 픽셀의 Z값은 0.0이고, 가장 먼 픽셀의 값은 1.0이다.

![](/assets/posts/2023-11-05-threejs-depth-bufferingz-buffering-개념/img1.png)

<small>출처: https://gamersnexus.net/guides/2355-deep-dive-how-vxao-frustum-tracing-and-flow-work</small>

위 그림처럼 Z-buffer는 화면을 의미하는 2차원 좌표(x, y) 기준의 픽셀들로 정렬되어 있다.

또한, 하나의 픽셀에 여러개의 물체가 겹쳐 있다면, Z-buffer에 저장된 Z값과 새로 그려야할 물체의 Z값을 비교해 더 작은 값을 렌더링하고, Z-buffer에 덮어 씌우는 방식이다.

위 그림은 `[0, 1]`로 정규화 되어있진 않지만, 숫자가 작을수록 카메라 시점(화면)에서 더 가까운 물체이고, 숫자가 클 수록 화면에서 더 먼 물체임을 나타낸다.

이렇게 Scene의 각 픽셀마다 Z값이 저장되어있는 것이 바로 Z-buffer이다.

## Three.js에서의 Depth-buffering

Three.js 역시 Depth-buffering를 사용하여 물체를 렌더링한다.

또한 `Material` 클래스의 프로퍼티를 통해 Depth-buffering의 수행을 제어할 수 있다.

- `depthTest` : 렌더링 할 객체의 픽셀 위치를 Z-buffer를 이용해 검사할 지 여부 (기본값: true)
- `depthWrite`: 렌더링 할 객체 픽셀의 Z값을 Z-buffer에 반영할 것인지 여부 (기본값: true)
- `depthFunc`: 렌더링 할 객체를 검사할 때 적용할 함수 
(기본값: `LessEqualDepth` - 렌더링 할 픽셀의 Z값이 작거나 같다면 true)
`depthFunc`의 종류는 [공식문서](https://threejs.org/docs/index.html#api/en/constants/Materials)에서 확인할 수 있다.

## 또다른 방법 - 화가 알고리즘(painter's algorithm)

화가 알고리즘도 Depth-buffering과 같은 목적을 수행하기 위한 기법 중 하나이다.

단순하게 설명하면 가장 멀리있는 물체부터 렌더링 하는 방법으로, 가까운 물체일 수록 늦게 렌더링되며 기존의 픽셀을 덮어 씌우기 때문에 자연스럽게 3차원 공간은 2차원 화면에 투영할 수 있다.

### 화가 알고리즘의 단점

#### 렌더링 비용
필요한 물체만 렌더링 할 수 있는 Depth-buffering과 다르게 모든 물체를 렌더링 해야하므로 많은 자원을 필요로 한다.

#### 고리모양 문제
![](/assets/posts/2023-11-05-threejs-depth-bufferingz-buffering-개념/img2.png)

<small>출처 : https://ko.wikipedia.org/wiki/%ED%99%94%EA%B0%80_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98</small>

또한, 위 예시처럼 여러개의 물체가 맞물려 있을 경우 이를 제대로 처리하지 못하는 경우가 발생한다.

위와 같이 고리형으로 맞물린 다각형을 그려야 할 경우, 화가 알고리즘으로는 구현하지 못하고 어떤 다각형 하나를 완전히 덮어야 할 것이다.

Depth-buffering은 바로 이런 문제를 해결하기 위해 등장하였다.


        