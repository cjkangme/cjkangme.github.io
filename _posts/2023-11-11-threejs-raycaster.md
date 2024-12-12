

        ---
        title: [three.js] Raycaster
        date: 2023-11-11 11:52:40.781 +0000
        categories: [three-js]
        tags: ['three.js']
        description: three.js의 Raycaster에 대해 알아보자
        image: /assets/img/posts/2023-11-11-threejs-raycaster/thumbnail.png
        
        ---

        Raycasting : 어떤 지점에서 광선을 발사해 그 광선에 닿는 물체가 있는지 없는지 검사하는 기법

3차원 공간을 2차원 화면에 투영하기 위해 각 픽셀마다 물체가 있는지 없는지 검사하는 것에 사용한다.

![](/assets/img/posts/2023-11-11-threejs-raycaster/img0.png)
<small>출처: https://ko.wikipedia.org/wiki/%EA%B4%91%EC%84%A0_%ED%88%AC%EC%82%AC</small>


## Three.js의 Raycastor

Three.js에서는 `Raycaster`라는 클래스를 통해 raycasting을 수행한다.

일반적으로 3차원 Scene의 객체를 마우스를 통해 집어내기 위해 사용한다.

raycasting이 필요한 이유는 마우스는 2차원 화면상에 있고, 물체는 3차원 공간상에 있기 때문이다.

만약 마우스가 (1, 1)을 가리키고 있을 때, 3차원 공간상에서는 (1, 1, 0)인지 (1, 1, 5)인지 알 수 없기 때문에 raycasting이 필요하다.

![](/assets/img/posts/2023-11-11-threejs-raycaster/img1.png)
<small>출처: https://docs.godotengine.org/ko/4.x/tutorials/physics/ray-casting.html</small>

위 그림처럼 마우스가 위치한 지점과 눈(=카메라)을 기준삼아 광선(Ray)을 발사함으로써, 광선에 닿는 물체를 감지 및 선택할 수 있다.

물론 카메라가 반드시 기준이 될 필요는 없고 직접 광선을 만들어서 지정해 줄 수도 있다.

## three.js의 Raycaster

### Constructor

- origin: 광선의 광원이 되는 지점 (Vector3)
- direction: 광선의 방향을 나타내는 벡터, 정규화 되어 있어야 함 (Vector3)
- near: near보다 멀리 있는 물체만 결과로 반환, 0보다 작을 수 없음 (float, 기본값: 0)
- far: far보다 가까이 있는 물체만 결과로 반환(float, 기본값: INF)

일반적으로 `origin`, `direction` 모두 동적으로 변경되는 경우가 많다.

이 경우 속성 지정을 돕는 `setFromCamera`, `set` 메서드를 사용한다.

### Properties
- far: far factor (`float`)
- near: near factor (`float`)
- camera: raycasting의 기준이 카메라 일 때 사용할 카메라 (`THREE.Camera`)
  - 기본값은 null이며 .camera 속성을 직접 설정하거나, setFromCamera 메서드를 통해 설정할 수 있다.
- layers: 3D Scene이 여러개의 레이어로 구성되어있을 때, 특정 레이어만 raycasting 하도록 지정하는 속성

```javascript
raycaster.layers.set( 1 );  // layer = 1 인 물체만 raycast
object.layers.enable( 1 );  // object의 layer를 1로 지정
```

- params: `Raycastor`가 광선-객체와의 교차점을 찾을 때 사용하는 다양한 매개변수를 정의하고 있다. raycasting 조건을 조절하거나 할 때 이 속성을 이용할 수 있다.

```javascript
{ 
	Mesh: { },
	Line: { threshold: 1 },
	LOD: {}, // 사용할 수 있는 매개변수 없음
	Points: { threshold: 1 },
	Sprite: {} 
}
```
`threshold`는 광선과 객체(Mesh, Line, Points)의 교차 거리를 설정할 수 있다. 
예를 들어 `Points`나 `Line`의 `threshold`를 더 크게 하면 광선이 Points와 직접 교차하지 않고, 주변을 지나가더라도 교차된 객체에 포함시킬 수 있다.

- ray: raycasting에 사용하는 광선 객체 (Ray)

### Methods

#### set(origin: Vector3, direction: Vector3)

origin과 direction을 새롭게 설정한다.

#### setFromCamera(coords: Vector2, camera: Camera)

set과 동일한 역할을 하지만, 2차원 화면과 카메라 시점을 바탕으로 origin, direction을 업데이트한다는 것이 다르다. 

- coords: 마우스의 2차원 좌표, -1 ~ 1의 범위를 갖는다.
- camera: 광선이 시작하는 카메라 지점

#### intersectObject(object: Object3D, recursive: Boolean, optionTarget: Array)

광선과 parameter로 전달한 객체가 교차하는 모든 교차점을 거리가 가까운 순으로 정렬한 배열(Array)을 반환한다. 

- object: 광선과의 교차를 체크할 객체
- recursive: 체크할 object의 자손까지 체크할지에 대한 여부 (기본값: true)
- optionalTarget (optional): 결과를 저장할 배열 지정
  - 새로운 배열을 생성하지 않고 이미 정의된 배열을 사용하므로 메모리를 절약할 수 있다.
  - 단 결과를 저장할 배열은 빈 배열이어야 한다

반환된 Array에는 다음과 같이 구성된 object가 저장되어있다.
`three.js`의 버전에 따라 제공되는 정보가 다를 수 있다는 것에 유의하자

- distance: origin으로부터 교차가 일어난 지점까지의 거리
- point: 교차가 일어난 좌표
- face: 교차된 면의 방향
- faceIndex: 교차가 일어난 면의 Geometry Index를 반환
  - Three.js의 물체(Mesh)는 여러개의 삼각형으로 구성되는데, 교차되는 면이 geometry에서 몇번째 삼각형에 해당하는지를 반환하는 것이다.
- object: 교차가 일어난 물체
- uv: 교차가 일어난 지점의 uv좌표
- uv1: 교차가 일어난 지점의 second uv set ← 솔직히 잘 모르는 개념..
- normal: 교차가 일어난 지점의 법선 벡터(normal vector)
- instanceld: InstancedMesh와 교차되었을 경우 해당 InstancedMesh의 인덱스 번호

물체의 뒷면(Backside)에 대해서는 Raycasting이 일어니지 않는다, 만약 원한다면 THREE.DoubleSide로 뒷면까지 렌더링해야한다.

#### intersectObjects(objects: Array, recursive: Boolean, optionTarget: Array)

intersectObject 와 동일한 동작을 한다. 대신 여러개의 object를 배열에 담아 인자로 전달하여 동시에 체크할 수 있다.

        