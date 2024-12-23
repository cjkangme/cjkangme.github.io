

---
title: [ML 스터디] MLOps Deployment and Life Cycling+
date: 2024-10-15 12:41:24.179 +0000
categories: [developer-AI-powered]
tags: []
description: [4주차] 가짜연구소 9기 "AI를 잘 활용하는 개발자로 성장하기" 프로젝트에 참여하여 Datacamp의 "Machine Learning Engineer" 강의를 수강하고 해당 내용을 정리한 게시글입니다.
image: /assets/posts/2024-10-15-ml-스터디-mlops-deployment-and-life-cycling/thumbnail.png

---

> 가짜연구소 9기 "AI를 잘 활용하는 개발자로 성장하기" 프로젝트에 참여하여 Datacamp의 "Machine Learning Engineer" 강의를 수강하고 해당 내용을 정리한 게시글입니다.
> [Datacamp Machine Learning 코스 페이지](https://www.datacamp.com/category/machine-learning)

# Chapter 1. MLOps in a Nutshell
## Modern MLOps Workflows
### MLOps란
ML 개발은 소프트웨어 개발의 하위 집합에 속한다.
하지만 일반적으로 소프트웨어 개발이 코드 작성으로 시작해 배포로 끝나는 것과 달리, 머신러닝의 경우 데이터 수집 및 처리와 모델링 과정이 더해진다.

많은 조직들이 머신 러닝을 시작할 때는, Operation을 고려하지 않고 수동으로 ML 워크플로우를 구성하고, 필요할 때마다 (Ad hoc; 특정 용도를 위한) (수동으로) 모니터링을 한다.

이는 결국 기술적 부채를 초래하여, 추후에는 다양한 문제, 개발 생산성 저하 등으로 인해 비용을 들여 리팩토링을 진행해야 한다.
기술 부채로 인해 더 큰 비용이 발생하는 것을 막기 위해서는 규모에 따라 설계단계부터 MLOps 적용을 검토해야 한다.

### ML workflows
- 워크플로우는 입력으로 시작되어 최종 출력을 내놓기까지의 과정이고, ML 워크플로우는 다음과 같은 과정을 포함한다.
  - 데이터 수집 및 처리
  - 데이터 라벨링
  - 모델 선택
  - 모델 학습
  - 모델 패키징 및 배포
  - 모델 모니터링 및 유지보수
- **MLOps의 성숙도(maturity)는 이 ML 워크플로우의 자동화 정도라고 볼 수 있다.**
## Life-cycling stages
### 머신러닝의 생명주기
머신 러닝의 생명주기는 크게 세 가지로 쪼개서 생각할 수 있다.
- 머신러닝 프로젝트의 생명주기
- 머신러닝 애플리케이션의 생명주기
- 머신러닝 모델의 생명주기

머신러닝 모델은 순수한 예측 기계에 불과하다.

머신러닝 애플리케이션은 머신러닝 모델을 코어로 하지만, 애플리케이션을 완성하기 위한 더 많은 구성요소가 들어가있다. (API, DB, 비즈니스 로직 등)

머신러닝 프로젝트는 비즈니스 전체를 포괄한 개념인데 너무 큰 내용이기 때문에 이번 강의에서는 다루지 않는다고 한다.

많은 머신러닝 모델은 애플리케이션과 모놀리식 아키텍처처럼 강하게 결합되어있지만, 언제든지 양쪽을 자유롭게 업데이트 할 수 있도록 분리하는 것이 좋다.
### 용어 정리
- 모델 : 학습이 완료되어 사용할 준비가 완료된 모델
- 배포 : 모델을 즉시 사용 가능한 상태로 만드는 것
- 모니터링 : 모델 배포 후 모델 서버의 가용성, 모델의 실제 성능을 확인하는 것
- 해체(decommissioning) : 모델을 새 버전으로 교체하여 오래된 모델이 더 이상 쓰이지 않게 되는 것
- 아카이브 : 디커미션된 모델을 저장하는 것
  - 아카이빙된 모델은 재현성이 있어야 한다.
  - 재현성(Reproducibility) : 아카이브된 모델을 즉시 재실행 가능한 것
## MLOps components
### MLOps의 구성 요소
- 파이프라인 (pipeline)
  - 데이터 처리와 모델 학습을 포함한 ML 워크플로우를 자동화하기 위한 액션 스크립트
- 아티팩트 (artifacts)
  - 파이프라인의 출력물
### MLOps 파이프라인
DevOps에서는 크게 빌드 파이프라인과 배포 파이프라인이 존재한다.

그리고 DevOps에서 ML 워크플로우가 추가된 MLOps는, DevOps의 파이프라인 외에 모델을 빌드하고 배포하기 위한 파이프라인이 추가로 필요하다.

#### 빌드 파이프라인

모델 빌드 파이프라인에는 모델의 학습 과정이 포함되며, 이를 위한 데이터 처리 파이프라인이 필요하다.

모델 빌드 파이프라인의 아티팩트는 두 가지로, 학습이 완료된 모델과 모델의 메타데이터이다. 이를 각각 모델 저장소(model registry)와 메타데이터 저장소(metadata store)에 저장한다.

여기서 메타데이터 저장소는 지금까지 빌드된 서로 다른 모델들의 중요한 정보를 저장하는 것이다.

#### 배포 파이프라인
애플리케이션 아티팩트와, 모델 아티팩트를 통합하여 서버 플랫폼에 올리는 파이프라인을 말한다.

또한, 모니터링을 위해 별도의 모니터링 플랫폼을 구성할 수 있다.

# Chater 2. Develop for Deployment

## Deployment-driven development (DDD)
개발된 모델이 정상적으로 배포되기 위해 고려해야 할 것은 다음과 같다.
- 배포 환경과의 호환성
  - 배포 환경의 하드웨어 스펙보다 더 높은 성능을 요구하는 경우 배포가 불가능
- 모델의 투명성과 재현성
  - 투명성은 raw data와 코드가 주어졌을 때 동일한 형태의 모델을 만들어내기 위한 것을 의미
  - 재현성은 모델을 쉽게 다시 배포하거나 수정할 수 있어야 한다는 것
  - 해당 모델의 책임자와 버전, 하이퍼파라미터 등 모델에 대한 정보가 관리되어야 함
  - 이를 달성하기 위해 데이터셋의 버전 관리, 실험 로깅, 메타데이터 스토어를 사용할 수 있다.
- 입력 데이터에 대한 검증 방법
  - 가장 좋은 것은 빌드 파이프라인에 데이터 프로파일링 과정을 포함해 그 결과를 메타데이터 스토어에 저장하는 것
- 성능 모니터링
  - 성능 감쇠를 예방하기 위해 입력과 출력에 대한 로깅이 필요
- 에러 발생 시 디버깅
  - 각 추론 단계별로 로그를 남겨야 에러 발생 시 원인을 추적할 수 있음
- 테스트 용이성
  - 모델의 수정, 애플리케이션 코드의 변경 후 다양한 테스트를 쉽게 수행할 수 있는 구조가 필요

무작정 모델을 개발한 뒤 이러한 것들을 고려하려면 너무 많은 비용이 필요하기 때문에, 모델 개발의 초기 단계부터 배포를 염두에 두고 설계하는 것이 중요하다. 이러한 접근 방식을 DDD라고 한다.
## Profiling, versioning and feature stores
### Data profiling (데이터 프로파일링)
데이터 분석과 고수준의 요약을 자동화하는 것
모델 입력에 대한 검증과 배포된 모델을 모니터할 때 사용됨
1. 잘못된 입력에 대해 사용자에 피드백
2. 재학습 여부 결정에 활용

데이터 프로파일링이 없다면 사용자가 모델에 대해 불만을 제시했을 때 검증할 방법이 없음
- 사용자의 잘못인지, 정말 모델의 잘못된 예측인지

이를 위해 ML 파이프라인을 설계할 때, 메타데이터 스토어에 데이터 프로파일 생성을 고려해야 함
### Data versioning
모델 학습 시, 학습에 사용된 데이터의 버전을 기록해 두는 것
모델의 reproducibility를 위해 사용
만약 학습에 사용되었던 데이터를 찾을 수 없게되거나, 변경된 경우에는 동일한 모델을 만들어 낼 수 없음

데이터셋 뿐만 아니라, 파이프라인에 대한 metadata도 필요
`DVC(Data Version Control)` 툴을 가장 대중적으로 사용함
### Feature Store
특별히 ML 모델 학습을 위한 데이터만을 모아둔 중앙 데이터베이스
여러 모델과 프로젝트에서 데이터를 재사용하기 위해 사용

새 모델 학습, 프로젝트마다 데이터를 다시 준비하는 비용을 절약할 수 있음
일반적으로 ML 생명주기에서 데이터 준비에 가장 많은 시간이 소요된다는 것을 고려할 때 매우 효율적

또한, 가장 잘 처리된 품질 높은 데이터를 항상 사용할 수 있음
## Model build pipelines in CI/CD
### 모델 파이프라인과 모델 빌드 파이프라인의 차이
- 모델 파이프라인 : ML 모델을 중심으로 Raw data 입력으로부터 예측을 출력하는 일련의 자동화된 과정
- 모델 빌드 파이프라인 : 모델 파이프라인과 학습 데이터를 바탕으로 모델을 학습하고, 학습된 모델을 출력하여 저장하는 일련의 자동화된 과정
### 모델 빌드 파이프라인
모델 빌드 파이프라인은 단순히 모델 뿐만 아니라, 버전 관리 및 재사용을 위한 메타데이터를 함께 출력함
앞서 정의한 용어대로 모델 빌드 파이프라인의 아티팩트는 이 모든 출력을 포함하며, 함께 패키징 되어야 함
### 메타데이터에 포함되어야 하는 정보
- 배포에 필요한 dependency 저장
- repoducibility를 위한 모든 파라미터 저장
  - 버전 관리가 된 코드
  - 버전 관리가 된 데이터
- 데이터 프로파일
  - 모델이 정상 작동하는지 모니터링하기 위한 정보

## Model Packaging
모델 개발을 마무리하는 단계이자, 모델 배포 단계의 시작
### Model Storage
모델을 저장할 수 있는 형식은 여러가지이다.
serving 프레임워크가 로드할 수 있고, 배포 환경에서 실행될 수 있는 형식이어야 한다.
#### PMML
- 모델을 학습하는 언어, 프레임워크와, 모델을 사용할 언어, 프레임워크가 달라도 사용이 가능하여 범용성이 높은 형식
- 모델의 커스터마이징이 어렵다는 단점이 있다.
  - 여기서 커스터마이징은 사용자가 원하는 로직을 추가, 교체하거나 PMML이 지원하지 않는 모듈을 추가하는 것을 말함
#### pickle(pkl)
- 파이썬에서 객체를 저장하는 형식
- 파이썬에서만 사용가능하다는 단점이 있지만, 파이썬을 사용한다면 무엇이든지 저장할 수 있어 자유도가 높다
### 메타데이터 저장
- 모델 실행에 필요한 dependencies
- reproducibility를 위한 메타데이터
  - 모델 빌드 파이프라인의 코드 저장소와 학습 당시 사용된 정확한 버전
  - 학습 당시 사용된 데이터베이스와 정확한 버전
  - train test split에 사용한 설정 등 각종 파라미터
  - 테스트 결과 (동일한 모델인지 검증하기 위해)
- 모니터링을 위한 메타데이터
  - 입력 데이터 프로파일
  - 출력 데이터 프로파일
# Chapter 3. Deploy and Run 
## Model Serving
ML 모델을 사용자에게 실제 서비스로 제공하는 것
언제 모델이 실행되는지에 따라 여러가지의 serving 형태가 있다.
### Batch prediction
- 대규모의 입력 데이터를 batch 단위로 예측하는 시스템
- 외부 이벤트와 상관 없이 일정 간격으로 누적된 데이터 batch를 입력으로 모델이 실행됨
### On-demand prediction
- 유저의 요청 시 모델이 실행되는 시스템
- 요청에 대한 실시간성이 중요함
  - 수 분 정도의 지연이 허용된다면 data stream을 처리하는 stream processing 사용 가능 (요청이 넘칠 경우 큐 같은 곳에 넣어두고 순차적으로 처리)
  - Edge Deployment : 매우 짧은 지연시간이 강제될 때 네트워크 통신 없이 사용자의 디바이스에 모델을 직접 배포해 지연시간을 최소화하는 것
## Building the API
- API : 외부에서 모델에 접근 가능하도록 외부에 노출하는 인터페이스
### API의 필수 요소
- 클라이언트의 잘못된 요청(입력)에 대한 검증 및 적절한 처리
  - 유효한 입력을 정의해둔 데이터 스키마를 Request model이라 한다. 보통 이러한 model을 통해 입력 데이터의 유효성을 검증한다.
- 출력 데이터(모델의 예측 결과) 검증 및 적절한 처리
- 요청 검증
  - 요청을 보낸 사용자가 모델에 접근 가능한 유효한 사용자인지 검증
- API 쓰로틀
  - 과도한 요청을 방지하기 위해 단시간 내의 과도한 요청을 쓰로틀을 통해 방지
## Deployment progression and testing
모델이 사용자에게 배포되기 전에는 철저한(thoroughly) 검증을 거쳐야 한다.
이번 섹션에서는 배포 전에 진행해야 할 테스트에 대해 다룬다.
여기서 테스트는 모델의 성능이 아닌, 모델을 포함한 전체 애플리케이션이 잘 동작하는지에 대한 테스트를 말한다.
### Test Environment
테스트는 모든 코드의 변경, 모델의 변화에 대해 실행되어야 하므로, 실시간 수정이 일어나는 개발 환경에서는 잦은 테스트가 생산성을 저해할 수 있다.
때문에 테스트를 실행하지 않는 개발 환경(Dev env)와 배포 전 테스트를 수행하는 테스트 환경(Test env)를 별도로 나누는 것이 바람직하다.
이 외에 모델이 즉시 배포 가능한지를 테스트하는 스테이징 환경, 실제 배포가 된 배포 환경이 있다.
### Unit test
- 예시 입력과 출력을 바탕으로 모델이 기대하는 출력을 내는지 확인하는 테스트
### Integration test
- 애플리케이션의 내부 컴포넌트간 또는 애플리케이션 간의 각 모듈이 정상 동작하는지 확인하는 테스트
- ex) A->B->C 로직에서 B의 출력이 C의 유효한 입력인지 검증하는 등
- 일반적으로 배포 환경을 복사한 스테이징 환경에서 테스트 진행
### Smoke test
- 애플리케이션이 오류 없이 배포되는지 테스트
- 스테이징 환경에서 테스트 진행
### Load test
- 스테이징 환경에서 실제 배포와 유사하게 요청을 주어 모델이 정상작동 하는지 확인하는 테스트
- 만약 입력 조건이 가혹하다면 Stress testing이라고 한다.
### User Acceptance test (UAT)
- 실제 테스터 그룹을 모집해 모델을 사용하도록 해보는 테스트
## Model Deployment Strategies
새로운 모델을 배포할 때 서비스 중단 없이 새로운 버전을 서빙하기 위해서는 배포 전략이 필요 
### Blue/Green 전략
- 애플리케이션에 이전 모델과 새 모델을 동시에 배포하여, 새 모델이 사용 가능한 상태가 되면 API 요청을 이전 모델에서 새 모델로 한 번에 전환하는 전략
- 가장 단순한 전략이지만, 새 모델이 정상 동작하지 않을 경우 모든 사용자가 피해를 입을 위험이 있다
### Canary 배포 전략
- 새 모델을 배포한 후에도 요청이 이전 모델로 전송되도록 하며, 점진적으로 새 모델로 전송되는 트래픽을 높이는 방법
- 새 모델을 충분히 검증한 뒤에 완전히 전환할 수 있다.
### Shadow 배포 전략
- 하나의 요청을 이전 모델과 새 모델로 모두 전송하고, 이전 모델의 출력을 사용자에게 전달하고, 새 모델의 출력은 검증용으로 사용하는 전략
- 새 모델의 성능을 확실하게 검증할 수 있지만, 트래픽이 2배가 되기 때문에 성능 이슈가 발생할 수 있다. 
# Chapter 4. Monitor and Maintain
## Monitoring ML services
### Performance indicators (성능 지표)
- 모델의 가용성
- 모델의 대역폭? (어느정도의 요청까지 처리할 수 있는지)
- 응답까지의 소요 시간
- **예측 정확도**
우선은 정확도가 높아야 나머지 지표가 의미가 있음
### Concept Drift
- 입력 데이터와 feature 간의 관계가 바뀌는 현상
	- ex) 수요 예측 모델에서 물가 상승으로 인해 가격이 (학습된 데이터보다) 높아도 과거보다 더 많이 구매하는 경우
