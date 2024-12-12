

        ---
        title: [three] BufferAttribute.setUsage()
        date: 2023-12-29 00:52:16.085 +0000
        categories: [three-js]
        tags: ['til', 'three.js']
        description: 처음보는 setUsage 메서드에 대한 정리
        
        
        ---

        # setUsage()가 필요한 이유

three.js는 OpenGL에 기반해 3D 물체를 렌더링하므로, OpenGL의 API를 사용한다.

OpenGL은 성능 최적화를 위해 해당 Buffer 객체를 어떻게 다룰 것인지(읽기, 쓰기 및 접근 빈도 )에 대한 힌트를 `Usage`를 통해 명시할 수 있게 하였다.

OpenGL이 정한 상수를 Usage로 명시하면 OpenGL이 해당 Buffer 객체를 다룰 때 메모리 할당 및 데이터 전송을 최적화할 수 있다.

# Usage 종류

크게 두 가지 관점에서의 Usage가 있다.

## 용도

- DRAW : 사용자가 buffer 객체를 자주 변경하지만, GL만 렌더링을 위해 접근
  - 일반적으로 vertex 정보를 저장하는 buffer 객체가 여기에 해당
- READ : buffer 객체를 자주 읽지만, 변경하지는 않음
  - buffer 객체를 자주 GL 명령어 대상으로 사용할 때가 여기에 해당
- COPY : buffer 객체를 읽지도, 변경하지도 않음
  - buffer 객체가 데이터 전달용으로만 사용될 때가 여기에 해당


## 변경 빈도

- STATIC : 처음 한 번만 데이터가 설정
- DYNAMIC : 사용자가 데이터를 자주 변경
- STREAM : 거의 항상 사용자가 데이터를 변경하거나 사용

어디까지나 최적화를 위한 힌트를 전달하는 것이기 때문에, STATIC으로 명시한 buffer 객체를 변경해도 상관 없고, 반대로 STREAM으로 명시한 buffer 객체를 변경하지 않아도 상관은 없다.

# BufferAttribute.setUsage

BufferAttribute와 이를 상속하는 모든 THREE 객체에 대해 Usage를 설정할 수 있는 메서드이다.

상수값은 THREE 라이브러리가 제공하며, 모든 Usage 경우의 수에 대응한다.

```javascript
THREE.StaticDrawUsage 
THREE.DynamicDrawUsage 
THREE.StreamDrawUsage
		
THREE.StaticReadUsage 
THREE.DynamicReadUsage 
THREE.StreamReadUsage
		
THREE.StaticCopyUsage 
THREE.DynamicCopyUsage 
THREE.StreamCopyUsage
```

예를들어 

```javascript
instancedMesh.instanceMatrix.setUsage(THREE.DynamicDrawUsage);
```

위 코드는 해당 InstancedMesh의 Matrix 정보에 대해 사용자는 변경만 할 것이며, 자주 변경될 것임을 OpenGL에 전달한다.

# 참고자료

https://threejs.org/docs/index.html?q=instancedMesh#api/en/constants/BufferAttributeUsage
https://www.khronos.org/opengl/wiki/Buffer_Object#Buffer_Object_Usage

        