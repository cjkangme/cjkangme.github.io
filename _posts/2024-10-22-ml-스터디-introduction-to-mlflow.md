

---
title: [ML 스터디] Introduction to MLflow
date: 2024-10-22 10:34:27.601 +0000
categories: [developer-AI-powered]
tags: []
description: [5주차] 가짜연구소 9기 "AI를 잘 활용하는 개발자로 성장하기" 프로젝트에 참여하여 Datacamp의 "Machine Learning Engineer" 강의를 수강하고 해당 내용을 정리한 게시글입니다.
image: /assets/posts/2024-10-22-ml-스터디-introduction-to-mlflow/thumbnail.png

---

> 가짜연구소 9기 "AI를 잘 활용하는 개발자로 성장하기" 프로젝트에 참여하여 Datacamp의 "Machine Learning Engineer" 강의를 수강하고 해당 내용을 정리한 게시글입니다.
> [Datacamp Machine Learning 코스 페이지](https://www.datacamp.com/category/machine-learning)

# 1. Introduction to MLflow & MLflow Tracking
## What is MLflow?
MLFlow는 모델 생명주기의 전 과정에 걸쳐 다양한 자동화 기능을 제공하여 MLOps를 지원한다.
### MLflow의 컴포넌트
MLflow는 크게 네 가지의 컴포넌트를 지원한다.
- MLflow Tracking : ML 모델의 학습에 사용된 데이터와 그 결과에 대한 추적을 지원
- MLflow Models : 배포 자동화를 위한 모델 표준 패키징을 지원
- MLflow Registry : 모델의 버전관리, 배포를 지원
- MLflow Projects : ML 프로젝트에 사용된 코드를 패키징하여 반복, 재사용을 지원
### MLflow experiments
- MLflow의 실험 관리를 위한 플랫폼으로, 모델의 학습 파라미터, 사용된 데이터, 결과물 등을 관리
- `client`와 `mlflow` 두가지 모듈을 지원한다.
  - `client` : 저수준 API로 관리할 데이터들을 자유롭게 커스텀 가능
  - `mlflow` : 고수준 API로 `set_experiment` 메서드 하나로 모듈내의 데이터 추적을 자동화하여 제공
## MLflow Tracking
ML 실험에 대한 추적 플랫폼이 필요한 이유는, 목표 달성을 위해 학습해야 할 모델이 많아질수록 알고리즘, metrics, 사용 데이터 등이 서로 달라 이들을 전부 관리하는 것이 어려워지기 떄문이다.

MLflow Tracking은 metrics, 파라미터 등을 등록하여 쉽게 관리할 수 있게한다.
### mlflow를 이용한 학습 관리
- `mlflow.start_run()` : 실험 시작을 선언하는 API. 프로그램이 종료되거나 `end_run()`을 실행할 때까지 로깅된 모든 데이터와 아티팩트들을 지정된 uri에 저장한다.
- Metrics logging
  - `log_metric`, `log_metrics` : metric 로깅
  - `log_param, log_params` : 파라미터 로깅
  - `log_artifact`, `lof_artifacts` : 아티팩트 로깅
- UI 시각화
  - `mlflow ui`를 명령어로 실행하면 로컬호스트에서 로깅된 결과를 UI로 확인가능
## Querying runs
`mlflow`는 한 run에 대한 시각화 뿐만 아니라, 여러 run을 조회하고, 서로 비교할 수 있는 쿼리 기능을 제공한다.
### mlflow run 조회
- `mlflow.search_runs()` : 현재 mlflow에 저장된 runs 정보를 반환
  - 기본값으로 pandas의 dataframe 형식을 반환
  - run_id, metric.metric1, metric.metric2, experiments_name 등등 다양한 형태로 조회가 가능하니 세부 조회를 하기 전에 전체적인 runs를 확인하기 좋음
- 인자로 query를 넣어 다양한 조건으로 조회 가능
  - `max_results` : 반환할 run의 최대 개수 지정
  - `order_by` : 수치값 기준 정렬
  - `filter_string` : 조건을 문자열 형태로 지정하여 query string 기반 검색
  - `experiment_names` : 실험 이름으로 특정 실험 검색

#### 사용 예시
```python
f1_score_filter = "metrics.f1_score > 0.60"
# experiment1 실험에서 f1 score가 0.6보다 큰 runs를 recall 기준으로 내림차순으로 정렬하여 반환
mlflow.search_runs(experiment_names=["experiment1"],
				  filter_string=f1_score_filter,
				  order_by=["metrics.recall DESC"])
```

# 2. MLflow Models
여러 라이브러리에서 개발된 ML 모델에 대한 표준화된 패키징을 지원하는 컴포넌트
기본값으로 `Flavors`라는 포맷으로 패키지가 저장된다.
표준화된 패키지는 MLflow가 지원하는 다양한 유명 ML 플랫폼에서 배포를 지원한다.
- pytorch, keras, sklearn, spark, tensorflow, onnx 등
## MLflow Models 예제
mlflow에서는 `autolog()` 함수를 통해 명시적인 로깅 정의 없이 metric, parameter, model 등을 자동으로 저장할 수 있다.
### autolog가 지원하는 로그
- metrics
  - Regression
    - MSE
    - RMSE
    - MAE
    - r2 score
  - Classification
    - precision
    - recall
    - f1 score
    - accuracy
- parameters : `model.get_param()`으로 얻을 수 있는 파라미터들을 저장
### sklearn 예제
```python
import mlflow
from sklearn.linear_model import LinearRegression

mlflow.sklearn.autolog()
lr = LinearRegression()
lr.fit(X, y) # Auto logging
```
### 저장 형식
autolog를 통해 모델의 정보를 저장할 경우 다음과 같은 형식으로 저장된다.
```
model
-- MLmodel  # 저장 위치, 사용 라이브러리, 버전 등 중요 정보에 대해 yaml 형식으로 저장, 이후 모델을 import할 때 라이브러리가 정보를 참조하기 위해 사용
-- model.pkl
-- python_env.yaml
-- requirements.txt
```
## Model API
MLflow의 Model 모듈은 모델을 저장, 로드, 로깅하기 위한 다양한 API 함수를 지원한다.
예를들어 MLflow는 sklearn을 내장하고 있어 다음과 같이 사용할 수 있다.
```python
mlflow.sklearn.save_model(model, path)  # 앞서 배운 표준 형식으로 저장
mlflow.sklearn.load_model(model_uri)
```
### 모델 로드 방법
MLflow에서는 모델 로드를 위해 절대경로, 상대경로 외에 다양한 방법을 제공한다.
- MLflow Tracking : `runs:/<run_id>/relative-path-to-model`
- S3 bucket : `s3://my_bucket/path-to-model`
### Last Active run
run을 불러왔다면 `last_active_run()` 함수를 통해 마지막 run을 재현성 있게 실행할 수 있다.
`last_active_run()`이 반환하는 객체는 마지막 run의 정보를 갖고 있어, 해당 run의 정보를 조회 가능하다.
## Custom models
MLflow는 강력한 API를 지원해주기는 하지만, 모든 상황에 대응할 수는 없다. 때문에 사용자가 커스텀 해야하는 경우 Model Customization 기능을 지원한다.

`mlflow.pyfunc` 모듈은 파이썬 함수를 FLAVOR 형식으로 사용 가능하도록 지원한다.
### Custom model class
- `mlflow.pyfunc.PythonModel`을 상속하는 클래스를 만들어서 FLAVOR 형식으로 저장하기 위한 코드와 함수를 정의
  - 다음의 메서드가 정의되어야 함
  - `load_context` : `load_model()`이 호출되었을 때 실행되는 메서드
  - `predict` : 로드된 모델에 대해 예측을 했을 때 실행되는 메서드
### 커스텀 모델의 저장
경로와 함께 커스텀 파이프라인 클래스를 호출하여 저장하면 된다.
- `mlflow.pyfunc.save_model(path="custom_model", python_model=CustomPredict())`
- `mlflow.pyfunc.log_model(artifact_path="custom_model", python_model=CustomPredict())`
### 모델 평가
`mlflow.evaluate()` 함수를 통해 데이터셋을 기반으로 모델의 예측 결과를 평가할 수 있다. (predict 함수가 정의되어있기 때문에가능)

```python
# 사용 예시
mlflow.evaluate("runs:/run_id/model",
			   eval_data,
			   targets="test_label",
			   model_type="regressor") # [regressor, classifier]
```


evaluate를 진행하면, 자동으로 `shap` 시각화 라이브러리로 시각화된 plot file이 생성된다.
## Model serving
모델을 표준화된 형식으로 패키징했기 때문에 쉽게 배포할 수 있다.
### 모델 서빙
- `mlflow models serve [OPTIONS]`
  - 기본적으로는 로컬호스트의 5000번 포트에 배포된다.
  - 자세한 사용법은 `mlflow models serve --help`를 통해 확인할 수 있다.
### REST API
모델을 mlflow로 배포할 경우 모델에 접근 가능한 REST API를 제공한다.
- `/ping`, `/health` : mlflow 서버의 health check를 위한 API
- `/version` : MLflow의 버전
- `/invocations` : 배포된 모델의 스코어링
  - json 또는 csv를 payload로 받는다 (application/json, application/csv)
  - `pandas_df.dataframe_split()` 함수를 통해 json 형식으로 변환이 가능하다.
# 3. MLflow Model Registry
## Introduction to MLflow Model Registry
모델의 생명주기 동안 모델은 개발 환경, 스테이징 환경, 배포 환경 등 서로 다른 환경에서 관리되어야 한다.
MLflow의 Model Registry는 다양한 환경들에 대해 하나로 통합된 MLflow 중앙 저장소에서 관리할 수 있는 컴포넌트이다.
### MLflow Model Registry
Model Registry는 크게 모델의 상태를 크게 4개로 구분하여, 모델이 생명주기에서 어떤 상태인지를 나타낸다.
1. None : MLflow에 의해 추적되고 있는 상태 (logging)
2. Staging : MLflow에 등록된 상태
3. Production
4. Archived
### MLflow client module
MLflow의 `client` 모듈을 통해 MLflow에 모델을 등록하고, 등록된 모델(Registered Model)을 관리할 수 있다.
client 모듈은 `MlflowClient` 클래스의 인스턴스를 기반으로 동작한다.
```python
from mlflow import MlflowClient

client = MlflowClient()
```
- `create_registered_model()` 메서드를 통해 모델을 MLflow에 등록
- 등록된 모델은 UI에서 확인이 가능하며 `search_registered_models` 메서드로 필터 스트링에 기반하여 검색
  - ex) `"name LIKE 'Unicorn%'"` : Unircorn을 모델 이름으로 포함한 모든 Registered model 검색
