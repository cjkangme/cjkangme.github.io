---
title: '[Python] 원하는 크기의 list 생성하기'
excerpt: '본문의 주요 내용을 여기에 입력하세요'

categories:
  - python
tags:
  - [python]

permalink: /swea/python-array-range/

toc: true
toc_sticky: true

date: 2022-10-20
last_modified_at: 2022-10-20
---

# Python에서 빈 list 생성하기

알고리즘 문제를 풀다보니, 반복수에 따라 크기가 변하는 리스트를 생성해야하는 경우가 발생했다.<br>
이 참에 리스트의 크기를 정하여 생성하는 방법을 몇가지 정리하여 앞으로 활용해 보려고 한다.

<br>

## [None] \* n

```python
lst = [None] * 10
print(lst)
# 결과 : [None, None, None, None, None, None, None, None, None, None]
```

None외에 원하는 값을 넣으면 해당 값이 리스트에 들어간다.

<br>

## for 문 이용

```python
lst = list()
for i in range(10):
  lst.append(i)
print(lst)
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

for 문 을 사용한 뒤 append를 이용하여 원하는 리스트를 추가하는 방법이다.
사실 이것보다 더 간단한 방법이 있다.

<br>

## range() 함수 이용

```python
lst = list(range(10))
print(lst)
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

for 문을 이용한 것보다 훨씬 간단하게 동일한 리스트를 만들 수 있다.<br>
파이썬 3.x 부터 사용할 수 있다.