- Concept Drift를 빠르게 감지해서 모델을 최신 데이터로 재학습 해야한다.
### Concept Drift를 감지하는 법
1. 모델의 예측을 저장했다가, 실제 결과가 나왔을 때 정확도를 모니터링
- 그러나 verification latency가 너무 클 경우 제대로 적용하기 어렵거나 실제 결과를 아예 관측할 수 없는 경우에는 이 방법을 적용하기 어려움
  - verification latency : 실제 결과를 확인하기 까지 걸리는 시간
2. 입력 feature의 분포를 시각화하여 covariate shift를 감지
- covariate shift : 입력 feature의 분포가 바뀌는 현상
  - 이 경우 concept drift와 어느정도 유사한 개념으로 볼 수 있다.
  - 하지만  covariate shift가 발생해도 concept drift가 발생한 것이 아니거나, 그 반대일수도 있으므로 여러 관점으로 살펴보아야 한다.
## Monitoring and alerting
- 모델의 문제를 가능한 빠르게 발견하고 조치하기 위해서는 모니터링과 함께 alerting 체계를 갖추는 것도 중요하다.
- ML 애플리케이션은 여러 컴포넌트로 구성되어있으므로, alerting은 문제가 어디서 발생했는지 자세히 알려줄수록 좋다.
### 엄밀한 Alerting을 하는 법
- 가능한 자세한 logging system 구축
- 서비스 내에서 전달되는 데이터에 대해 상세한 validation 수행
  - input, output 뿐만 아니라 컴포넌트 간의 전달, 데이터 가공 전후로 validation을 수행
  - alerting 뿐만 아니라, feature의 분포를 측정하기에도 좋은 방법
