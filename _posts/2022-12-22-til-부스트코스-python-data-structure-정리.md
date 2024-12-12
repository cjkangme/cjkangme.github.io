

        ---
        title: [TIL] 부스트코스 Python Data Structure 정리
        date: 2022-12-22 06:16:05.511 +0000
        categories: [부스트코스-PreCourse]
        tags: ['부스트코스', '프리코스']
        description: 데이터 구조(Data Structure) : 특징이 있는 데이터를 효율적으로 저장하기 위해 필요한 구조LIFO(Last In First Out) 구조 : 나중에 넣은 데이터를 먼저 반환데이터의 입력을 Push, 출력을 Pop이라고 한다.파이썬에서는 리스트를 사용하여 스
        
        
        ---

        # 파이썬의 기본 데이터 구조
데이터 구조(Data Structure) : 특징이 있는 데이터를 효율적으로 저장하기 위해 필요한 구조

## 스택(Stack)
- LIFO(Last In First Out) 구조 : 나중에 넣은 데이터를 먼저 반환
- 데이터의 입력을 Push, 출력을 Pop이라고 한다.

### 리스트
- 파이썬에서는 리스트를 사용하여 스택 구조를 구현할 수 있다.
- push는 `list.append()`, pop은 `list.pop()`을 통해 사용할 수 있다.

```python
a = [1, 2, 3, 4, 5]
a.pop() # 5 반환
a.pop() # 4 반환
a.append(6)
print(a) # [1, 2, 3, 6]
```

## 큐 (Queue)
- FIFO(First In First Out) 구조 : 먼저 넣은 데이터를 먼저 반환
- Stack과 반대되는 개념이다.
- 데이터의 입력을 Put, 출력을 Get이라고 한다.

### 리스트
- 리스트를 통해서 큐 역시 구현할 수 있다.
- put은 `list.append()`, get은 `list.pop(0)`를 통해 사용할 수 있다.
- 하지만 `list.pop(0)`은 O(n)의 시간복잡도를 갖기 때문에, deque를 더 많이 사용한다.

### deque
- 파이썬의 `collections` 내장 모듈에 들어 있는 자료구조이다.
- 큐에서 필요한, `popleft()`를 사용할 수 있다.
- deque의 자세한 내용은 뒤쪽에...

```python
from collections import deque
a = deque([1, 2, 3, 4, 5])
a.popleft() # 1 반환
a.append(6)
a.popleft() # 2 반환
print(a) # [3, 4, 5, 6]
```

## 튜플 (tuple)
- 값의 변경이 불가능한 리스트이다.
- 선언 시 소괄호(`()`)를 사용한다.
- 값의 변경은 불가능하지만, 인덱싱 및 슬라이싱은 동일하게 사용 가능하며, 연산을 통해 튜플을 합치는 것도 가능하다.

### 튜플을 사용하는 이유
- 프로그램이 작동하는 동안 변경되서는 안되는 데이터를 저장하기 위해 사용한다.
- 사용자의 실수로 에러가 발생하는 것을 방지할 수 있다.

## 집합 (set)
- 값을 순서 없이 저장하며, 중복을 허용하지 않는 자료구조이다.
- 순서가 없기 때문에 인덱싱을 이용한 호출이 불가능하다.
- 중복값을 모두 제거하기 때문에 중복을 허용하지 않는 데이터나 집합 연산에 주로 사용한다.
- `if x in set`과 같은 조건문에서 리스트보다 훨씬 빠르기 때문에 사용하기도 한다.

### 집합 메서드
- `s.add(n)` : n을 집합에 추가한다. n이 이미 있다면 추가하지 않는다.
- `s.remove(n)` : n을 집합에서 찾아 삭제한다. n이 없다면 keyError가 발생한다.
- `s.discard(n)` : n을 집합에서 찾아 삭제한다. n이 없어도 에러가 발생하지 않는다.
- `s.clear()` : 집합의 모든 원소를 삭제한다.

### 집합 연산
- `s1.union(s2)` : s1과 s2의 합집합을 구한다.
	- s1 + s2와 동일하다.
- `s1.intersection(s2)` : s1과 s2의 교집합을 구한다.
	- s1 & s2와 동일하다.
- `s1.difference(s2)` : s1과 s2의 차집합을 구한다.
	- s1 - s2와 동일하다.

