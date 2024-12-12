

        ---
        title: [논문 공부] Compact 3D Gaussian Representation for Radiance Field
        date: 2024-05-18 03:29:56.058 +0000
        categories: [논문]
        tags: ['cv', '논문']
        description: (CVPR 2024 Highrights) 수백MB~수GB가 필요한 3D 가우시안을 25배로 압축하여, 가우시안 저장 및 렌더링에 필요한 메모리를 획기적으로 절약할 수 있는 논문
        image: /assets/img/posts/2024-05-18-논문-공부-compact-3d-gaussian-representation-for-radiance-field/thumbnail.png
        
        ---

        # 논문 소개

제한된 입력(이미지, 텍스트 등)으로 3D Scene을 복원하는 문제는 컴퓨터 비전에서 매우 활발하게 연구되고 있는 분야입니다.
그 중 `NeRF`는 상당한 수준의 high-fidelity한 3D 복원이 가능했기 때문에 3D reconstruction 분야 발전에 큰 기여를 하였습니다.
하지만 NeRF는 volumetric rendering 방법을 사용하기 때문에 렌더링에 많은 연산이 필요해 실시간 렌더링이 어렵다는 단점이 있었습니다.

NeRF 이후 `3D Gaussian Splatting(3DGS)`이라는 방법이 새롭게 등장했습니다. 3DGS에 대해 자세한 설명은 [이전 게시글](https://velog.io/@cjkangme/3D-Gaussian-Splattingfor-Real-Time-Radiance-Field-Rendering)을 참고 부탁드립니다.
3DGS는 기존 컴퓨터 그래픽스가 이용하던 rasterization pipeline을 사용할 수 있기 때문에 비교적 low-end GPU에서도 실시간 렌더링이 가능하다는 장점을 갖습니다.

하지만 3DGS 역시 어플리케이션 수준에서 사용하는 것에 방해가되는 큰 단점을 갖고 있는데, 바로 메모리 문제입니다.

![](/assets/img/posts/2024-05-18-논문-공부-compact-3d-gaussian-representation-for-radiance-field/img0.png)
<small>출처 : [Radiance Field Article - Shrinking 3DGS File Size](https://radiancefields.com/shrinking-3dgs-file-size)</small>

Large Scene을 표현하는 3D 가우시안을 저장하기 위해서는 수백 메가바이트 ~ 수 기가바이트 단위의 용량이 필요하고, 렌더링 시에는 이 용량이 메모리에 적재되어야 합니다. 이는 일반 사용자 입장에서 매우 부담스러울 수 있습니다.

이러한 가우시안의 메모리 이슈를 해결하기 위해 이 논문은 `compact 3D gaussian representation(compact 3D)` 프레임워크를 제안합니다.

논문에서는 가우시안을 보다 컴팩트하게 표현하기 위해 두 가지 키 목표를 설정하였습니다.

![](/assets/img/posts/2024-05-18-논문-공부-compact-3d-gaussian-representation-for-radiance-field/img1.png)


### 1. scene 표현을 위해 필요한 가우시안의 수 줄이기
이를 달성하기 위해 기존의 학습 과정(original 3DGS)에 opacity, scale 기반의 마스킹 기법을 사용합니다.

마스킹된 가우시안을 scene에서 제거함으로써 저장해야 할 가우시안의 수 자체를 줄이고, 렌더링에 필요한 연산 역시 선형적으로 줄일 수 있다고 합니다.

### 2. 개별 가우시안의 용량 줄이기
구체적으로는 각 가우시안이 갖는 속성값들을 압축하기 위해 여러가지 기법을 적용합니다.

#### color attribute
각 가우시안들은 view-dependent color를 표현하기 위한 값을 속성으로 갖고 있습니다.

하지만 scene에서 이웃하는 가우시안들은 대체로 비슷한 색상을 갖고 있기 때문에, 개별 가우시안이 각자 color 속성값을 갖고 있는 것은 비효율적이라 할 수 있습니다.

이 논문에서는 `Instant NGP` 논문의 `hash-based grid representation`을 통한 색상 표현 기법을 3DGS에 적용하였습니다. 이를 통해 가우시안 저장에 필요한 용량을 상당히 줄였다고 합니다.

#### covariance(scale, rotation)
color와 마찬가지로 scale, rotation으로 표현되는 가우시안의 기하학적 성질 역시 대체로 비슷하기 때문에 중복성을 제거하고 효율적으로 저장할 수 있습니다.

Compact 3DGS에서는 학습가능한 codebook을 이용하여 scale, rotation 값을 직접 저장하는 대신 codebook의 인덱스만 저장하도록 하는 방식으로 컴팩트한 표현을 사용하였습니다.

<br />

이러한 압축 기법들을 통해 렌더링 품질을 비슷하게 유지하면서도 약 15배 가량 용량을 줄였고, 렌더링 속도도 향상시킬 수 있다고 합니다.

# 논문에 대한 상세 내용

가짜연구소에서 진행한 스터디 모임에서 해당 논문에 대한 발표를 진행하였습니다.

다시 정리하려니 시간을 너무 많이 쓸 것 같아서...
발표한 영상으로 대체하려고 합니다.

혹시 윗 내용을 보고 관심이 생기셨다면 시청해주시면 감사하겠습니다. (사진 또는 링크 클릭)

[![](/assets/img/posts/2024-05-18-논문-공부-compact-3d-gaussian-representation-for-radiance-field/img2.png)
](https://youtu.be/VLZju5oWIOM)
[유튜브 영상](https://youtu.be/VLZju5oWIOM)


        