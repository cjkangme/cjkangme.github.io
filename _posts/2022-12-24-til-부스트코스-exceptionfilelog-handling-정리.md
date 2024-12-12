

        ---
        title: [TIL] 부스트코스 Exception/File/Log Handling 정리
        date: 2022-12-24 12:05:14.141 +0000
        categories: [부스트코스-PreCourse]
        tags: ['부스트코스', '프리코스']
        description: 프로그램을 사용할 때 일어나는 오류들크게 예상 가능한 예외와 예상 불가능한 예외로 나뉜다.예상 가능한 예외 : 사용자의 잘못된 입력 등 발생 여부를 사전에 인지할 수 있는 예외		\- if문 등을 이용하여 개발자가 반드시 명시적으로 정의해야 한다.예상 불가능한 예외 :
        
        
        ---

        # Exception(예외)
- 프로그램을 사용할 때 일어나는 오류들
- 크게 예상 가능한 예외와 예상 불가능한 예외로 나뉜다.

- 예상 가능한 예외 : 사용자의 잘못된 입력 등 발생 여부를 사전에 인지할 수 있는 예외
	- if문 등을 이용하여 개발자가 반드시 명시적으로 정의해야 한다.
- 예상 불가능한 예외 : 개발자의 실수, 잘못된 데이터 등으로 인터프리터 과정에서 발생하는 예외
	- 인터프리터가 자동으로 에러를 호출하고 프로그램을 중지/종료한다.
    - 예외가 발생할 경우를 대비한 후속 조치 등 대처가 필요하다.
    - 파이썬에서는 Exception Handling을 이용할 것을 권장한다.
## Exception Handling
### try ~ except 문법
```python
try:
	# 예외가 발생할 수 있는 코드 (필수)
except <Exception Type> as e:
	# 예외 발생시 실행되는 코드 (필수)
    # as e를 통해 에러의 정보를 인자로 받아 사용할 수 있다.
else:
	# 예외가 발생하지 않고 try가 모두 끝났을 때 동작하는 코드 (선택)
finally:
	# 예외 발생여부와 상관없이 실행되는 코드 (선택)
```
- try ~ except 문법의 장점은 반복문과 같은 상황에서 오류가 발생하여도 오류처리를 끝낸 뒤 남은 반복문을 마저 돌 수 있다는 것이다.
- except에 에러 종류를 지정하지 않으면, 지정된 오류를 제외한 모든 오류를 catch한다.
	- 하지만 어디서 Exception이 발생했는지 다른 사용자가 알기 어렵기 때문에 권장하지 않는다.
### 예외의 종류
- 파이썬이 기본으로 제공하는 수많은 Built-in Exception이 있다.
> 공식문서 : https://docs.python.org/3/library/exceptions.html
- `EOFError` : `input()` 으로 파일을 읽어올 때, 파일의 끝(EOF)에 도달하여 아무것도 읽어올 수 없을 때 발생(raise)하는 에러
- `IndexError` : sequence 데이터의 Index 범위를 넘어 호출했을 때 발생하는 에러
- `KeyError` : 딕셔너리에 존재하지 않는 키를 호출했을 때 발생하는 에러
- `NameError` : 정의되지 않은 변수를 호출할 때 발생하는 에러
- `RecursionError` : 재귀 깊이가 설정된 값을 넘었을 때 발생하는 에러 (`sys.setrecursionlimit(limit)` 를 통해 재귀 깊이 설정 가능)
- `ZeroDivisionError` : 0으로 나누었을 때 발생하는 에러
- `ValueError` : `int("c")`와 같이 변환할 수 없는 문자/숫자를 변환할 때 발생하는 에러
등 절말 많은 에러가 존재한다.

> 예외처리로 if 문법을 쓸지, except 문법을 쓸지는 동료 간 의사소통하여 결정하는 것이 좋다.

### raise 구문
- 필요에 따라 강제로 Exception을 발생시키는 문법이다.
	- 우리가 만든 모듈을 다른 사람이 사용할 때, 잘못된 사용이 발생할 수 있으므로 코드가 계속 실행되어 악영향이 커지는 것을 방지한다.

```python
while True:
	value = input("비밀번호를 입력해주세요")
    for digit in value:
    	if digit not in "0123456789":
        	raise ValueError("숫자 외의 값을 입력하셨습니다.")
        else:
        	pass # 실행될 코드
```

### assert 구문
- `assert (조건문)`으로 사용하며, 조건문의 결과가 False일 경우 AssertError를 발생시킨다.

