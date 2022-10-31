---
title: '[파이썬] 리스트와 내장함수'
excerpt: '알아두면 유용한 배열 관련 내장함수'

categories:
  - TIL
tags:
  - [python]

permalink: /posts_img/list-built-in-function

toc: true
toc_sticky: true

date: 2022-10-31
last_modified_at: 2022-10-31
---

<div>인프런에서 김태원 강사님의 파이썬 알고리즘 문제풀이 입문 강의를 보고 정리한 글입니다.<br>
<a href='https://www.inflearn.com/course/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8/dashboard' target='_blank'>파이썬 알고리즘 문제풀이 입문 (코딩테스트 대비)</a></div>{: .notice--info}

# 리스트

서로 연관성이 있는 변수들을 연속적으로 묶어 저장하는 자료형이다.
각 value값에는 인덱스가 순서대로 부여되어있어, 인덱스를 통한 접근이 가능하다.

## 리스트 관련 메소드

### `lst.append(object)`

가장 배열의 가장 마지막 index로 object를 추가

### `lst.insert(index, object)`

지정한 index에 object를 추가, 해당 인덱스에 저장되어 있던 기존 값은 모두 뒤로 하나씩 밀린다.

### `lst.pop(\[index\])`

지정한 index에 있는 내부값을 제거, 별도로 지정하지 않으면 가장 마지막 인덱스의 값이 제거된다.

### `lst.remove(object)`

리스트에서 지정한 object 값을 찾아서 제거한다.

### `lst.sort()`

리스트를 정렬한다.<br>
인자로 아무것도 선언하지 않을 경우 오름차순으로 정렬된다.<br>
`reverse = True`를 선언하면 내림차순으로 정렬된다.

### `lst.clear()`

리스트를 빈 리스트로 만든다.

## 리스트의 for 문 활용

리스트는 반복문, 그 중에서도 for문과 함께 활용할 때 아주 유용하다.

### `for x in lst:`

인덱스의 순서대로 배열의 값을 x에 넣어 반복문을 실행한다.

### `for x in enumerate(lst):`

`enumerate()` 파이썬의 내장 함수로, 리스트를 인덱스와 데이터 값으로 구성된 `튜플`로 출력한다.

> 튜플 : 대괄호가 아닌 소괄호로 묶여 저장된 1차원 배열 형식의 자료형이다. 기본적으로 리스트와 동일하지만 내부 값을 수정할 수 없다는 차이가 있다.

```python
a = [23, 12, 36, 53, 19]
for x in enumerate(a):
	print x

# Result
# (0, 23)
# (1, 12)
# (2, 36)
# (3, 53)
# (4, 19)
```

다음과 같이 반복문에서 변수 2개를 선언하여 인덱스와 내부값을 각각 활용할 수 있다.<br>
리스트를 반복문으로 활용할 때 가장 유용하게 쓸 수 있다.

`for index, value in enumerate(lst)`

## 내장 함수(Built-in Functions)

내장 함수는 별도의 python 언어 자체가 제공하는 함수로 별도의 import 없이 사용할 수 있는 함수를 말한다.<br>
그중 배열과 함께 사용하면 유용한 것들을 몇가지 알아보자.

### sum(lst)

sum은 인자를 모두 더한 값을 반환하는 함수이다.<br>
여기에 리스트를 입력하면 리스트의 값을 모두 더한 것을 반환한다.

### max(lst), min(lst)

각각 인자의 값중 최대값/최소값을 반환하는 함수이다.<br>
리스트에서도 동일하게 작동한다.

### all(조건문)

조건문이 모두 참일 때 `True`, 하나라도 거짓이면 `False`를 반환하는 함수이다.
다음과 같이 리스트에서 활용할 수 있다.

`if all(x > 0 for x in lst):`
x에 lst의 모든 값을 순차적으로 대입하여 조건문을 실행한다.<br>
이 경우 모든 값이 조건을 만족해야 `True`가 된다.

### any(조건문)

하나라도 참일 때 `True`, 모두 거짓이면 `False`를 반환하는 함수이다.
all()과 동일하게 사용할 수 있다.

## 기타 함수

### r.shuffle(lst)

리스트의 인덱스를 무작위로 섞는 함수이다.<br>
내장 함수가 아니기 때문에 `random`을 import해주어야 사용할 수 있다.
