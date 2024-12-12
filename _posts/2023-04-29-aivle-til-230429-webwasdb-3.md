

---
title: AIVLE TIL ('23.04.29) WEB/WAS/DB (3)
date: 2023-04-29 14:45:48.306 +0000
categories: [에이블스쿨]
tags: ['aivle']
description: Flask 다루어보기
image: /assets/posts/2023-04-29-aivle-til-230429-webwasdb-3/thumbnail.png

---

# 웹 호스팅

![img](/assets/posts/2023-04-29-aivle-til-230429-webwasdb-3/img0.png)

이번 과정을 통해 진행한 웹 호스팅의 모든 단계를 요약하자면 다음과 같다.

1. git bash CLI 환경을 이용해 ssh 통신으로 ubuntu 서버에 원격 접속
2. scp를 통해 웹페이지 파일, assets(폰트, 이미지 등 정적 컨텐츠), 서버 app 파일 전송
3. Nginx를 통해 서버에 있는 웹페이지 파일을 꺼내와 보여주는 역할을 함
4. Flask를 이용하여 서버 구동, DB와의 통신
5. 최종적으로 서버의 public ip와 지정한 포트를 사용해 서버의 127.0.0.1(로컬)로 부터 웹페이지의 컨텐츠를 받아와 브라우저에 표시

# Flask를 통한 서버 어플리케이션 파일 작성

Flask는 파이썬으로 제작된 웹 프레임워크로, 가능한 최소한의 기능만을 제공하여 가볍고, 몇 줄의 코드만으로 서버를 간단히 구동할 수 있다.

## 프레임워크

### 프레임워크란
프레임워크는 어떤 복잡한 문제(서버 구동과 같은)를 해결하기 위한 틀의 역할을 하는 프로그램이다.
가령 웹 프레임워크는 서버 구동에 필요한 다양한 구성요소와 아키텍처를 제공하여 개발자가 이를 직접 하나하나 코딩하지 않아도 서버를 구동할 수 있도록 해준다.

즉 프레임워크는 소프트웨어를 효율적으로 사용하기 위한 일종의 도구이다.

### 프레임워크 사용하기
프레임 워크를 사용한다는 것은 일종의 빈칸 채우기와 같다. 프레임워크가 제공하는 적절한 기능을 찾아, 프레임워크가 요구하는 문법대로 '빈칸'을 채워서 전달하면 프레임워크는 전달 받은 정보를 바탕으로 기능을 수행한다.

```python
@app.route('/user/<username>')
def user(username):
    return render_template('index.html', user=username)
```
예를 들어 Flask에서 `@app.route('/user/<username>')`을 입력하면 `/user/coolusername`과 같은 url이 서버에 전달되었을 때 데코레이터 아래 정의된 함수를 실행한다.
이 때, `<username>` 처럼 꺽쇠안에 있는 문자열을 매개변수로 전달하는데, 위의 예시에서 `user`함수의 `username`매개 변수에는 `coolusername`이 대입된다.

`return render_template('index.html', user=username)` : `render_template` 함수는 현재 루트폴더(.py 파일이 위치한 폴더)에서 templates라는 이름의 폴더를 찾아 그 안에 있는`index.html` 파일을 찾아 렌더링한다는 뜻이다.

이러한 사용법이 모두 프레임워크의 문법에 해당하며
이를 지키지 않으면 서버는 동작하지 않는다.

각 프레임워크별로 문법은 모두 다르므로 공식 문서를 참고해 필요한 기능을 찾고, 적절한 문법을 사용하는 것을 익혀야한다.
Flask의 사용법을 일일히 나열하는 것은 다소 비효율적이라 생각되어, 서버 코드를 가져와 어떤 기능을 구현했는지 정도를 살펴보자.

## 코드

```python
from flask import *
import pymongo

app = Flask(__name__)
app.secret_key = 'super secret key'
client = pymongo.MongoClient('mongodb://kt:ktpw@43.201.110.250:27017/?authSource=admin')

def is_login():
    try:
        session['id']
        return True
    except:
        return False

@app.route('/')
def index():
    user = dict()
    if is_login():
        user['name'] = session['id']
    return render_template('index.html', user=user)

@app.route('/join')
def join():
    return render_template('join.html')

@app.route('/login')
def login():
    return render_template('login.html')

@app.route('/contents')
def contents():
    user = dict()
    if is_login():
        user['name'] = session['id']
    return render_template('contents.html', user=user)

@app.route('/logout')
def logout():
    session.pop('id')
    return redirect('/')

# API
@app.route('/api/join', methods=['POST'])
def api_join():
    users = client.mongo.users
    account = dict(request.values)
    result = dict()
    # check account
    document = users.find_one({'email': account['email']})
    print(document)
    if document:
        result['msg'] = 'This email already exist.'
    # save account
    else:
        save_result = users.insert_one(account)
        result['inserted_email'] = str(save_result.inserted_id)
        result['msg'] = 'Joined account.'
        
    return jsonify(result), 200

@app.route('/api/login', methods=['POST'])
def api_login():
    users = client.mongo.users
    account = dict(request.values)
    result = dict()
    # check wrong
    document = users.find_one({'email': account['email']})
    
    if document:
        password = document['password']
        if password == account['password']:
            session['id'] = account['email']
            result['msg'] = 'Loggined!'
        else:
            result['msg'] = 'Wrong Password'
    else:
        result['msg'] = 'This email doesn\'t exist'
    return jsonify(result), 200

# debug : 개발자 모드 - 코드 수정하면 바로 서버 재시작해서 사용함
app.run(debug=True)
```

회원가입 기능, 로그인 기능, 로그인된 사용자만 contents에 접근할 수 있는 기능이 구현된 파일이다.

### 회원가입 기능

```python
@app.route('/api/join', methods=['POST'])
def api_join():
    users = client.mongo.users
    account = dict(request.values)
    result = dict()
    # check account
    document = users.find_one({'email': account['email']})
    print(document)
    if document:
        result['msg'] = 'This email already exist.'
    # save account
    else:
        save_result = users.insert_one(account)
        result['inserted_email'] = str(save_result.inserted_id)
        result['msg'] = 'Joined account.'
    return jsonify(result), 200
```

`account = dict(request.values)` : `request`는 Flask에서 POST로 받은 입력 정보이다. (프레임 워크 기능에 의해 제공됨)

`document = users.find_one({'email': account['email']})` : `pymongo`를 통해 mongoDB와 통신하여 요청 받은 이메일이 이미 DB에 존재하는지 확인한다.
- 이미 있을 경우 : 이메일이 이미 존재한다는 메세지를 결과에 담아 전송한다. (js에서 처리)
- 없을 경우 : mongodb에 생성된 계정 정보를 추가하고 성공했다는 메세지를 결과에 담아 전송한다.

### 로그인 기능

```python
@app.route('/api/login', methods=['POST'])
def api_login():
    users = client.mongo.users
    account = dict(request.values)
    result = dict()
    # check wrong
    document = users.find_one({'email': account['email']})
    
    if document:
        password = document['password']
        if password == account['password']:
            session['id'] = account['email']
            result['msg'] = 'Loggined!'
        else:
            result['msg'] = 'Wrong Password'
    else:
        result['msg'] = 'This email doesn\'t exist'
    return jsonify(result), 200
```

회원가입 때와 유사한 프로세스로 진행된다.
 총 3가지 경우의 수로 나뉜다.
1. 이메일이 DB에 없을 경우, 
2. 이메일은 DB에 있으나 비밀번호가 DB 정보와 다른 경우 
3. 이메일도 있고 비밀번호도 일치하는 경우

1, 2번은 각기 다른 메세지를 결과 딕셔너리에 담아 json 형태로 응답한다.
3번은 `session['id'] = account['email']`을 통해 현재 세션 정보에 유저 정보를 추가한다.
세션에 유저 정보가 남아있는 한 유저는 계속 로그인 한 것으로 취급된다.

### 로그아웃 기능
```python
@app.route('/logout')
def logout():
    session.pop('id')
    return redirect('/')
```

`session.pop('id')` : 세션에 있는 유저 정보를 제거하고 홈 화면으로 이동시킨다.

### 로그인 사용자에게만 페이지 표시하기
```python
def is_login():
    try:
        session['id']
        return True
    except:
        return False

@app.route('/contents')
def contents():
    user = dict()
    if is_login():
        user['name'] = session['id']
    return render_template('contents.html', user=user)
```

`is_login()`이라는 전역으로 정의된 함수를 사용한다.
`is_login` 함수는 세션에 정보가 있는지 체크하여 있다면 True 반환, 없다면 False를 반환한다.

이를 통해 로그인이 되어있는지 아닌지를 판단해 되어있는 경우에만 `contents` 웹페이지를 보여준다.

# 소감

정말 정신없이 진행되었던 과정이었다.

이전에 node.js에서 express 프레임워크를 사용해 서버를 구동했던 경험이 있기 때문에 그나마 어떤 과정이 진행되는지 따라갈 수 있었던 것 같다.

비록 언어도 다르고 사용 문법도 많이 다르지만, 객체를 이용한다는 점이나 return을 통해 페이지를 렌더링하고 유저와 정보를 주고받는 다는 점을 통해 프레임워크가 근본적으로는 같은 기능을 한다는 것을 깨달았다.

사실 강사님이 준비는 하셨는데 시간상 못 구현한 기능이 많은데
시간이 된다면 이를 꼭 직접 해보고 싶다.
시간... 되겠지?

        