- Statistical validation
  - 경고 상황에 대한 threshold를 설정하여 threshold를 넘은 상황에 대해서만 alerting
  - ex) 입력 후 응답시간이 1000ms 이상인 경우 alert
  - 적절한 threshold를 설정해야 너무 많은 alert를 발생시키지 않으면서, 중요한 alert를 인지할 수 있음
문제가 발생하여 수정했다면, 이를 잘 기록하여 이후 개발 시 같은 실수를 반복하지 않는 것이 중요
## Model maintenance
- 모델의 성능 저하가 발생했을 때 조치
  - 모델의 구조 개선 (MODEL-centric approach)
  - 학습 데이터 강화 (DATA-centric approach)

모델 구조 개선에 비해 데이터를 더 많이 모으고, 더욱 고품질로 정제하는 것의 자유도가 더 높기 때문에 최근에는 DATA-centric approach가 더 각광받고 있다고 한다.
또한 경험적으로 같은 노력을 했을 때, 모델 구조 개선보다 학습 데이터 강화가 더욱 많은 성능 개선을 이끌었다고 한다.
### Quality > Quantity
- 여기서 말하는 데이터의 품질은 Ground truth에 가깝게 라벨링 되어있는 데이터를 말한다.
  - ex) segmentation 모델의 학습 데이터셋에서는 정교한 mask가 제공될수록 고품질 데이터라 할 수 있음
