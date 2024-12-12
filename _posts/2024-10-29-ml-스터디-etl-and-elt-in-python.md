

        ---
        title: [ML 스터디] ETL and ELT in Python
        date: 2024-10-29 09:45:53.970 +0000
        categories: [developer-AI-powered]
        tags: []
        description: [6주차] 가짜연구소 9기 "AI를 잘 활용하는 개발자로 성장하기" 프로젝트에 참여하여 Datacamp의 "Machine Learning Engineer" 강의를 수강하고 해당 내용을 정리한 게시글입니다.
        image: /assets/img/posts/2024-10-29-ml-스터디-etl-and-elt-in-python/thumbnail.png
        
        ---

        > 가짜연구소 9기 "AI를 잘 활용하는 개발자로 성장하기" 프로젝트에 참여하여 Datacamp의 "Machine Learning Engineer" 강의를 수강하고 해당 내용을 정리한 게시글입니다.
> [Datacamp Machine Learning 코스 페이지](https://www.datacamp.com/category/machine-learning)

# Chapter 1. Introduction to Data Pipelines
## ETL, ELT 파이프라인 소개
### Data pipelines
- 데이터를 source에서 destination으로 옮기는 파이프라인
  - 여기서 옮긴다는 것은 destination에서 사용 가능한 형태로 변환하는 것을 포함
- Data pipeline의 대표적인 종류로 `ETL`, `ELT`가 있음
### ETL
- Extract, transform, load
- 추출 -> 변환(처리 및 정제) -> destination으로 이동
- 가장 전통적인 파이프라인으로 추출된 데이터를 변환하기 위해 임시 데이터 스테이징이 필요
### ELT
- Extract, load, transform
- 추출 -> 데이터 레이크에 저장 -> 필요한 형태로 변환 및 사용
- 데이터 레이크 개념이 등장하면서, 원본 데이터를 최대한 빨리, 많이 로드하여 유연성 있게 사용할 수 있기 때문에 최근 많은 관심을 받고 있음

> 추가 조사 내용
> - ETL : 온라인 분석 처리의 경우 데이터가 저장 되기 위해 관계형으로 변환되어야 하기 때문에 transfrom이 먼저 필요함. 데이터가 저장 되기 전에 먼저 마스킹과 같은 사전 처리를 진행하므로 개인정보보호 등 규제, 보안에 유리하다. 하지만 실시간 처리에 대한 유연성이 떨어진다는 단점이 있다.
> - ELT : 데이터 레이크(모든 종류의 데이터를 수용하는 특별한 데이터 저장소)에 데이터를 즉시 로드한 뒤, 실제 사용할 때 데이터 레이크에서 꺼내온 데이터를 변환하는 형식. 데이터가 정형화될 필요가 없기 때문에 유연하게 데이터를 수집할 수 있으며, 속도 측면에서 유리하다. 하지만 저장되는 데이터의 품질을 보장하기 어려울 수 있다.

## ETL, ELT 파이프라인 구축
### pandas
`pandas` 라이브러리를 통해 csv와 같은 tabular raw data를 가져와 후속 작업을 수행할 수 있다.
- `read_csv()` : csv를 DataFrame으로 변환
- `df.head()` : 데이터의 일부 행을 빠르게 확인
- `df.loc()` : DataFrame을 다양한 방법으로 필터링
- `df.to_csv(path)` : path에 DaraFrame을 csv 형식으로 저장
  - `to_json()`, `to_excel()`, `to_sql()` 등 다른 형식도 지원
### SQL Queries
SQL 쿼리를 지원하는 라이브러리를 사용하면 pythob에서 SQL 쿼리를 실행할 수 있다.
```python
# 예제
from sqlalchemy import create_engine

# engine 연결
engine = create_engine('sqlite:///mydatabase.db')

# SQL 쿼리를 DataFrame으로 불러오기
df = pd.read_sql("SELECT * FROM MYTABLE", con=engine)
```

이러한 extract, load, transformation을 지원하는 다양한 라이브러리를 이용하여, ETL 및 ELT 각 순서에 맞도록 함수를 짜 순차적으로 실행하는 것이 대부분의 Data pipeline 프로그램이다.
# Chapter 2. Building ETL Pipelines
## Extracting data from structure sources
거의 모든 파이프라인의 시작은 source로부터 데이터를 추출하는 것이다.
이번 코스에서는 CSV, JSON, Parquet, SQL 데이터베이스 등 tabular system 데이터에서 데이터를 추출하는 과정을 다룬다.
### parquet
- 컬럼 중심의 오픈소스 파일 포맷
- `pandas`에서 csv와 유사하게 사용할 수 있지만, 읽기 및 쓰기 속도가 훨씬 더 빠르다는 장점
  - `pd.read_parquet(path, engine="fastparquet")`
### SQL 데이터베이스
- `pandas`의 `read_sql()` 함수를 통해 SQL 데이터베이스에 쿼리 기반 데이터 추출이 가능
- `sqlalchemy`와 같은 별도 라이브러리를 이용해 먼저 SQL 데이터베이스와 연결된 인스턴스(db engine)를 만들어서 `read_sql()` 함수에 함께 전달
  - `pd.read_sql("QUERY", db_engine)`
## Transforming data with pandas
추출한 데이터를 분석 및 학습에 사용할 수 있도록 적절히 변환하는 과정
`pandas`는 tabular 데이터를 변환할 수 있는 매우 강력한 도구이다.
### .loc[]
- 데이터 프레임에서 row 또는 column 값에 기반한 필터를 이용해 원하는 데이터만 조회할 수 있는 메서드
- `loc[]`로 조회한 데이터를 수정하면 데이터 프레임에서 조회된 데이터만 변경됨
- ex) `df.loc[df["column1"] > 0, ["column1", "column2", "column3"]]`
  - `column1`이 0보다 큰 행의 `column1`, `column2`, `column3` 열만 출력
