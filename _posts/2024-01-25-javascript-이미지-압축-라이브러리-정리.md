

        ---
        title: javascript 이미지 압축 라이브러리 정리
        date: 2024-01-25 10:12:34.325 +0000
        categories: [javascript]
        tags: []
        description: 프로젝트에 사용할 라이브러리 조사
        
        
        ---

        # browser-image-compression

github: https://github.com/Donaldcwl/browser-image-compression/issues/189

npm: https://www.npmjs.com/package/browser-image-compression

## 특징

- 가장 높은 다운로드 수 (약 12만)
- 최신 업데이트가 가장 최근 (8개월 전)
- 이슈
    - 일부 안드로이드 디바이스에서 압축 시 이미지가 회전하는 이슈
    - BMP 확장자 파일에 대해 정상적으로 작동하지 않는 경우가 많음
    - 이미지 용량이 클 경우 0byte broken 이미지가 생성 되는 이슈
    - 한번에 많은 이미지를 압축할 경우 black 이미지가 출력될 수 있음
        - 위 두 이슈는 브라우저의 메모리 사용량이 원인으로 기기별, 브라우저별로 에러 발생 여부가 다름. 제작자는 한번에 여러 이미지 처리할 경우 최대한 순차적으로 처리할 것을 권장
        

다른 이슈는 괜찮다 쳐도, 이미지 회전 이슈는 치명적인데 어떤 경우에 오류가 발생하는지 찾기도 힘들어 적용이 어려울 것 같다.

## compressorjs

github: https://github.com/fengyuanchen/compressorjs

npm: https://www.npmjs.com/package/compressorjs

- 다운로드 수 약 10만
- 최근 업데이트 1년 전
- 이슈
    - PNG 이미지 압축 후 transparency를 잃는 이슈
    - 압축 후 metadata가 사라지는 이슈
    - IOS에서 압축 시 이미지가 회전하는 이슈
        - 제작자가 CSS의 `image-orientation` 속성을 설정하거나, `strict` 옵션을 `false`로 설정하는 등의 해결책을 제시해 주고 있다.
        - browser-image-compression처럼 많은 사람이 이슈를 겪는 것은 아닌듯 하다.

그나마 이슈가 앱 사용성에 미칠 영향이 작은 편인 것 같아서 가장 먼저 채택을 시도해 보기로 함

## react-image-file-resizer

github: https://github.com/onurzorluer/react-image-file-resizer#readme

npm: https://www.npmjs.com/package/react-image-file-resizer

- 다운로드 수 약 7만
- 최근 업데이트 약 2년 전
- 이슈
    - Vite에서 빌드가 되지 않음
        - 라이브러리의 특정 파일을 직접 코드베이스에 가져와서 import하는 방식으로 해결은 가능한데 좋은 방법인지는 모르겠음
    - React 17버전에서 이미 호환이 되지 않는다는 이슈가 일부 존재

2023년 이후 이슈에 대한 답글이 없는 등 관리되고 있지 않는 것 같아 사용하면 안될 것 같다.

        