

        ---
        title: [TIL] 의사코드(Pseudo-Code) 작성하기
        date: 2023-02-04 04:37:13.688 +0000
        categories: [TIL]
        tags: ['til']
        description: 슈도코드를 배우고, 직접 작성해보는 TIL
        image: /assets/img/posts/2023-02-04-til-의사코드pseudo-code-작성하기/thumbnail.png
        
        ---

        # 의사코드(Pseudo-Code)란?

개인적으로 의사코드라는 말 보다는 슈도코드가 더 입에 잘 붙기 때문에 여기서는 `슈도코드`라고 하겠다.

슈도는 '가짜', '유사한'라는 의미로, 슈도코는 말 그대로 코드와 유사하지만 진짜 코드는 아닌 코드를 말한다.
보통 파이썬 같은 고수준 언어와 흡사한 형태이고, 엄격한 규칙이 있는 것이 아니기 때문에 일정한 형식만 지키면 자유롭게 작성할 수 있다.

실제 프로그래밍 언어가 아니기 때문에 메모장이나 손 등 편한 곳에 작성해나가면 된다.

# 슈도코드의 장점

1. 프로그래머가 아닌 사람에게 알고리즘을 설명하기 유용하다.
2. 실제 코드를 작성하기 전에 알고리즘을 정리할 수 있다.
3. 코드에 논리적인 에러가 있을 때, 슈도코드를 통해 에러가 어디서 발생하는지 쉽게 찾을 수 있다.

# 슈도코드의 원칙

> 원칙이라기엔 사람마다 각자 쓰는법이 다르기 때문에, 바람직한 방향이라고 생각하면 될 것 같다.

1. 자신만의 일정한 규칙 만들기
엄격한 규칙이 있는 것은 아니지만, 마구잡이로 작성하면 굳이 슈도코드를 작성하는 의미가 없어진다. 자신이 편한 형식을 따르되 남들도 쉽게 이해할 수 있도록 작성하면 된다.

2. 알고리즘의 목표를 명시하고 시작하기
알고리즘의 개략적인 순서를 작성하는 것도 좋은 방법이다.
명확한 목표를 쓰고 시작하면, 작성하다 중간에 헤매는 것을 꽤 잘 막아준다.

3. 한 줄의 하나의 행동만 적기, 위에서 아래 순서로 적기

4. Indent를 명확히 하기
조건문, 반복문을 사용할 때는 Indent를 명확히 해주어야 한눈에 보기 쉽다. (Clearly)
조건문, 반복문을 빠져나오는 것을 확실하게 알려주는 방법도 좋고, 편집기에서 처럼 줄을 그어주는 것도 방법이다.
![](/assets/img/posts/2023-02-04-til-의사코드pseudo-code-작성하기/img0.png)

5. 키워드를 명확히 하기
arr, num과 같은 변수명은 소문자, IF, FOR, PRINT 같은 키워드는 대문자로 구분하면 읽기가 더 쉽다.

# 작성 예시


> boj 6603. 로또 : https://www.acmicpc.net/problem/6603

```python
# https://www.acmicpc.net/problem/6603

import sys
input = sys.stdin.readline


def DFS(L, idx):
    if L == 6:
        print(*answer)
        return
    else:
        for j in range(idx+1, k):
            answer.append(S[j])
            DFS(L+1, j)
            answer.pop()


if __name__ == "__main__":
    while True:
        S = list(map(int, input().split()))
        k = S.pop(0)
        answer = []
        if k == 0:
            break
        for i in range(k-5):
            answer.append(S[i])
            DFS(1, i)
            answer.pop()
        print()
```

```
입력받은 리스트에서 6개를 뽑는 모든 경우의 수를 출력
- 0이 입력될 때 까지 리스트를 입력받는다
- 리스트가 n개일 경우 0번부터 5번이 시작, n-6번부터 n-1번이 마지막이 되도록 순서대로 출력한다.

WHILE 무한 루프:
  list <- INPUT
  num_of_list <- list의 첫번째 값
  list에서 첫번째 값 제거
  
  IF num_of_list = 0 THEN:
    END WHILE
  
  FOR start_index <- 0 TO num_of_list-6:
    count <- 0
    CALL DFS with count, index
    PRINT 줄바꿈
  NEXT FOR
NEXT WHILE
    
FUNCTION BFS(count, index):
  answer <- 답으로 출력될 6개의 정수를 담는 리스트
  answer에 list[index] 추가
  
  IF count = 6 THEN:
    # answer 리스트에 값 6개를 전부 넣었다면 답 출력
    PRINT answer
    END FUNCTION
  END IF
  
  FOR next_index <- 다음 index TO list의 마지막 인덱스:
    CALL DFS with count, next_index
  NEXT FOR  
```

# 소감

최근 알고리즈 문제를 풀면서, 틀린답을 제출하거나 에러가 나는 일이 잦아졌다.
일단 코드를 써놓고 오류를 고치려하니 코드가 누더기가 되거나 아예 새로 짜야하는 상황이 많았다.
이를 개선하려고 슈도코드를 배우고, 직접 작성하는 TIL을 했다.

아직은 작성이 많이 어려운 것 같다. 
오래 걸리고, 읽기 편한지도 잘 모르겠다.

확실한 가이드라인이 있으면 좋은데, 사람에 따라 작성법이 천차만별이다...
프로그래밍 코드와 거의 구분이 되지 않는 경우도 있고, 아예 서술글 수준인 경우도 있었다.
일단 해보면서 내게 맞는 방향을 파악하고 조금씩 쓰는 방법을 개선해 나가도록하자!


        