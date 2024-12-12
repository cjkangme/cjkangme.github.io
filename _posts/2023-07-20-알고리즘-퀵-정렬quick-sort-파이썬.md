

        ---
        title: [알고리즘] 퀵 정렬(Quick Sort) (파이썬)
        date: 2023-07-20 12:28:12.850 +0000
        categories: [Algorithm]
        tags: ['백준', '알고리즘', '정렬']
        description: 백준 안테나 문제풀이와 함께 알아보는 퀵 정렬
        image: /assets/img/posts/2023-07-20-알고리즘-퀵-정렬quick-sort-파이썬/thumbnail.png
        
        ---

        # 퀵 정렬

## 정의

퀵 정렬은 이름 그대로 빠른 정렬을 뜻한다.
평균 시간 복잡도가 O(NlogN)으로 다른 정렬(버블 정렬, 순차 정렬 등)보다 훨씬 빠르다.
하지만 최악 시간 복잡도는 O(N^2)이며, 불안정 정렬이라는 단점이 있다.

> ※ 불안정 정렬이란 : `[4, 2, 2, 1]` 을 정렬할 때, 정렬 후 2의 순서를 보장할 수 없는 정렬을 의미한다. 1번 인덱스에 있는 2와, 2번 인덱스에 있는 2는 정렬 후 순서가 바뀔 수 있다.

## 원리

퀵 정렬의 한 사이클은 먼저 **pivot**이라는 기준값을 두고, pivot보다 작다면 왼쪽, pivot보다 크다면 오른쪽에 두는 정렬을 진행한다.

한 사이클을 돌면 pivot 왼쪽에는 모두 pivot보다 작은 수가, 오른쪽에는 모두 pivot보다 큰 수가 존재하게 된다.

이제 pivot의 위치를 기준으로 배열을 둘로 나누어 각각 새로운 pivot을 선정하여 사이클을 돌리면 모든 배열이 정렬된다.

### 사이클

#### 1. pivot 선정

![](/assets/img/posts/2023-07-20-알고리즘-퀵-정렬quick-sort-파이썬/img0.png)

배열에서 pivot이 될 값을 선정한다.
아무 값이나 선정해도 상관 없지만, 보통 `(left + right) // 2`나 제일 왼쪽값, 오른쪽 값을 선정한다.

#### 2. 교환할 값 찾기

![](/assets/img/posts/2023-07-20-알고리즘-퀵-정렬quick-sort-파이썬/img1.png)

이제 pivot을 중심으로 교환할 값을 찾아야 한다.

- left : 배열의 가장 왼쪽부터 시작해서 pivot보다 크거나 같은 값이 나올 때까지 탐색
- right : 배열의 가장 오른쪽부터 시작해서 pivot보다 작거나 같은 값이 나올 때까지 탐색

탐색조건에 '같은 값'이 있는 이유는 pivot을 만났을 때 멈추기 위함이다.

#### 3. 교환하기

![](/assets/img/posts/2023-07-20-알고리즘-퀵-정렬quick-sort-파이썬/img2.png)

- left는 2, 5, 6 순으로 탐색하다가 pivot과 같은 값을 만나서 정지했다.
- right의 4가 이미 pivot보다 작은 값이기 때문에 더 이상의 탐색 없이 정지했다.

![](/assets/img/posts/2023-07-20-알고리즘-퀵-정렬quick-sort-파이썬/img3.png)

이제 left와 right를 교환하면 된다.
두 값을 교환했으므로 left, right를 각각 전진시킨다.

#### 4. left, right가 역전될 때까지 반복

![](/assets/img/posts/2023-07-20-알고리즘-퀵-정렬quick-sort-파이썬/img4.png)

`left > right`가 될 때까지 1 ~ 3의 과정을 반복한다.

이번의 경우에는 추가 교환 없이 left가 pivot보다 크거나 같은 값이 될 때까지 탐색하는 과정에서 역전이 발생하였다.

#### 5. pivot을 기준으로 재귀적 탐색

![](/assets/img/posts/2023-07-20-알고리즘-퀵-정렬quick-sort-파이썬/img5.png)

보라색 테두리가 다음에 탐색할 범위이다.

pivot이 가운데 있었다면 오른쪽에도 보라색 테두리 배열이 하나 더 생겼겠지만, 이번 경우에는 pivot보다 왼쪽에 있는 배열 하나만을 탐색하면 된다.

이제 이를 코드로 구현하는 것이 중요한데

```python
# L, R : 이번에 탐색한 배열의 시작과 끝
# left, right : 탐색 후 포인터의 위치
if L < right:
    quick_sort(L, right)
if left < R:
    quick_sort(left, R)
```

위와 같이 배열의 왼쪽 끝부터 right 포인터의 배열 / left 포인터부터 오른쪽 끝까지의 배열 길이가 최소 2일 때 새로 탐색하면 된다.

#### 6. 반복

![](/assets/img/posts/2023-07-20-알고리즘-퀵-정렬quick-sort-파이썬/img6.png)

같은 과정을 반복했을 때 탐색범위(보라색)과 pivot(빨간색)은 위와 같다.

## 연습문제

> 문제 : https://www.acmicpc.net/problem/18310

전형적인 정렬문제인 18310. 안테나를 준비하였다.

![](/assets/img/posts/2023-07-20-알고리즘-퀵-정렬quick-sort-파이썬/img7.png)

문제를 쉽게 설명하면 N개 집의 위치를 정렬했을 때 `(N-1)//2` 번째 집의 위치가 곧 정답이 되는 문제이다.

단 집의 위치가 뒤죽박죽으로 주어지므로, 퀵소트를 통해 정렬을 거쳐야 한다.

### 코드

집의 위치를 입력받고, 퀵소트를 정렬한 다음에
`(N-1)//2`번째 집의 위치를 출력하는 코드이다.

```python
import sys

input = sys.stdin.readline

def quick_sort(L, R):
    pivot = antennas[(L + R) // 2]
    left, right = L, R
    
    while left <= right:
        # L의 값이 pivot의 값보다 클 경우 중단
        while antennas[left] < pivot:
            left += 1
        # R의 값이 pivot의 값보다 작을 경우 중단
        while pivot < antennas[right]:
            right -= 1
        # 아직 포인터가 역전되지 않았다면 교환
        if left <= right:
            antennas[left], antennas[right] = antennas[right], antennas[left]
            left += 1
            right -= 1
            
    # 재귀적 탐색
    if L < right:
        quick_sort(L, right)
    if left < R:
        quick_sort(left, R)
    

if __name__ == "__main__":
    N = int(input())
    antennas = list(map(int, input().split()))
    
    # 정렬 수행
    quick_sort(0, N-1)
    
    # 정답출력
    print(antennas[(N-1) // 2])
```

# 코멘트

사실 정렬을 맨날 `list.sort(), sorted()`로 때우다 보니 정작 합병정렬, 퀵 소트와 같은 정렬의 기본을 잘 모르고 있었다.

티어도 어느정도 올렸으니 이제는 조금 더 기본에 충실하고 싶어서 퀵 소트를 정리해보았다.


        