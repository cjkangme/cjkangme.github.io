

        ---
        title: [TIL] 파이썬 filter 함수
        date: 2023-01-16 04:08:41.079 +0000
        categories: [TIL]
        tags: ['python']
        description: filter(조건 함수, iterable 객체)filter 함수는 iterator 중에서 조건 함수를 만족하는 값만을 filter 객체에 담아 반환한다.filter 객체 역시 iterator이므로, 리스트 등 다른 자료형으로 변환하여 사용하면 된다.'아... 리스트에서
        image: /assets/img/posts/2023-01-16-til-파이썬-filter-함수/thumbnail.png
        
        ---

        ## `filter` 함수

> `filter(조건 함수, iterable 객체)`

`filter` 함수는 iterator 중에서 조건 함수를 만족하는 값만을 **filter 객체**에 담아 반환한다.
filter 객체 역시 iterator이므로, 리스트 등 다른 자료형으로 변환하여 사용하면 된다.

## 유용성

_'아... 리스트에서 최소값을 찾고 싶은데, N보다 큰 값이어야 하면 어떻게 찾지?'_

이럴 때 filter를 이용할 수 있다.

## 사용 방법

### 정의된 함수 사용

- 리스트에서, 10보다 작은 값을 오름차순 정렬하기

```python
def less_10(n):
    if n < 10:
        return True
    else:
    	return False

a = [14, 5, 10, 2, 15, 13, 6, 3, 11, 7, 12, 19]
result = sorted(list(filter(less_10, a)))

print("원래 리스트 : ", a)
print("필터 후 결과 : ", result)
```

```text
원래 리스트 :  [14, 5, 10, 2, 15, 13, 6, 3, 11, 7, 12, 19]
필터 후 결과 :  [2, 3, 5, 6, 7]
```

### lambda 함수 사용

- 일회용으로 사용한다면, lambda 함수를 사용하여 한 라인에 해결할 수 있다.
- `a[3:]` 부터의 값 중 `a[2]` 보다 큰 값을 오름차순 정렬하기

```python
a = [14, 5, 10, 2, 15, 13, 6, 3, 11, 7, 12, 19]
result = sorted(list(filter(lambda x: x > a[2], a[3:])))

print("원래 리스트 : ", a)
print("필터 후 결과 : ", result)
```

```text
원래 리스트 :  [14, 5, 10, 2, 15, 13, 6, 3, 11, 7, 12, 19]
필터 후 결과 :  [11, 12, 13, 15, 19]
```

## 예시

[백준 10972. 다음 순열](https://www.acmicpc.net/problem/10972) 문제에서 다음과 같이 활용하였다.

```python
import sys
input = sys.stdin.readline

if __name__ == "__main__":
    N = int(input())
    lst = list(map(int, input().split()))
    answer = []
    set_answer = set()

    for i in range(N-1, 0, -1):
        if lst[i] > lst[i-1]:
            lst[i-1] = min(filter(lambda x : x>lst[i-1], lst[i:]))
            answer = lst[:i]
            set_answer = set(answer)
            break
    if answer:
        for j in range(1, N+1):
            if j not in set_answer:
                answer.append(j)
        print(*answer)
    else:
        print(-1)
```



        