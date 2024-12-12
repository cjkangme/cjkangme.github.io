

---
title: [three.js] Scene Graph
date: 2023-10-30 11:58:31.933 +0000
categories: [three-js]
tags: ['three.js']
description: Scene Graph 를 이용해 손쉽게 공전 모형 만들어보기
image: /assets/posts/2023-10-30-threejs-scene-graph/thumbnail.png

---

> 참고 강의 : [GIS Developer 유튜브 강의](https://youtu.be/nKK7L6QLjkg?si=6anDPt0UkfzJDKDg)

## Scene Graph(씬 그래프)란

<img alt="img" src="https://velog.velcdn.com/images/cjkangme/post/26f2079a-5df7-4255-84e3-d31919976691/image.png" width = "480px"/>
<small>출처 : Example scene graph structure in glTF (https://github.khronos.org/)</small>

Scene Graph는 복잡한 3차원 객체(scene)를 그려내기 위해 계층 구조로 Object 3D(Mesh, Line, Points 등)를 쌓아 구성하는 방법이다.

각 Object 3D는 position(위치), rotation(회전), scale(크기) 속성을 갖는데, 이는 **부모를 기준으로 한 상대값**이다.

각각의 속성은 x, y, z 축 방향의 방향성을 갖는다.
- x축 : 화면을 바라보는 기준 오른쪽
- y축 : 화면을 바라보는 기준 위쪽
- z축 : 화면을 바라보는 기준 모니터를 뚫고 나오는 방향


이 세가지의 주요속성은 4 x 4 행렬 정보로 변환되어 처리된다. (직접 설정하여 Custom 하는 것도 가능)

## Scene Graph 실습

![img](/assets/posts/2023-10-30-threejs-scene-graph/img0.png)

자전하는 태양, 태양을 공전하며 자전하는 지구, 지구를 공전하며 자전하는 달

![img](/assets/posts/2023-10-30-threejs-scene-graph/img1.png)

위 Scene을 구성하기 위해 다음과 같은 Scene Graph 구조를 사용할 수 있다.

### Scene Graph를 사용하는 이유

그냥 트리구조 없이 태양, 지구, 달을 각각 만들어서 렌더링하면 되지 않을까?라는 생각이 들 수 있다.

하지만 Scene Graph를 부모 객체에 적용된 속성을 하위 객체가 상속받아 동일하게 동작할 수 있다는 장점이 있다.

만약 태양, 지구, 달을 각각 독립적으로 만들었다면 달의 복잡한 궤도를 일일히 수학적 공식으로 계산해야 할 것이다.

하지만 Scene Graph를 이용하면 그냥 부모의 회전에 다신의 회전만 더하면 공전-자전 모형을 완성할 수 있다.

즉 훨씬 효율적으로 Scene을 그려낼 수 있다.

### Three.js에서 Scene Graph 구현하기

부모 객체(Object 3D)에 add() 메소드로 자식 객체를 추가하는 것을 통해 트리구조를 구현할 수 있다.

```javascript
// solarSystem 추가
    const solarSystem = new THREE.Object3D();
    this._scene.add(solarSystem);

    const radius = 1;
    const widthSegements = 12;
    const heightSegments = 12;

    // 공통 Geometry 정의 및 생성
    const sphereGeometry = new THREE.SphereGeometry(
      radius,
      widthSegements,
      heightSegments
    );

    // sunMesh 생성
    const sunMaterial = new THREE.MeshPhongMaterial({
      emissive: 0xffff00,
      flatShading: true,
    });

    // sunMesh 생성
    const sunMesh = new THREE.Mesh(sphereGeometry, sunMaterial);
    sunMesh.scale.set(3, 3, 3);
    solarSystem.add(sunMesh);

    // earthOrbit 생성
    const earthOrbit = new THREE.Object3D();
    earthOrbit.position.set(10, 0, 0);
    solarSystem.add(earthOrbit);

    // earthMesh 생성
    const earthMaterial = new THREE.MeshPhongMaterial({
      color: 0x0000ff,
      flatShading: true,
    });

    const earthMesh = new THREE.Mesh(sphereGeometry, earthMaterial);
    earthMesh.scale.set(1.5, 1.5, 1.5);
    earthOrbit.add(earthMesh);

    // moonOrbit 생성
    const moonOrbit = new THREE.Object3D();
    moonOrbit.position.set(3, 0, 0);
    earthOrbit.add(moonOrbit);

    // moonMesh 생성
    const moonMaterial = new THREE.MeshPhongMaterial({
      color: 0xaaaaaa,
      flatShading: true,
    });

    const moonMesh = new THREE.Mesh(sphereGeometry, moonMaterial);
    moonMesh.scale.set(0.5, 0.5, 0.5);
    moonOrbit.add(moonMesh);
```

위 코드에서 한가지 더 봐야할 것은 하나의 공통 sphereGeometry를 선언하고 태양, 지구, 달을 각각 color와 scale만 조정하는 것을 통해 적은 코드만을 작성하여 구현하였다는 것이다.

이렇게 하면 일일히 객체를 생성하지 않아도 되기 때문에 메모리를 덜 사용하는 최적화가 가능하다.

        