## Registering Models
### MLflow에 모델을 등록했을 때 이점
- 모델의 버전 관리 가능
- 쉬운 모델 관리를 통해 원활한 협업
- 자동화를 위한 MLflow의 다양한 기능을 지원받을 수 있음
### MLflow에 모델을 등록하는 방법
- `mlflow.register_model(model_uri, name)`
  - `model_uri`는 로컬 디렉토리나 load_model과 동일한 문법을 이용할 수 있음
  - 새로운 모델을 학습하면서 등록할 때는, `log_model`을 호출할 때 `registered_model_name` 인자를 지정하여 해당 이름으로 모델의 아티팩트를 저장하며 함께 등록할 수 있음
- 동일한 이름으로 모델을 등록할 경우 버전이 1씩 증가하여 등록이 이루어짐
## Model stages
- 실제 ML 모델의 생명주기에서 서로 다른 모델의 상태(개발 환경, 배포 환경)를 관리하기 위해 MLflow는 Model stages 기능을 제공한다.
### Model stages
- 모델의 각 버전은 MLflow가 제공하는 4개의 stages 중 한가지의 상태만 가질 수 있다.
  1. None : 등록 후 별도의 상태를 받지 않은 default 상태
  2. Staging : 테스트 및 평가 단계의 상태
  3. Production : 모델이 모든 테스트를 완료하고 배포할 준비가 된 상태.
      - 버전이 여러개여도 Production 버전은 단 하나만 존재할 수 있다. 만약 이미 Production stage인 모델이 있을 때 다른 모델의 stage를 production으로 변경하면 기존 모델의 stage는 설정에 따라 staging 또는 archived로 변경된다.
  4. Archived : 더 이상 사용되지 않는 상태
