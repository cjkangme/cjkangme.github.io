

---
title: [three.js] three.js의 기본 구성 요소
date: 2023-10-28 10:33:17.518 +0000
categories: [three-js]
tags: ['three.js']
description: three.js의 기본 구성 요소 알아보기
image: /assets/posts/2023-10-28-threejs-threejs의-기본-구성-요소/thumbnail.png

---

각각의 구성요소는 여러가지 종류의 클래스, 옵션을 가지므로 좀 더 깊이 알아야하지만

여기서는 하나의 화면이 어떤 구성요소들로 구성되는지만 알아보자

![img](/assets/posts/2023-10-28-threejs-threejs의-기본-구성-요소/img0.png)

> <small>자료 출처 : https://sunzhongkui.wordpress.com/2015/08/08/3d-viewer-based-on-threejs/</small>

### Renderer

Scene을 출력 장치에 출력(렌더링)할 수 있는 객체
```javascript
// Renderer 예시
const animate = () => {
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
};
```

animate()
콜백을 통해 재귀적으로 실행되는 애니메이션 함수(requestAnimationFrame 등)에서 render를 호출하면 애니메이션을 표현 할 수 있다.

### Scene

화면에 렌더링 되는 3차원 그래픽 구조체 (트리 구조 기반)

그래픽 장면에 필요한 모든 정보를 담을 수 있다.

Light와 Mesh(Object3D), Camera 등 3D 장면(Scene)을 구성하는 모든 요소는 Scene에 추가되어야 한다.

### Light

3차원 형상이 화면에 보이기 위한 광원

### Mesh

Object 3D에서 파생된 클래스로 크기(scaled), 위치(positioned), 회전(rotated) 등의 변형을 하는 것에 더해 Geometry, Material을 추가로 다룰 수 있는 클래스이다.

#### Geometry 
육면체, 구체 등 객체의 3D 형상을 정의한다. GPU로 전달되기 때문에 빠른 형상화가 가능하다.

과거에는 `Geometry`가 3D 객체를 구성하는 폴리곤(정점과 정점으로 이루어진 삼각형)을 직접 갖고 있었지만, 
최신버전의 `BufferGeometry` 는 정점 정보와 정점에 대한 인덱스를 배열로 저장함으로써 보다 빠른 렌더링 성능을 얻게 되었다. 

#### Material
객체의 재질을 정의한다. 재질 종류에 따라 색상, 투명도, 반사도 등을 정할 수 있다. 

```javascript
// Mesh 예시
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial({ color: 0x00aa00 });
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);
```

### Camera

렌더링 된 씬을 보여주는 화면 시점을 담당한다.
이용자가 보는 3D 장면은 전부 카메라를 통해 보여주는 것이다.

![img](/assets/posts/2023-10-28-threejs-threejs의-기본-구성-요소/img1.png)
> <small>자료 출처 : https://i.stack.imgur.com/3hUZ2.png</small>

```javascript
// Camera 예시
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
```

카메라는 `PerspectiveCamera`와 `OrthographicCamera` 두 종류가 존재한다.
전자는 사람의 시각을 모방한 카메라로, 멀리있는 물체가 더 작게 보이는 원근감을 표현한다.

`PerspectiveCamera( fov : Number, aspect : Number, near : Number, far : Number )`

fov : 세로 방향 기준 시야각 (단위 : degree)

aspect : 카메라의 기준이 되는 화면 종횡비

near : 이 값보다 가까운 객체는 렌더링하지 않음

far : 이 값보다 멀리 있는 객체는 렌더링하지 않음

        