- 고품질 데이터를 만들기 위해 좋은 labeling tools를 선택하는 것도 중요하다
### Human in the loop system
- 수동 labeling이 어렵다면, confidence를 별도의 지표로 사용하여, 모델의 예측 중 confidence가 낮은 데이터에 대해서만 전문가에게 판단을 요청하고, 이를 다시 모델 학습에 사용할 수 있음
## Model governance
- 모델 예측 결과의 접근을 제어하고, 모델과 그 결과에 대한 활동을 추적하는 전반적인 프로세스
- 모델 거버넌스의 목적은 조직의 이익 및 브랜드에 대한 위험을 최소화하는 것

모델의 잘못된 예측에 대한 위험을 정확히 평가하고, 모델 배포 의사결정에서 고려해야 한다.
### 디자인 단계의 거버넌스
다음과 같은 사항을들 검토할 수 있다.
- 모델이 AI 윤리에 적합한지 여부
- 민감 정보 사용여부와, 적절한 처리 방안
- 잠재적인 모델에 대한 위험성 판단
### 개발 단계의 거버넌스
- 모델 개발 과정에 대한 투명성
- 모델에 대한 추적성 마련
일반적으로 버전 관리와 metadata store를 잘 구축하는 것으로 목적을 달성할 수 있다.
### 배포 단계의 거버넌스
- API가 안전한 것인지 검토
- alerting 시스템 구축
- 모니터링 및 검증에 대한 절차 및 문서화 체계 마련
### 모델의 리스크 관리
- AI/ML 분야라고 특별하지는 않다
- 발생 빈도와 심각성을 기반으로 위험성을 평가하여 각 위험성에 대한 적절한 대처를 마련해야 한다.