- `iloc[]`은 인덱스 기반의 조회 메서드
### 데이터 타입 변경
- `to_datetime()` : time string을 date 타입으로 변경
  - date 타입은 각종 날짜 연산이 가능한 데이터 타입
  - `format` 파라미터로 time string이 어떤 형식으로 되어있는지 설명
  - `unit` 파라미터 정수의 단위가 무엇인지 설명
### 데이터 변환 시 주의할 점
데이터를 변경하는 것은 다음의 위험을 동반한다
- 정보 손실
- 잘못된 정보 생성
`head`, `nsmallest`, `nlargest` 등의 메서드로 초기값 및 극단값을 확인하여 잘못된 변환이 있는지 대략적인 검사를 하는 것을 추천한다고 한다.
## Persisting data with pandas
- persistenting data : 변환된(정제된) 데이터를 사용자가 원할 때 접근할 수 있도록 데이터의 스냅샷을 영구적인 형태(파일 등)로 저장하는 과정
- ETL의 마지막 단계 뿐 아니라, 데이터 파이프라인의 중간중간에도 여러 단계에 걸쳐 이루어짐
  - 문제가 생겼을 때 복원을 위해 또는 재취득이 어려운 source data 보존을 위해 등등
### 데이터 저장
`pandas`는 `to_csv()` 메서드를 통해 데이터프레임을 csv로 저장할 수 있다.
- `header` : 열의 이름을 csv의 첫번째 행으로 저장할지 여부 (bool, True)
- `index` : 인덱스 열을 저장할지 여부 (bool, True)
- `sep` : 컬럼을 구분할 구분자 (string, ',')
그 외에 `to_parquet()`, `to_json()`, `to_sql()` 등 다양한 형식으로 저장 가능
저장을 한 후에는 `os.path.exists` 등을 이용하여 파일 정상적으로 저장되었는지 확인하는 것이 좋다.
## Monitoring a data pipeline
데이터 파이프라인은 자동화되어있는 만큼 정상 작동을 모니터링하는 것이 중요
### Logging data pipeline
- 실행된 파이프라인의 성능을 측정하여 문서화
- 오류가 발생했을 때 발생한 위치를 제공
### logging
- python의 내장모듈로 제공되는 로깅 라이브러리
- `debug`, `info`, `warning`, `error`, `critical` 순으로 심각도에 따른 로깅 가능
```python
# 사용 예시
import logging

# 기본 설정
logging.baseConfig(format='%(levelname)s: %(message)s', level=logging.DEBUG)

logging.debug(f"debug message {path}")
logging.info("program complete")
```
- `baseConfig` 설정시 `filename` 인자를 설정하면 `level`과 같거나 더 심각한 로깅 메세지를 설정된 경로에 txt로 저장

# Chapter 3. Advanced ETL Techniques
## Extracting non-tabular data
### non-tabular 데이터의 종류
현실의 데이터는 거의 대부분이 non-tabular인 경우가 많다.
- 텍스트
- 음성 데이터
- 이미지 및 영상
- 센서 등 IoT 데이터
- JSON, XML
  - non-tabular이지만 규격화된 형식으로, API로 제공되는 데이터가 일반적으로 이 형식