# File Handling
- 파일 시스템은 OS에서 파일을 저장하는 **트리구조**의 저장 체계이다.
- 파일은 디렉토리(폴더)와 파일로 구성된다.
## 파일의 종류
- 컴퓨터에서 파일은 기본적으로 text 파일과 binary 파일로 나뉜다.
- 컴퓨터는 text 파일을 처리하기 위해 binary 파일로 변환시킨다.
- **text 파일**
	- 인간도 이해할 수 있는 형태인 문자열 형식으로 저장된 파일
    - 메모장으로 내용 확인이 가능
    - 종류 : txt 파일, HTML 파일, 언어 코드 파일 등
- **binary 파일**
	- 컴퓨터만 이해할 수 있는 형태인 이진형식으로 저장된 파일
    - 메모장으로 내용 확인이 불가 (깨져보인다)
    - 호환되는 어플리케이션을 이용하여 실행할 수 있다.
    - 종류 : 워드 파일, 엑셀 파일 등
    
## Python File I/O

- 파이썬에서는 `open` 키워드를 통해 파일을 처리할 수 있다.

```python
f = open("file.txt", "r") # (파일명, 접근 모드)
contents = f.read() #파일을 불러와 contents 변수에 저장
print(contents)
f.close
```

- 접근 모드
	- r : 읽기 모드 (파일을 읽기만 할 수 있다)
    - w : 쓰기 모드 (파일 생성 및 기존 내용을 덮어쓸 수 있다)
    - a : 추가 모드 (기존 파일의 마지막에 새로운 내용을 추가할 수 있다)
- 경로
	- 기본적으로 같은 디렉토리 내에 있어야하며, 현재 디렉토리를 기준으로 상대참조로 불러온다.

### file read 함수
- `f.readlines` : 파일을 \n을 기준으로 잘라서 리스트 형태로 저장한다. 파일의 모든 line이 한꺼번에 메모리에 올라간다.
- `f.readline` : 파일을 \n을 기준으로 한 줄씩 읽어온다. 전체 파일 용량이 너무 커서 메모리에 담을 수 없을 때 주로 사용한다.

### file write
- 파일을 추가 및 수정할 때는 encoding을 해주어야 한다.
	- os 별로 문자를 encoding하는 포맷이 다를 수 있기 때문에 가장 보편적인 utf8 인코딩을 하는 것이 좋다.
    
```python
f = open("sample.txt", "w", encoding="utf8")
for i in range(1,11):
	data = "%d번째\n" % i
    f.write(data)
f.close()
```
## 파이썬의 directory 다루기
### os 모듈
- `mkdir()`와 같은 함수를 사용해서 디렉토리 생성이 가능
- 동일 이름의 디렉토리가 있으면 FileExistError가 발생
	- `isdir()` 또는 `exists()` 함수를 사용해서 확인할 수 있다.
- 이외에도 다양한 함수가 내장되어있음

### pathlib
- **경로(path)를 객체로 다룰 수 있도록해주는 모듈**

```python
import pathlib

cwd = pathlib.Path.cwd() # cwd = current working directory
print(cwd)
# D:\coding\boostcourse
print(list(cwd.parents))

print(list(cwd.glob("*")))
```
- `cwd()` : 현재 스크립트를 실행한 디렉토리 경로
- `parent()` : cwd의 부모 경로
- `parents()` : cwd의 모든 부모 경로
- `glob()` : 현재 디렉토리에 있는 다른 디렉토리와 파일 목록을 필터링하여 검색할 수 있다.
	- \* : 모든 디렉토리와 파일
    - \*.txt : 파일 확장자가 txt인 모든 파일
    - \*.p\* : 파일 확장자가 p로 시작하는 모든 파일

## Pickle
- 파이썬의 객체는 프로그램 실행시 메모리에 저장된다. 그러나 파이썬 인터프리터가 종료되면 사라진다.
- pickle을 이용하면, 파이썬의 객체를 파일로 **영속화(persistence)**하여 원할 때 불러와 사용할 수 있다.
	- 영속화 : 하나의 객체를 파일로 저장해서 쓰는 것
- 객체 뿐만 아니라 클래스도 영속화하여 쓸 수 있다.
- 저장해야하는 정보, 계산결과(모델) 등 활용이 많다.

```python
import pickle

f = open("list.pickle", "wb") # 1
test = [테스트]
pickle.dump(test, f) # 2
f.close

del test # 테스트 리스트를 지운다.

f = open("list.pickle", "rb")
test_pickle = pickle.load(f) # 3
print(test_pickle) # [테스트]
f.close()
```
1. `f = open("list.pickle", "wb")` : wb는 write+binary를 의미한다. 바이너리 파일 쓰기 모드로 파일을 열겠다는 뜻.
2. `pickle.dump(test, f)` : test 리스트를 list.pickle 파일에 저장한다.
3. `pickle.load(f)` : pickle 파일을 불러온다.