### Stage 변경 방법
- UI를 통해 쉽게 변경 가능
- `client` 인스턴스의 `transition_model_version_stage(name, verstion, stage)` 메서드에서 모델의 이름과 버전을 지정하여 특정 stage로 변경할 수 있다.

> [정보]
> https://mlflow.org/docs/latest/model-registry.html#migrating-from-stages
> MLflow 2.9.0 버전 부터는 보다 유연한 관리를 위해 Model stages가 deprecate된다고 한다.
> 이후로는 model version tags와 model verion aliases를 제공하여 모델의 환경을 관리할 수 있도록 한다고 한다.
> model version tags는 docker의 tag와 유사하게 <모델이름>@<태그이름>으로 구성되어, 태그별로 모델을 구분할 수 있다.

## Model Deployment
### 모델 배포 방법
MLflow는 Model registry에 등록된 모델을 배포하기 위해 두 가지 옵션을 제공한다.
1. `load_model()` 함수
2. `mlflow models serve` 명령어 (CLI)

특정 모델을 지정해서 배포하기 위해 model uri가 필요하다
### Models URI
버전이나 stage 상태를 이용해서 특정 버전 또는 특정 스테이지의 모델을 지정할 수 있다.
- `models:/model_name/version or stage`
### 모델 로딩 예제
#### 파이썬
```python
import mlflow.sklearn

model = mlflow.sklearn.load_model("models:/Unicorn/Production")
```
#### CLI
```bash
mlflow models serve -m "models:/Unicorn/Production"
```