## 딕셔너리(dict)
- 데이터를 key-value 한 쌍의 형태로 저장한다.
- 저장된 데이터는 key 값을 통해 서로 구분지어진다.
- 다른 언어에서는 Hash Table이라는 용어를 사용하기도 한다. (dict와 완전히 동일하지 않음)

### 딕셔너리 선언
- 중괄호(`{}`) 또는 `dict()` 함수를 통해 선언이 가능하다.
- a = {"name" : "cjkangme", "nickname" : "nureongi"} 와 같이 `key:value`로 값을 추가할 수 있다.
- a\["name"\] = "cjkangme" 와 같이 `dict[key] = value`로 값을 추가할 수 있다.

### 딕셔너리 조회
- `dict[key]`를 통해 value를 가져올 수 있다. (key가 없다면 keyError 발생)
- `dict.items()` : key-value 쌍을 튜플에 담아 출력한다.
	- 주로 for문과 함께 사용하여 key, value에 동시해 접근하기 위해 사용한다.
- `dict.keys()` : 딕셔너리의 key만 출력한다.
- `dict.values()` : 딕셔너리의 value만 출력한다.

# collections
- 파이썬에 내장된 확장 자료 구조(모듈)이다.
- 기존 리스트, 튜플, 집합보다 편의성과 실행 효율이 높은 자료 구조를 제공한다.

## deque
- stack과 queue를 지원하는 모듈로, 보다 효율적인(빠른) 자료 저장 방식을 지원한다.
- 기존 리스트의 함수를 모두 지원하면서, rotate, reverse와 같은 Linked List의 특성을 지원한다.

### 메서드
`dq.rotate(n)` : `dq.appendleft(dq.pop())`과 같은 행동을 n회 수행한다.
`dq.extend(iterable)` : dq 리스트 뒤에 인자를 추가한다.
`dq.extendleft(iterable)` : dq 리스트 앞에 인자를 reversed된 형태로 추가한다.

## OrderedDict
- 기존 일반 dict는 호출 시 입력한 순서를 보장하지 않았지만, OrderedDict는 입력한 순서대로 dict를 반환한다.
- 하지만 파이썬 3.6부터는 일반 dict도 입력한 순서를 보장하기 때문에 현재는 사용할 일이 없다.

## defaultdict
- dict에 기본값을 지정하여, 신규값 생성시 value를 지정하지 않으면 기본값으로 생성된다.
- 기본값은 반두시 함수 형태로 선언해야 한다.

```python
from collections import defaultdict
d = defaultdict(object) # defaultdict생성
d = defaultdict(lambda: 0) # 기본값을 0으로 지정
print(d["anything"]) # 0
```

### 장점
- 일반 dict에서는 없는 key를 호출할 경우 keyError가 발생하지만, defaultdict는 기본값으로 생성하여 기본값을 value로 불러온다.
- Text-mining에 활용할 수 있다.

## Counter
- 시퀀스 자료형의 데이터 개수를 dict 형태로 반환한다.
	- Sequence type : int형의 인덱스를 통해 원소에 접근할 수 있는 iterable을 말한다. 리스트, 튜플, 문자열이 여기에 해당한다.
    
```python
from collections import Counter

c = Counter()
c = Counter('aivleschool')
print(c) # Counter({'l': 2, 'o': 2, 'a': 1, 'i': 1, 'v': 1, 'e': 1, 's': 1, 'c': 1, 'h': 1})
```

### 특징
- set의 연산들을 지원한다.
	- `c - d` : set의 차집합과 같다.
    - `c + d` : set과는 다르게 각 요소를 그대로 더한다. ( 4 + (-2) = 2 )
    - `c | d` : set의 합집합과 같다. ( 4 + (-2) = 4 )
    - `c & d` : set의 교집합과 같다.
    
## namedtuple
- 튜플 형태로 데이터의 **구조체**를 저장한다.
	- 구조체 : 데이터를 저장할 때 어떤 형태로 저장할지 사전에 약속한 것(스키마와 비슷)
- 저장되는 data의 variable을 사전에 지정할 수 있다.
- 데이터의 기본적인 체계를 하나로 묶을 수 있다는 장점이 있다.
- 파이썬에서는 대부분 클래스를 사용하기 때문에 많이 사용하지는 않는다.

        