# Logging Handling
- Logging : 프로그램이 실행되는 동안 일어나는 정보를 기록으로 남기는 것
	- 정보는 사용자 접근, 예외 발생, 특정 함수의 사용 등이 있다.
    - 기록된 로그를 분석하여 의미있는 결과(에러 수정 등)를 도출하기 위해 사용한다.
- 콘솔화면에 출력하거나, DB에 남기기, 파일 생성 등의 방법을 이용할 수 있다.
	- 하지만 콘솔화면에 남기는 기록은 콘솔창을 끄면 사라지기 때문에 분석 시 사용할 수 없다.
    - 분석을 위해서는 별도의 파일, DB로 기록을 남겨야 한다.
- 사용 목적에 따라 레벨별(개발, 운영, 사용자) 또는 모듈별로 각각의 log를 남겨야한다.

## logging 모듈

- python의 기본 로그 관리 모듈이다.
- 5가지 레벨의 로그를 남길 수 있다.
	- debug : 개발 시 처리 기록을 남겨야하는 로그 정보
    	- A함수 호출, 변수 변경 등
    - info : 처리가 진행되는 동안의 정보
    	- 서버 시작 및 종료, 사용자 접속
    - warning : 사용자의 잘못된 입력과 처리는 가능하나 원래 의도와 다른 정보가 들어왔을 때 정보
    	- 예를들어 버전 업데이트로 더이상 사용되지 않는 deprecated 함수를 사용할 때 등
    - error : 잘못된 처리로 인해 에러가 발생했으나, 예외 처리 등으로 프로그램을 동작할 수 있을 때 정보
    	- 연결 불가, 처리해야할 파일이 없음
    - critical : 프로그램이 종료될 정도로 심각한 버그가 발생 했을 때 (사용자 강제종료 등)
    	- 사용자에 의한 강제 종료, 잘못된 접근으로 인한 파일 삭제 등
- 일반적으로 다음과 같이 레벨링한다. 
	- 개발 : debug
    - 운영 : info, warning
    - 사용자 : error, critical

### logging level
- logging 모듈은 default로 warning 수준의 로그부터 출력한다.
- 파이썬 3.8버전부터 `logging.basicConfig(level=logging.[레벨])`을 통해 출력할 레벨을 설정할 수 있다.
	- 도중에 변경하려면 setLevel() 함수를 이용한다.

```python
import logging

if __name__=="__main__":
    logger = logging.getLogger("main") # 1
    logging.basicConfig(level=logging.INFO) # info부터 로깅
    
    steam_handler = logging.FileHandler( # 2
        "my_log", mode="w", encoding="utf8"
    )
    logger.addHandler(steam_handler)

    logger.debug("DEBUG")
    logger.info("INFO")
    logger.warning("WARNING!")
    logger.error("ERROR!!!")
    logger.critical("CRITICAL!!!!!")
    
    #INFO:main:INFO
	#WARNING:main:WARNING!
	#ERROR:main:ERROR!!!
	#CRITICAL:main:CRITICAL!!!!!
```
1. getLogger() : logger를 선언한다. 인자를 통해 로그 이름을 지정할 수 있다.
2. FileHandler() : Logger의 출력 방법을 선언한다. FileHandler의 경우 로그를 선언한 파일에 저장한다.

## Logging의 실제 사용
- 실제 프로그램에 logging을 사용할 때는 여러 설정이 필요하다.
	- 데이터 파일 위치, 로그 기록 저장 장소, operation type 등..
- 이러한 설정을 해주는 방법은 크게 두 가지가 있다.

### configparser
- 프로그램의 실행 설정을 파일에 따로 저장하여 불러와 사용한다. (일반적인 방법)
- Section, Key, Value 값의 형태로 설정하며, Dict Type으로 호출 후 사용할 수 있다.
	- 보통 한 개의 section은 여러개의 Key, Value로 구성되므로 for문을 이용하여 key-value값을 가져온다.
- 주로 csv 파일 형식으로 작성한다. (현재는 자세히 이해할 필요는 없지만 어떤 로깅을 위해 사용하는지 알 수 있을 정도여야 한다)
    
### argparser
- console 창에서 프로그램 실행 시 setting 정보를 저장한다.
- console 창에서 argument와 함께 파일을 실행할 때 적용된다.
	- ls **--help**
    - conda uninstall jupyter **--y**
    - python **--version**
- argparser의 선언은 부스트코스 장표의 57페이지 참조

### Logging formatter
- Log 결과값의 format을 지정할 수 있다.

```python
import logging

formatter = logging.Formatter('%(asctime)s %(levelname)s %(process)d')
# 2022-12-24 21:03:05, 458 ERROR 4439 -> 이런 식이다.
```



        