모델은 기본적으로 5000번 포트에 서빙되며, API를 통해 데이터를 보낼 수 있다.
데이터는 csv 또는 json 형태여야 한다.
# 4. MLflow Projects
## Introduction to MLflow Projects
MLflow Projects 컴포넌트는 ML code를 재실행하고, 모델을 재생산 할 수 있는 방법을 제공
코드를 MLflow가 실행 가능한 단위로 패키징하여 재현성을 보장한다.
### Projects artifact
```
project/
-- MLproject
-- train_model.py
-- python_env.yaml
-- requirements.txt
```

`MLproject` 파일은 프로젝트에 대한 프로퍼티들을 저장한 yaml 파일로, 프로젝트를 불러올 때 참조된다.
- `name`, `entry_points`, `python_env` 등을 저장
수동으로 yaml을 작성하여 Projects에 다양한 설정을 할 수 있다.
### MLproject 파일 작성 예시
```yaml
name: insurance_model
python_env: python_env.yaml
entry_points:
  	main:
    	command: 'python3.9 train_model.py'
```
## Running MLflow Projects
MLflow Projects는 API 또는 CLI 명령어를 통해 실행할 수 있다.
### Projects API
mlflow는 `projects`라는 모듈을 제공하여 MLflow Project 관련 함수를 지원한다.

- `mlflow.projects.run()` 으로 로컬 저장소 또는 git 저장소에 있는 프로젝트 실행 가능
- 인자로 `uri`, `entry_point`, `experiment_name`, `env_manager` 등을 설정 가능
### 사용 예제
#### API
```python
mlflow.projects.run(
					uri="./", # 실행할 프로젝트의 경로
					entry_pont="main",
					experiment_name="Salary Model"
)
```
#### CLI
```bash
mlflow run --entry-point main --experiment-name "Salary Model" ./
```
- 필수 argument인 uri를 마지막에 넣어주면 된다.

