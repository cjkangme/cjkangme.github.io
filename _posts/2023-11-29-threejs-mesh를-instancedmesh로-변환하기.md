

        ---
        title: [Three.js] Mesh를 InstancedMesh로 변환하기
        date: 2023-11-29 13:13:52.129 +0000
        categories: [three-js]
        tags: ['three.js']
        description: 복잡한 구조의 Object3D를 InstancedMesh로 변환해보자
        image: /assets/img/posts/2023-11-29-threejs-mesh를-instancedmesh로-변환하기/thumbnail.png
        
        ---

        대부분의 3D Assets은 여러개의 Mesh가 계층 구조로 묶여있는 하나의 Group이다.

이러한 Assets를 대량으로 렌더링해야한다면, DrawCall이 급격하게 증가할 것이다. 이럴 때 인스턴싱을 고려해볼 수 있다.

그러나 InstancedMesh는 하나의 Geometry, Material만 지정할 수 있어 이렇게 Mesh가 여러개인 Assets을 인스턴싱하는 것에는 많은 어려움이 따른다.

이를 어떻게 인스턴싱 할 수 있을까?

## 인스턴싱이란
![](/assets/img/posts/2023-11-29-threejs-mesh를-instancedmesh로-변환하기/img0.png)

위와 같이 6개의 Mesh로 이루어진 Group이 있다고 가정하자.

그룹을 구성하는 각 Mesh들은 각자 상대적인 Position, Rotation, Scale 값을 가질 것이다. (빨간색 글씨)

또한, 어떤 Mesh끼리는 동일한 Geometry, Material을 공유할 수도 있다. 위 그림에서 이를 형광펜 색으로 나타내었다.

![](/assets/img/posts/2023-11-29-threejs-mesh를-instancedmesh로-변환하기/img1.png)

결과적으로 이를 인스턴싱하기 위해서는 동일한 Geometry, Material을 쓰는 Mesh별로 묶어 각각 InstancedMesh를 생성해주어야 한다.

Position, Rotation, Scale은 InstancedMesh의 setMatrixAt() 메서드를 통해 설정할 수 있기 때문에, Geometry, Material가 같다면 서로 묶어줄 수 있다.


즉 여러 Mesh로 구성된 그룹을 인스턴싱하기 위한 절차는 다음과 같다.

1. Group을 Mesh 단위로 탐색한다. 
2. 탐색하면서 각각의 Position, Rotation, Scale 값을 저장해둔다. Mesh의 Geometry, Material가 같다면 하나의 그룹으로 묶어준다.
3. 분류된 각각의 그룹에 대해 InstnacedMesh를 생성한다.
4. 각 InstancedMesh에 기존에 저장해 두었던 Position, Rotation, Scale을 부여한다.

지금부터 이를 코드적으로 어떻게 할 수 있는지 알아보자

### 1. Group 탐색

가장 먼저 탐색할 Group의 Position과 Rotation, Scale을 초기화 해야한다.

이러한 과정이 필요한 이유는 여러개의 인스턴스는 3D 씬에서 (0, 0, 0)을 중심으로 다양한 Position과 Rotation, Scale 벡터값을 가질 수 있기 때문이다. 만약 (0, 0, 0)으로 초기화 되어있지 않다면, 이후 변환 과정에서 위치가 어긋나게 될 수 있다.

예를 들어 위 그림에서 몸통의 Position은 (0, 0, 0)이면 머리는 (0, 1, 0)인데, 초기화가 제대로 되지 않아 몸통 (1, 1, 1), 머리 (1, 2, 1)인 상태로 저장한다면 추후 행렬 연산 과정에서 의도치 않은 동작을 할 수 있다.

```javascript
const cloneGroup = SkeletonUtils.clone(scatterObject.object.mesh);
cloneGroup.position.set(0, 0, 0);
cloneGroup.rotation.set(0, 0, 0);
cloneGroup.scale.set(1, 1, 1);
cloneGroup.updateMatrixWorld();
```

그 다음으로는 Object3D.traverse 메서드를 사용해서 Group내의 모든 Mesh를 탐색할 수 있다.

```javascript
cloneObject.traverse((child) => {
  if (child.isMesh) {
    // TODO: Position, Rotation, Scale 값을 저장 및 Geometry / Material 별로 묶어주기
  }
});
```

### 2. Position, Rotation, Scale 값을 저장 및 Geometry / Material 별로 묶어주기

#### Geometry / Material 별로 묶어주기

이제 하나의 여러개의 InstancedMesh를 하나로 묶어주기 위해 javascript object를 활용할 것이다.

object의 각 요소는 InstancedMesh의 원본이 될 Mesh와 각 인스턴스에 부여해야 할 Position, Rotation, Scale 값이 object 형태로 저장된다.

그리고 같은 Geometry / Material을 갖는 Mesh가 중복 저장 되는 것을 방지 하기 위해, three.js Object3D에 자동 부여되는 uuid를 이용해 새로운 id값을 만들어주어 해시 테이블처럼 사용할 것이다.

