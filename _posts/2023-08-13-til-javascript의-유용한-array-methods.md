

---
title: [TIL] Javascript의 유용한 Array Methods
date: 2023-08-13 13:52:59.055 +0000
categories: [TIL]
tags: ['javascript', 'til']
description: map, reduce, filter, some, every에 대해 알아보자
image: /assets/img/posts/2023-08-13-til-javascript의-유용한-array-methods/thumbnail.png
math: true
---

> 참고 영상 : https://youtu.be/WdSQK72G7tw

Javascript의 Array 객체에서 사용할 수 있는 유용한 알고리즘들에 대해 알아보자


## map()

- 배열내의 모든 요소를 매개변수 하여, 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환한다.

```jsx
const arr = [1, 2, 3, 4, 5]

const result = arr.map((num) => num * 2)

console.log(result)
```

![](/assets/img/posts/2023-08-13-til-javascript의-유용한-array-methods/img0.png)


- 객체에 대해서도 적용이 가능하다.

```jsx
class MySkill {
  constructor(name, score) {
    this.name = name;
    this.score = score;
  }
}

const javascript = new MySkill("javascript", 30);
const python = new MySkill("python", 50);
const typescript = new MySkill("typescript", 10);

const mySkills = [javascript, python, typescript];

mySkills.map((mySkill) => {
  console.log(`My $$ {mySkill.name} score is  $${mySkill.score}.`);
});
```

![](/assets/img/posts/2023-08-13-til-javascript의-유용한-array-methods/img1.png)


## some()

- python의 `any()` 와 비슷하다.
- 배열 안의 요소 중 하나라도 주어진 조건 함수에 true가 된다면 true를 반환한다.

```jsx
const numbers = [1, 2, 3, 4, 5];

const isHavingThree = numbers.some((number) => number === 3)

const isHavingZero = numbers.some((number) => number === 0)

console.log(`Is having 3? ${isHavingThree}`);
console.log(`Is having 0? ${isHavingZero}`);
```

![](/assets/img/posts/2023-08-13-til-javascript의-유용한-array-methods/img2.png)

### some의 순회 종료

- 순회를 하다가 true가 하나라도 나오면 순회를 종료한다.

```jsx
const numbers = [1, 2, 3, 4, 5];

const isHavingThree = numbers.some((number) => {
  console.log(`Searching... ${number}`);
  return number === 3;
});

console.log(`Is having 3? ${isHavingThree}`);

const isHavingZero = numbers.some((number) => {
  console.log(`Searching... ${number}`);
  return number === 0;
});

console.log(`Is having 0? ${isHavingZero}`);
```

![](/assets/img/posts/2023-08-13-til-javascript의-유용한-array-methods/img3.png)

- 3을 찾는 경우 3을 탐색하자마자 순회가 종료되었지만, 0을 찾을 경우 배열 끝까지 순회한 후 `false`를 출력하였다.

## every()

- python의 `all()` 과 비슷하다.
- 배열 안의 요소 모두가 주어진 조건 함수 또는 조건식에 true를 반환해야 true를 반환한다.

```jsx
const numbers = [0, 1, 2, 3, 4, 5];

const isNumber = numbers.every((number) => typeof number === "number");

const isPositve = numbers.every((number) => number > 0);

console.log(`Is every elemnet number? ${isNumber}`);
console.log(`Is every element positive? ${isPositve}`);
```

![](/assets/img/posts/2023-08-13-til-javascript의-유용한-array-methods/img4.png)

### every의 순회 종료

- 순회를하다가 false가 하나라도 나오면 순회가 종료된다.

## filter()

- python의 `filter()`와 비슷하다.
- 배열 안의 요소 중 주어진 조건식이나 함수의 결과가 true인 요소만을 모아 새로운 배열로 반환한다.
- 즉 false인 요소들은 제거된다.

```jsx
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];

const even_numbers = numbers.filter((number) => number % 2 === 0)

console.log(even_numbers)
```

![](/assets/img/posts/2023-08-13-til-javascript의-유용한-array-methods/img5.png)


## reduce()

- 파이썬3에서는 `functools` 내장모듈의 `reduce`를 import 하여 사용할 수 있다.
- 배열의 각 요소에 대해 주어진 **리듀서(reducer) 함수**를 실행하여 하나의 결과를 반환한다.

### 리듀서 함수

- 리듀서 함수는 **리듀스(Reduce) 함수**를 인자로 전달할 수 있다.
- JS의 리듀스 함수는 누적값을 계산하는 함수이다.
    - 즉 배열의 요소를 누적값 계산을 통해 단 하나의 값으로 줄이는 역할을 한다.
    - 리듀스 함수가 반환한 값은 `acc` 에 저장된다.
- 리듀스(Reduce) 함수의 매개변수
    - `acc` (prev) : 누적값
    - `cur` (curr) : 현재값
    - `idx?` : 현재 인덱스
    - `src?` : 원본 배열
- 리듀서 함수의 매개 변수
    - `reduce(acc, cur, idx, src)` : 리듀스 함수
    - `initVal?` : acc의 초기값 (생략 가능)

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];

const reducedNumbrs = numbers.reduce((acc, cur, idx) => {
  console.log(`acc: $$ {acc}, cur:  $${cur}, idx: ${idx}`);
  return acc + cur;
}, 0);

console.log(reducedNumbrs);
// 45
```

![](/assets/img/posts/2023-08-13-til-javascript의-유용한-array-methods/img6.png)

※ 리듀스 함수의 결과를 반환하지 않으면 acc 값은 undefined가 된다.

## 코멘트

파이썬을 사용하면서 경험해본 것들이라 다소 익숙했다.

다만 `reduce` 메서드는 본 적이 없어서 신기했는데, 매번 별도로 변수를 선언하고 for loop을 돌면서 변수값을 갱신할 필요 없이 reduce로 해결하면 훨씬 코드를 적게 작성하면서 원하는 기능을 처리할 수 있을 것 같다.

python3에서도 내장모듈을 import 하는 것을 통해 사용할 수 있다니 기회가 된다면 알고리즘 문제 풀이에서 이용해봐야겠다.

        