## Specifying  parameters
MLflow Projects 이전 코드를 그대로 재사용 할 수도 있고, 파라미터를 사용하여 유연하게 커스텀 할수도 있다.
이러한 기능은 하이퍼파라미터 튜닝과 같이 동일한 환경에서 일부 설정을 변경하면서 ML 모델을 탐색하는 활동에 유리하다.
### 파라미터 정의
파라미터 정의는 MLproject 파일의 entry_points 내에서 할 수 있다.
```yaml
entry_points:
	main:
		parameters:	
			parameter_1_name:
				type: data_type  # float, string 등 파이썬이 지원하는 데이터 타입
				default: default_value
			parameter_2_name:
				type: ...  # default: string
```
### 파라미터 사용
전달된 파라미터는 다음과 같이 가져올 수 있다.
```python
import sys

parameter_1_name = int(sys.argv[1])
parameter_2_name = bool(sys.argv[2])
```
이는 CLI 명령어로 전달된 파라미터와 동일하게 취급되므로, `argparse`와 같은 다른 파라미터 파싱 도구와 함께 사용할 수도 있다.
```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("--parameter_1_name", type=int, default=1)
parser.add_argument("--parameter_2_name", type=bool, default=False)

...

args = parser.parse_args()
print(args.parameter_1_name, args.parameter_2_name)
```

또한 `run` 인스턴스의 params 딕셔너리를 통해서도 접근할 수 있다.
```python
import mlflow

parameter_1_name = mlflow.active_run().data.params["parameter_1_name"]
```
### 파라미터 정의
mlflow.projects 모듈을 사용한다면, `run` 함수 실행 시 딕셔너리 형태로 파라미터를 지정할 수 있다.
```python
import mlflow

mlflow.projects.run(
					...,
					parameters = {
						'parameter_1_name': value1,
						'parameter_2_name': value2
					}
)
```

CLI를 사용한다면 `-P` 옵션과 함께 파라미터를 전달할 수 있다.
```bash
mlflow run ... -P parameter_1_name=value1 -P parameter_2_name=value2 ...
```
## Workflows
MLflow Projects는 여러 단계의 워크플로우를 순차적으로 수행하도록 만들수도 있다.
### multi step 수행하기
`MLproject` 파일은 여러개의 entry_points를 만들 수 있다.
```yaml
entry_points:
	step_1:
		...
	step_2:
		...
```

이제 `run` 함수를 실행할 때 어떤 entry_points를 실행해야 하는지 순차적으로 정의할 수 있다.
```python
step_1 = mlflow.projects.run(uri="./", entry_point="step_1")
step_2 = mlflow.projects.run(uri="./", entry_point="step_2")
```
`run` 함수는 `mlflow.projects.SubmittedRun` 객체를 반환하므로, 후속 작업에서 필요시 사용할 수 있다. (공식문서 참조)
### SubmittedRun의 메서드
- `run.cancel()` : 현재 run 취소
- `run.get_status()` : 현재 run의 정보 획득
- `run.run_id()` : 현재 run의 run_id 획득
- `run.wait()` : run이 종료될 때까지 대기

# 소감
## 알게 된 점
예전에 국비학원에서 mlflow를 잠깐 다루었을 때 실험 저장 및 모델 배포용으로 잠깐 배운 적이 있는데, 이렇게 전체 생명주기에 걸쳐 다양한 기능을 지원한다는 것을 배울 수 있었습니다.

또 강의 내용을 정리하고, 궁금한 점을 검색하면서 stages가 deprecate된다는 사실을 알게 되었는데, 역시 이런 라이브러리, 프레임워크를 사용할 때는 버전에 맞는 공식문서를 참조하는 것이 중요하다는 것을 느꼈습니다.

## 궁금한 점
mlflow가 다양한 모델을 지원한다는 것은 처음 알았는데, 실무에서도 `mlflow.sklearn`과 같이 mlflow가 지원하는 모듈을 많이 사용하는지가 궁금합니다.

        