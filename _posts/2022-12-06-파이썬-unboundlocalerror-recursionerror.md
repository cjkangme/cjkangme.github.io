

        ---
        title: [파이썬] UnboundLocalError, RecursionError
        date: 2022-12-06 05:40:48.501 +0000
        categories: [error]
        tags: ['python', '백준', '에러']
        description: 백준: 손익분기점 문제https&#x3A;//www.acmicpc.net/problem/1712위의 백준 문제를 풀어보다 2가지 에러와 마주했다.간단히 설명하자면 def, if, for, while 등의 안에서 선언된 변수를 바깥에서 사용할 때 발생하는 에러이다. 지역
        
        
        ---

        > 백준: 손익분기점 문제
> https://www.acmicpc.net/problem/1712

위의 백준 문제를 풀어보다 2가지 에러와 마주했다.

## UnboundLocalError

간단히 설명하자면 def, if, for, while 등의 안에서 선언된 변수를 바깥에서 사용할 때 발생하는 에러이다. 지역 변수를 전역 변수처럼 사용하려고 할 때 발생한다고 볼 수 있다.

```python
A, B, C = map(int,input().split())
profit = C - B
cnt = 0
total = 0
def break_even_point():
    if A >= total:
        cnt += 1 # <- 여기
        total += profit # <- 여기
        return break_even_point()
    else:
        print(cnt)
if profit > 0:
    break_even_point()
else:
    print(-1)
```
`+=`를 사용할 때 cnt와 total을 '재할당' 하게 되는데, 이때 파이썬은 재할당된 변수를 `break_even_point()` 안에서 선언된 지역변수로 간주하게 된다.

즉 재귀를 통해 다시 함수를 호출했을 때 해당 지역변수는 없어진거나 다름 없기 때문에 UnboundLocalError가 발생하게 된다.

권장되진 않지만 굳이 써야겠다면 문제가 생기는 변수 할당 부분 앞에 `global`을 붙여 전역변수로 만들어서 사용하면 된다.

## RecursionError

```python
A, B, C = map(int,input().split())
profit = C - B
def break_even_point(total, cnt): #total과 cnt를 함수의 인자로 만들었다.
    if A >= total:
        cnt += 1
        total += profit
        return break_even_point(total, cnt) # <- 여기
    else:
        print(cnt)
if profit > 0:
    break_even_point(0, 0)
else:
    print(-1)
```
이번엔 RecursionError가 발생했다
"Recursion"은 재귀란 뜻으로, 말그대로 최대 재귀 깊이보다 재귀의 길이가 더 깊어질 때 발생하는 오류이다.

예를들어 A값이 21억이고, profit 값이 1이라면, 재귀를 21억 + 1번 해야하므로 RecursionError가 발생한 것이다.

BOJ Help에 따르면 백준 채점서버에서 파이썬의 기본 재귀 깊이는 1,000이라고 한다.

`sys.getrecursionlimit()`를 통해서 현재 최대 재귀 깊이를 알 수 있으며
`sys.setrecursionlimit()`를 통해 최대 재귀 깊이를 설정할 수 있다. (설정해도 백준 서버가 감당하지 못할 정도로 재귀 깊이가 깊어지면 다시 오류가 발생한다)

> 출처 : https://help.acmicpc.net/judge/rte/RecursionError

## 해결
```python
A, B, C = map(int,input().split())
profit = C - B
if profit > 0:
    break_even_point = (A // profit) + 1
    print(break_even_point)
else:
    print(-1)
```
생각해보니.. 굳이 재귀로 풀 필요가 없는 문제였다 ㅎㅎ
결과를 먼저 생각하고 최적의 과정을 찾아야하는데, 과정부터 접근하니 재귀로 풀 생각을 하게 된 것 같다.
정답률이 20%대던데.. 아마 많은 사람들이 나와 비슷한 실수를 하지 않았을까 싶다.

        