이번 강의에서는 비교적 규격화 되어있으면서, 일반적으로 사용되는 JSON 형식에 대해 다루고 있다.
### pandas에서 JSON 파일 다루기
- `read_json()` 메서드를 통해 json을 데이터프레임으로 쉽게 변환 가능
- `orient` : 데이터프레임으로 파싱하기 위해, JSON이 어떤 형식으로 저장되어있는지 설명
  - `'split'` : dict like `{index -> [index], columns -> [columns], data -> [values]}` 
  - `'records'` : list like `[{column -> value}, ... , {column -> value}]`
  - `'index'` : dict like `{index -> {column -> value}}`
  - `'columns'` : dict like `{column -> {index -> value}}`
  - `'values'` : just the values array
  - `'table'` : dict like `{'schema': {schema}, 'data': {data}}`
### nested JSON 다루기
JSON에 저장된 데이터는 종종 여러 depth가 중첩된 nested 형태일 수 있는데, 이 경우 데이터프레임으로 곧바로 파싱하는 것은 불가능하다.
대신 `json` 내장 모듈을 이용하여 `dict` 형식으로 먼저 로드한 뒤 적절히 처리하여 pd가 처리할 수 있는 형태로 변환하는 것이 좋다.
## Transforming non-tabular data
앞서 설명한 nested json과 같이 곧바로 dataframe 형태로 변환할 수 없는 데이터들은 적절한 변환 과정을 거쳐야 한다.
### 딕셔너리 변환
json을 딕셔너리로 변환했으므로, 딕셔너리의 `keys()`, `values()`, `items()` 메서드를 적절히 활용해 nested된 데이터를 iterable로 접근할 수 있다.

또한 `get(key, value)` 메서드를 통해 key에 해당하는 데이터가 존재하는지 여부를 검사하고, 존재하지 않을 경우 `value`의 값을 할당할 수 있다.

최종적으로 nested dict를 flatten하여 1차원 list of lists 형태로 데이터를 변환하면, DataFrame으로 변환할 수 있다.
이 경우 column이름, 각 행의 인덱스는 리스트의 인덱스(0, 1, 2...)인 상태이므로 `df.columns`를 직접 지정하고 `df.set_index()` 메서드를 통해 인덱스 역할을 할 열을 지정할 수 있다.
## Advanced data trasnformation with pandas
일반적으로 raw데이터에는 결측치, 이상치 등의 노이즈가 존재한다.
이들을 적절히 처리하는 것도 데이터 변환의 필수적인 과정이다.
### 결측치 채우기
- `df.fillna(value)` : 데이터프레임의 결측치를 특정 값으로 채우는 메서드
  - `value`로 딕셔너리를 넣어 column 별로 결측치에 들어갈 값을 다르게 지정할 수 있다.
  - `value`로 특정 column을 지정하면, 결측치가 같은 row의 해당 column 값으로 채워진다.
    - ex) `df.fillna(df["column1"], inplace=True)`
### groupby
SQL의 GROUP BY 문과 유사하게, 특정 column의 값이 동일한 데이터들을 묶어 그룹별 통계량을 계산 할 수 있다.
- ex) `df.groupby(by=["column"], axis=0).mean()`
- 적용 가능한 집계함수는 `mean()`, `min()`, `max()`, `sum()` 등
### 커스텀 변환
`apply()` 메서드를 이용하면, 데이터를 변환하는 커스텀 함수를 일괄 적용하여 데이터 프레임을 변환할 수 있다.
```pythin
def custom_change(row):
	...logic

df_transformed = df.apply(custom_change, axis=1)
```
## Loading data to a SQL database with pandas
### pandas에서 SQL 데이터 베이스로 데이터 로드하기
pandas는 `to_sql()` 메서드를 통해 SQL로의 데이터 로드를 지원한다.
다음의 파라미터를 적절히 전달하여야 한다
- `name`
- `con`
- `if_exists`
- `index`
- `index_label`
### 예제
```python
# 1. DB 연결
db_engine = sqlalchemy.create_engine(uri)

stock_data.to_sql(
	name="stock_data",  # 데이터를 저장할 테이블 이름
	con=db_engine,  # 저정할 DB uri (연결되어있어야 함)
	if_exists="append",  # 테이블이 이미 존재할 때 데이터 추가 방법
	index=True,  # 인덱스를 함께 저장할 것인지 여부
	label_index="timestamps"  # index==True일 때 인덱스로 사용할 컬럼
)
```
### SQL 로드 후 검증
SQL로 데이터가 정상적으로 로드 되었는지 검증 과정이 필요하다.
다음과 같은 검증을 할 수 있다.
- QUERY 문을 이용하여 데이터가 잘 로드되었는지 확인
- count를 확인해 데이터가 로드되었을 때의 예상 count와 일치하는지 확인
- 각 row를 직접 불러와 equality 검증
# Chapter 4. Deploying and Maintaining a Data Pipeline
## Manually testing a data pipeline
배포 후에는 파이프라인을 테스트하기 어려워지므로, 배포 전 테스트를 통해 데이터가 정확히 입력되는지, 또한 적절하게 변환되는지 검증을 마치는 것이 중요하다.
다음의 데이터 파이프라인 테스트에 대해 배운다
- End-to-end testing
- validating data at checkpoints
- unit testing
### Testing 환경
일반적으로 테스트 환경은 배포 환경과 분리된 환경에서 진행한다.
이렇게 하는 이유는 테스트 도중 변경 사항으로 인해 데이터 파이프라인에 오류가 발생할 수 있으므로, 파이프라인을 사용하는 사용자에게 영향이 미치지 않게 하기 위함이다.
### End-to-end testing
- 테스트 source에 대해 extract-transformation-load 까지의 과정을 end-to-end로 반복 진행하며 오류가 발생하는지 확인
- 데이터를 저장하는 중간 checkpoint를 만들어 중간 단계의 데이터가 의도된 형태로 존재하는지 검증
### Validating data at checkpoints
- 데이터 파이프라인의 각 컴포넌트 사이사이에 데이터를 저장하는 checkpoint를 만들어 데이터가 정상적으로 존재하는지, 손실된 데이터가 없는지 확인
- 또는 테스트 데이터에 대한 정상 결과 데이터를 미리 만들어두어 checkpoint 데이터와의 equality를 확인
  - `df.equals` 메서드를 통해 비교 가능
