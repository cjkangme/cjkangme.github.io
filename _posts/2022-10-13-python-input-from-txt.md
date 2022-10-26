---
title: '[Python] .txt 파일에서 input 받기'
excerpt: '알고리즘 문제에서 test case를 받기위한 방법 정리'

categories:
  - TIL
tags:
  - [python, algorithm]

permalink: /swea/python-input-from-txt/

toc: true
toc_sticky: true

date: 2022-10-13
last_modified_at: 2022-10-13
---

## txt 파일 호출

SWEA에서 제공하는 input.txt 파일을 호출하는 함수는 다음과 같다

```python
import sys
sys.stdin = open("input.txt", "r")
```

같은 폴더에 존재하는 input.txt를 읽기 전용으로 열겠다는 뜻이다.<br>
그러나 VS Code 등에서는 상대경로 등이 다를 수 있어 `FileNotFoundError`가 발생할 수 있다.<br>
이 경우 절대경로를 이용하는 것이 좋다.<br>
절대 경로를 사용할 때는, 백슬래시(`\`)가 이스케이프 문자로 사용되는 경우를 방지하기 위해 두번씩 사용해주는 것이 좋다.

```python
import sys
sys.stdin = open("D:\\coding\\algorithm\\swea\\input.txt", "r")
```

## txt 파일 읽기

기본적으로 input()의 경우 파일 내의 텍스트를 한 줄씩 읽게 된다.<br>
여러줄을 읽기 위해서는 for문 등의 반복문으로 한 줄씩 순회하며 읽을 수 있다.

### 예시 txt 파일

```txt
5
77 34 59 10 5
49 39 2 19 0
192 284 75 6 55
182 85 92 95 5
8 71 85 64 17
```

알고리즘 문제에서 흔히 입력받는 형태의 txt 파일이다.<br>
첫줄의 5는 총 test case의 수를 나타내며,<br>
각각의 줄이 하나의 case이다.

### 정수 1개 입력 받기

```python
N = int(input()) # 5
```

기본적으로 `input()`은 문자열의 형태로 데이터를 반환한다.<br>
때문에 정수를 입력받기 위해서는 `int()`로 감싸주어야 한다.

### 한 줄의 정수 입력 받기

```python
li = map(int, input().split())
```

한 줄의 정수를 전부 입력 받기 위해서는 `map()` 함수를 이용한다.<br>
예제의 경우는 input()을 통해 입력 받은 것을 공백을 구분자로 구분하여 정수형으로 li 변수에 저장한다는 뜻이다.<br>

단, 이상태로는 li는 `map 객체`이기 때문에, 리스트나 튜플 등 원하는 형태로 변환하는 과정이 필요하다.

```python
li = [*map(int, input().split())]
```

```python
li = list(map(int, input().split())
```

첫번째 방법은 asterisk(`*`)로 map 객체를 unpacking하여 리스트로 저장하는 것이다.<br>
두번째 방법은 `list()` 함수를 이용하여 map 객체를 리스트로 변환한다.

### 한 줄의 정수를 여러개의 변수로 나누어 저장하기.

```python
a, b, c, d, e = map(int, input().split())
```

`map()`을 통해 입력 받은 정수형 데이터를 곧바로 변수에 순서대로 저장한다.<br>
이 때 `split()`을 통해 나누어진 데이터의 개수와 변수의 개수는 일치해야 한다.<br>
예제의 경우 한 줄에 5개의 데이터가 존재하므로, 5개의 변수를 선언해야 한다.

## 예제 코드 입력 받기

```python
T = int(input())
for test_case in range(1, T + 1):
    my_list = [*map(int, input().split())]
    # ...
```

1. 첫 줄의 정수 5가 변수 T에 저장된다.
2. 입력받은 정수 5를 토대로 for문을 이용해 5회 반복 실행되도록 한다.
3. my_list에 각 줄의 정수형 데이터를 저장한다.
4. 이제 my_list를 바탕으로 test case를 적용하면 된다!

## 이 글을 쓰게 된 계기...

사실 python의 기초도 못뗀채로 무작정 알고리즘 문제를 풀려고하다 input부터 막혔다.<br>
이렇게 정리해두면 어느정도 원리도 파악하고, 잘 잊지 않게 될 것 같다.<br>
지금은 D1 문제나 깔짝이고 있지만... 조만간 D3 문제도 무리없이 풀 수 있게되길 바란다.
