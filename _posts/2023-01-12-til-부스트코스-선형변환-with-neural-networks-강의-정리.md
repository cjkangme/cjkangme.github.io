

---
title: [TIL] 부스트코스 선형변환 with Neural Networks 강의 정리
date: 2023-01-12 05:54:56.718 +0000
categories: [부스트코스-PreCourse]
tags: ['부스트코스', '프리코스']
description: 강의주소 : https&#x3A;//www.boostcourse.org/ai251/lecture/540318신경망에서의 Wx + b변환은 아래 그림과 같이 표현할 수 있다.선형변환 -> bias에 의한 translation -> 비선형 함수에 의한 변형위 그림을 토대로
image: /assets/posts/2023-01-12-til-부스트코스-선형변환-with-neural-networks-강의-정리/thumbnail.png

---

> 강의주소 : https://www.boostcourse.org/ai251/lecture/540318

# Neural Networks(신경망)에서의 선형변환

- 신경망에서의 **Wx + b**변환은 아래 그림과 같이 표현할 수 있다.

![img](/assets/posts/2023-01-12-til-부스트코스-선형변환-with-neural-networks-강의-정리/img0.png)

> 선형변환 -> bias에 의한 translation -> 비선형 함수에 의한 변형

- 위 그림을 토대로 2차원->2차원 선형변환을 다음과 같이 표현할 수 있다.
![img](/assets/posts/2023-01-12-til-부스트코스-선형변환-with-neural-networks-강의-정리/img1.png)![img](/assets/posts/2023-01-12-til-부스트코스-선형변환-with-neural-networks-강의-정리/img2.png)
- 즉 2차원 모눈종이 공간이 선형변환된 공간은 기울어진 평행사변형으로 이루어진 모눈종이라고 볼 수 있다.

## Affine Layer

- 신경망은 일반적으로 `bias term`을 포함한다.
- 이 경우 선형변환이 아니라, `affine transformation`이 이루어진다.

### 예시

- 벡터화(vectorization)된 픽셀 4개로 이루어진 이미지를 3차원으로 affine transformation 한다고 생각하자.

> **Wx + b** = **h**
> **W**는 이미 학습에 의해 최적화된 가중치 행렬이다.
> **x**는 입력받은 이미지 벡터이다.

![img](/assets/posts/2023-01-12-til-부스트코스-선형변환-with-neural-networks-강의-정리/img3.png)

- 이경우 **Wx** 행렬곱 후 더해지는 `bias term`을 `affine layer`라고 한다.
- `affin layer`에 의해 선형변환이 아니라 `affine transformation`이 된다.

### Column Combinations를 이용한 선형변환

- Affine transformation은 [column combination](https://velog.io/@cjkangme/TIL-%EB%B6%80%EC%8A%A4%ED%8A%B8%EC%BD%94%EC%8A%A4-%EC%84%A0%ED%98%95%EA%B2%B0%ED%95%A9-%EA%B0%95%EC%9D%98-%EC%A0%95%EB%A6%AC#%ED%96%89%EB%A0%AC%EA%B3%B1%EC%9D%98-%ED%91%9C%ED%98%84)에 의해 선형변환으로 표현할 수 있다.
![img](/assets/posts/2023-01-12-til-부스트코스-선형변환-with-neural-networks-강의-정리/img4.png)![img](/assets/posts/2023-01-12-til-부스트코스-선형변환-with-neural-networks-강의-정리/img5.png)![img](/assets/posts/2023-01-12-til-부스트코스-선형변환-with-neural-networks-강의-정리/img6.png)

- 즉 `bias term`은 input의 첫 dimension이나 마지막 dimension에 1을 추가하고, `bias term`을 가중치 행렬에 추가함으로써 선형변환 문제로 해결할 수 있다.

        