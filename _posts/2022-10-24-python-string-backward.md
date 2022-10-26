---
title: '[Python] 문자열 거꾸로 정렬하기'
excerpt: '파이썬에서 문자열을 거꾸로 정렬하는 방법'

categories:
  - TIL
tags:
  - [python, algorithm]

permalink: /python/python-string-backward

toc: true
toc_sticky: true

date: 2022-10-24
last_modified_at: 2022-10-24
---

# 파이썬에서 문자열 거꾸로 정렬하기

SWEA에서 알고리즘 문제를 풀던 중, 초심자의 회문 검사라는 문제를 알게되었다.<br>
[문제 링크](https://swexpertacademy.com/main/code/problem/problemDetail.do?problemLevel=2&contestProbId=AV5PyTLqAf4DFAUq&categoryId=AV5PyTLqAf4DFAUq&categoryType=CODE&problemTitle=&orderBy=FIRST_REG_DATETIME&selectCodeLang=ALL&select-1=2&pageSize=10&pageIndex=1)

문제는 정말 쉬웠지만, 문자열을 거꾸로 저장하기 위해 다음과 같이 구린내 풀풀나는 for문을 이용하였다.

```python
text = input()
reverse = list(range(len(text)))
for i in reverse:
    reverse[i] = text[len(text)-(i+1)]
```

아무리 봐도 이것보다 깔끔하고, 쉬운 방법이 있을 것 같아서 검색해보았더니 아니나 다를까...

## (배열) .reverse() 메소드 이용하기

여러가지 이유로 문자열을 배열로 바꾸어야 할 경우 배열에 제공되는 메소드인 `.reverse()`를 이용하면 된다.<br>
`.reverse()`는 배열을 거꾸로 정렬해주는 메소드이다.<br>
주의해야할 점이은 원래 배열에 덮어써버리기 때문에 원래 배열도 필요하다면 따로 저장해두어야 한다.

```python
string = "hello"
lst = list(string)
lst.reverse()

print(''.join(lst))
# olleh
```

## (문자열, 배열) [::-1] 인덱스 활용하기

문자열을 `[::-1]` 인덱스로 호출하면 된다. 훨씬 쉽고 효과적인 방법이다.<br>
의미는 처음부터 끝까지(`:`) 역순으로 호출(`:-1`)하라는 의미이다.<br>
-1 대신 다른 정수를 입력하여 문자열이나 인덱스를 건너 뛰는 것도 가능하다<br>

복잡하게 문자열을 배열로 변환할 필요도 없고, 리스트와 튜플에도 적용이 가능하다.<br>
`.reverse()`와 달리 거꾸로 정렬된 값을 저장하지 않고 출력하므로, 원하는 변수에 저장하거나 `print()`를 통해 즉시 출력하는 것도 가능하다.<br>

심지어 슬라이싱과 결합하여 특정 인덱스만 골라서 출력할 수 있다.

```python
string = "hello"

print(string[::-1])
# olleh

print(string[3::-1])
# lleh

print(string[3:1:-1])
# ll
```

알고리즘 문제를 조금씩 풀면서 느끼는건데, 정말 유용한 메소드나 함수가 많은 것 같다.<br>
특히 D2 정도의 쉬운 문제에서는 함수 하나로 해결되어버리는 경우도 있으니...<br>
특히 배열 관련한 다양한 메소드가 있는 것 같으니, 이를 숙지하고 문제를 풀어야겠다. 언제까지 for문과 조건문으로 떡칠할거야...