모델의 빠른 배포가 우리의 목표가 아닌 것을 명심하고, 귀찮더라도 거버넌스를 잘 지켜 비즈니스 가치를 창출할 수 있도록 하는 것이 중요하다.
MLOps는 모델의 예측이 많을 수록 중요하다. 때문에 무조건 높은 수준의 MLOps를 구축하려고 하기보다는 규모에 맞게 점진적으로 수준을 높이는 것이 좋은 방법이 될 수 있다.

# 소감
## 알게 된 점
- 모델 배포 전 어떤 테스트를 거쳐야하는지 명확히 알 수 있었습니다.
- concept drift를 감지하기 위해 input data의 분포를 확인하는 것도 중요하다는 것을 알게 되었습니다.
- PMML이라는 모델 저장 형식을 알게되었습니다.

## 느낀점
DevOps에 대해 배웠던 1주차와 겹치는 내용이 많다는 생각이 들었습니다. 그런데 모델 빌드 파이프라인에 들어가면서 ML 배포시 고려해야할 점들, 그리고 데이터 처리나 모델 학습 단계에서는 미처 생각하지 못하는 모니터링, alerting 등을 자세히 다루면서 '작동하는' 모델을 배포하기 위해 이런 것들도 많이 고려해야하는 구나를 느꼈습니다.

거버넌스 부분에서 위험성 평가에 대해 얘기할 때 제조업에서 말하는 위험성 평가와 컨셉이 너무 비슷해서 결국 위험 관리를 위한 본질은 같다는 것을 느꼈습니다.




        