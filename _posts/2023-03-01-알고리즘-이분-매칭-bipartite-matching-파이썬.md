

---
title: [알고리즘] 이분 매칭 (Bipartite Matching) (파이썬)
date: 2023-03-01 09:26:46.437 +0000
categories: [Algorithm]
tags: ['aivle', 'til', '백준', '알고리즘', '이분매칭']
description: 백준 문제 풀이와 함께 알아보는 이분 매칭 알고리즘 (1671. 상어의 저녁식사)
image: /assets/posts/2023-03-01-알고리즘-이분-매칭-bipartite-matching-파이썬/thumbnail.png

---

> 참고자료
> - [29. 이분 매칭(Bipartite Matching)](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ndb796&logNo=221240613074)
> 알고리즘의 단계가 하나하나 잘 설명되어 있다. 강의도 하시는 분인지 유튜브 링크도 있다.
> - [[알고리즘] 이분 매칭 알고리즘 (Bipartite Matching)](https://yjg-lab.tistory.com/209)
> 마찬가지로 알고리즘의 단계 하나하나를 잘 설명하셨다. 가독성도 뛰어남
> - [이분 매칭(Bipartite Matching)](https://blog.naver.com/kks227/220807541506)
> 알고리즘을 구현 위주로 잘 설명하셨고, 시도할 수 있는 백준문제가 제시되어 있다.

# 이분 매칭

![img](/assets/posts/2023-03-01-알고리즘-이분-매칭-bipartite-matching-파이썬/img0.png)

- 위와 같은 이분 그래프에서 서로를 매칭시켜준다고 가정해보자.

- 가령 위 그림에서 1-5, 2-4, 3-6이 매칭된다면 매칭의 수는 3이다.

- 하지만 1-6, 2-5가 매칭되면 3은 매칭할 상대가 없으며, 매칭의 수는 2가 된다.

- 이분 매칭은 이러한 이분 그래프에서 **최대 매칭의 수**를 구하는 알고리즘이다.

## 이분 그래프

이분 매칭을 알기 위해서는 사전지식으로 **이분 그래프**에 대해 개념 정도는 알고 있어야 한다.

부끄럽지만 이전에 정리해둔 것이 있다.

> [[백준] 1707. 이분 그래프(파이썬)](https://velog.io/@cjkangme/%EB%B0%B1%EC%A4%80-1707.-%EC%9D%B4%EB%B6%84-%EA%B7%B8%EB%9E%98%ED%94%84%ED%8C%8C%EC%9D%B4%EC%8D%AC)

## 네트워크 유량

이분 매칭은 사실 네트워크 유량 알고리즘으로 설명할 수 있다.

![img](/assets/posts/2023-03-01-알고리즘-이분-매칭-bipartite-matching-파이썬/img1.png)

- 이분 그래프의 최대 매칭 수는 모든 간선 용량이 1일 때 source에서 sink로 흐르는 최대 유량을 구하는 것과 같다.

- 이는 에드몬드-카프 알고리즘을 통해 구할 수 있다.

> [[알고리즘] 네트워크 유량 (Network Flow) (파이썬)](https://velog.io/@cjkangme/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%9C%A0%EB%9F%89-Network-Flow-%ED%8C%8C%EC%9D%B4%EC%8D%AC)

- 하지만 에드몬드-카프 알고리즘의 시간 복잡도는 **O(V \* E^2)** 이다. 이분 그래프를 사용하는 이분 매칭에 한해서는 **O(V * E)**의 시간 복잡도로 풀 수 있는 알고리즘이 존재한다.

## 알고리즘 설명

![img](/assets/posts/2023-03-01-알고리즘-이분-매칭-bipartite-matching-파이썬/img2.png)

- 우선 한쪽 집합을 선택해서 모든 노드가 매칭상대를 찾도록 for loop를 돈다.

- 1-5에서 5번 노드가 비어있으므로 그대로 연결할 수 있다.

![img](/assets/posts/2023-03-01-알고리즘-이분-매칭-bipartite-matching-파이썬/img3.png)

- 2-4도 연결할 수 있으므로 연결한다.

![img](/assets/posts/2023-03-01-알고리즘-이분-매칭-bipartite-matching-파이썬/img4.png)

- 3-5에서 충돌이 발생하였다.

- 5번 노드에는 부모노드가 1번으로 저장되어 있으므로, 1번 노드에게 다른 매칭 상대를 찾을 것을 요청한다. (DFS로 1번 노드에 대해 재귀적 호출을 함)

![img](/assets/posts/2023-03-01-알고리즘-이분-매칭-bipartite-matching-파이썬/img5.png)

- 1-6이라는 다른 매칭이 있으므로, 매칭을 한 뒤 True를 retrun하여 다른 매칭을 했음을 알려준다.

![img](/assets/posts/2023-03-01-알고리즘-이분-매칭-bipartite-matching-파이썬/img6.png)

- 기존 1-5 매칭은 끊어지고, 3-5 매칭이 생성된다 (5번 노드의 부모가 3이 됨)

- 모든 노드를 방문 했으니 탐색을 종료한다. 매칭 개수는 3개이다.

- 만약 1에게 다른 매칭상대가 없는 상황이라면 기존 매칭은 유지하고 3의 다음 매칭 상대를 탐색한다. 만약 모든 3을 탐색했는데도 매칭이 되지 않았다면 방문 처리만 하고 다음 노드로 넘어간다.

# 코드 (상어의 저녁식사)

> 문제 : [1671. 상어의 저녁식사](https://www.acmicpc.net/problem/1671)

## 문제 요약

- 잡아 먹는 쪽이 매칭을 시작하는 집합, 잡아 먹히는 쪽이 매칭되는 집합으로 분류되는 이분 매칭 문제이다.


- 능력치가 같은 상어도 서로 잡아 먹을 수 있는데, 잡아 먹힌 상어가 다른 상어를 먹을 수 없으므로 기준을 잡고 나누어야 한다.
    - 따로 조건문을 걸어 인덱스가 큰쪽만 집합에 추가하는 식으로 한다.
    

- 한 상어가 두 마리를 잡아 먹을 수 있다. 즉 잡아 먹는 쪽 집합은 2개의 매칭을 할 수 있다.
    - 노드 하나 당 2번의 DFS를 돌면 된다.
    
    
## 코드

```python
import sys
input = sys.stdin.readline

def DFS(shark):
    if visited[shark]:
        return False
    visited[shark] = True
    
    for food in eat[shark]:
        if match[food] == -1 or DFS(match[food]):
            match[food] = shark
            return True
    return False

N = int(input())
sharks = []
for _ in range(N):
    sharks.append(list(map(int, input().split())))
# 잡아먹을 수 있는 상어를 eat에 넣어줌
eat = [[] for _ in range(N)]
for i in range(N):
    for j in range(N):
        if i == j:
            continue
        # 능력치가 같다면 인덱스가 작은 쪽이 잡아먹히도록 함
        if (sharks[i][0] == sharks[j][0] 
            	and sharks[i][1] == sharks[j][1] 
            	and sharks[i][2] == sharks[j][2]):
            if i < j:
                continue
                
        if (sharks[i][0] >= sharks[j][0] 
            and sharks[i][1] >= sharks[j][1] 
            and sharks[i][2] >= sharks[j][2]):
            
            eat[i].append(j)
            
match = [-1 for _ in range(N)]
# 잡아 먹히는 상어 수(= 매칭 수) 만큼 상어가 줄어드므로 N부터 시작해 매칭당 1씩 감소 함
answer = N
for i in range(N):
    for _ in range(2):
        visited = [False for _ in range(N)]
        if DFS(i):
            answer -= 1

print(answer)
```

        