## Unit-testing a data pipeline
### pytest
- `pytest` : unit test를 지원하는 라이브러리
- `test*`로 시작하는 함수를 자동으로 감지 및 파싱하여 해당 함수를 갖고 단위 테스를 수행
- 실행 결과로 테스트 횟수 및 실행 결과 반환
  - 오류가 발생하지 않는다면 통과, 오류가 발생했다면 실패
  - 일반적으로 `assert` 문과 함께 사용
### pytest.fixture()
`pytest.fixture()`를 데코레이터로 사용하는 함수는 pytest에서 테스트 함수에 인자로 넣을 수 있게 된다. 
이렇게 전달된 fixture는 테스트 함수가 실행될 때 내부적으로 먼저 실행되어 반환한 값으로 치환된다. 
테스트를 위한 데이터를 복잡한 과정에 걸쳐 얻어야하는 경우 fixture를 사용하면 효율적인 테스트를 수행할 수 있다.
예를들어 `isinstance`와 같은 검사 함수에 `fixture`를 넣어 함수 단위의 테스팅이 가능하다.
```python
# 예시
@pytest.fixture
def clean_data():
	...logic
	return cleand_data

def test_cleaned_data(clean_data):
	assert isinstance(clean_data, pd.DataFrame)
	assert clean_data["count"].min() >= 0
```
## Running a data pipeline in production
### 데이터 파이프라인 오케스트레이션
데이터 파이프라인이 배포된 후에는, 특정 스케줄/이벤트에 맞춰 한정된 자원 내에서 데이터 파이프라인이 자동으로 실행되도록 해야한다. 이를 위해서는 실패 감지 및 자동 재실행 역시 필요하다.

이를 사용하는 것이 오케스트레이션 tool이다.
가장 대중적으로 사용되는 도구는 `Apache airflow`이다.
airflow는 오픈소스이기 때문에 매우 폭넓은 기능과 플러그인을 제공한다.
이외에도 다양한 도구가 있고, 때로는 이러한 로직을 custom-built solutions로 직접 구현하기도 한다.

올바른 도구를 선택하는 의사결정이 매우 중요하다

# 소감
## 새롭게 알게된 것
`dict` 객체의 `get()` 메서드를 자주 사용했었는데, 두번째 인자로 value를 전달하면 `get()`의 결과가 `None`일 경우 `value`를 반환한다는 사실을 처음 알게 되었습니다. 종종 유용하게 쓰일 것 같습니다.

## 궁금한 점
`logging` 내장 모듈을 사용하여 로깅을 하는 내용이 나오는데, 실무에서도 `logging`을 사용하여 이러한 로그를 처리하는지, 아니면 조금 더 발전된 라이브러리를 사용하는지 궁금합니다.

## 느낀점
사실 이번 강의는 그동안 배워왔던 판다스로 데이터 다루기에 가까워서 비교적 어렵지 않게 들을 수 있었습니다.

wrap up에서 데이터 파이프라인 오케스트레이션 관련하여 airflow와 같은 tool에 대한 강의가 있다는 것을 알려주는데, 필요한 경우 이런 강의들을 들어도 좋을 것 같습니다.
아쉬웠던 것은 데이터 파이프라인 오케스트레이션이 중요한 과정일 것 같은데, tool 선택에 대한 의사결정에 무엇을 고려해야 하는지 간략하게라도 알려줬으면 합니다.

        