```javascript
  meshTree = {};

  cloneObject.traverse((child) => {
    if (child.isMesh) {
      const uuid = child.geometry.uuid + "/" + child.material.uuid; // 새로운 uuid 생성
      if (!this.meshTree[uuid]) {
        child.meshTree[uuid] = {
          mesh: node,
          matrixes: [],
        };
      }
    }
  });
```

이렇게 하면 같은 Geometry / Material을 갖는 Mesh가 여러개여도 한 번만 저장된다.

#### Position, Rotation, Scale 값 저장

Position, Rotation, Scale 값은 하나의 4x4 행렬(Matrix4)로 저장할 수 있다.

이렇게 하면 `InstancedMesh.setMatrixAt()` 메서드를 통해 다른 변환 과정 없이 손쉽게 개별 인스턴스 값을 업데이트 할 수 있기 때문이다.

three.js가 `Object3D.matrixWorld()` 메서드를 제공해주기 때문에, 이를 통해 기존 Mesh의 Matrix4 값을 쉽게 가져올 수 있다.

```javascript
meshTree = {};

cloneObject.traverse((child) => {
    if (child.isMesh) {
      const uuid = child.geometry.uuid + "/" + child.material.uuid;
      if (!meshTree[uuid]) {
        child.meshTree[uuid] = {
          mesh: node,
          matrixes: [],
        };
      }
      meshTree[uuid].matrixes.push(child.matrixWorld.clone()); // 추가된 부분
    }
  });
```

위의 추가된 부분처럼 Matrix4를 matrixes 배열에 추가해주면 된다.

### 3. 그룹별로 InstancedMesh 추가하기

그룹(meshTree)는 자바스크립트 object이므로 Object.keys() 함수를 통해 반복문을 돌 수 있다.

반복문을 통해 그룹 내 모든 요소를 순회하면서 각각 InstancedMesh를 만들고 별도의 객체에 저장해주면 된다.

이때 우리가 부여한 uuid를 통해 접근할 수 있도록 해야 추후 이미 렌더링 된 인스턴스를 수정하고자 할 때 기존의 Matrix 값을 가져올 수 있다.

```javascript
const instancedMeshes = {};
Object.keys(meshTree).forEach((uuid) => {
      const { mesh, matrixes } = meshTree[uuid];
      const count = 100; // 원하는 인스턴스 개수
      const instancedMesh = new THREE.InstancedMesh(
        mesh.geometry,
        mesh.material,
        count * matrixes.length
      );
      instancedMeshes[uuid] = instancedMesh;
      // TODO: Matrix 저장했던 Matrix 적용
    });
```

### 4. Position, Rotation, Scale을 부여

이미 기존에 matrixes라는 속성에 저장해두었으므로, 꺼내와서 그대로 setMatrixAt()을 통해 부여하면 된다.

모든 과정이 완료되면 Scene에 추가하도록 하자.

```javascript
const instancedMeshes = {};
Object.keys(meshTree).forEach((uuid) => {
      const { mesh, matrixes } = meshTree[uuid];
      const count = 100; // 원하는 인스턴스 개수
      const instancedMesh = new THREE.InstancedMesh(
        mesh.geometry,
        mesh.material,
        count * matrixes.length
      );

      matrixes.forEach((matrixWorld, idx) => {
        instancedMesh.setMatrixAt(idx, matrixWorld);
      });

      instancedMeshes[uuid] = instancedMesh;
      scene.add(instancedMesh) // Scene에 추가
    });
```

### 5) 개별 인스턴스 수정

현재 상황에서는 인스턴스가 몇백개가 있든 모두 (0, 0, 0)에 겹쳐 생성될 것이다.

각 인스턴스에 서로 다른 벡터 값을 부여하기 위해 별도의 함수(또는 메서드)를 만들어야 한다.

```javascript
_tempMatrix = new THREE.Matrix4()
setMatrixAt(meshTree, instancedMeshes, index, matrix) {
    Object.keys(meshTree).forEach((uuid) => {
      const instancedMesh = instancedMeshes[uuid];
      const { matrixes } = meshTree[uuid];
      matrixes.forEach((matrixWorld, i) => {
        _tempMatrix.copy(matrixWorld);
        _tempMatrix.premultiply(matrix);
        instancedMesh.setMatrixAt(
          index * matrixes.length + i,
          _tempMatrix
        );
      });
      instancedMesh.matrixWorldNeedsUpdate = true;
    });
  }
```

위 과정은 적용하고자 하는 InstancedMesh들을 기존에 저장했던 uuid를 통해 찾고

각 index 값을 이용해 적용할 인스턴스를 찾은 뒤

새로 적용할 matrix(matrix)에 기존 인스턴스의 matrix(matrixWorld)를 곱하는 과정이다.

이렇게 하면 새로 적용할 변환 값이 적용되는데

그 원리는 아래 게시글을 참조하자

[과거 작성했던 네이버 블로그 글](https://blog.naver.com/nureongi0214/223227024179)

        