

        ---
        title: GAN(Generative Adversarial Networks) 개념 정리
        date: 2024-01-17 11:07:03.829 +0000
        categories: [TIL]
        tags: ['ai', 'til']
        description: 모르는 인공지능 개념 정리하기
        image: /assets/img/posts/2024-01-17-gangenerative-adversarial-networks-개념-정리/thumbnail.png
        
        ---

        # GAN이란?

GAN(Generative Adversarial Networks)은 생성 AI 모델의 한 종류로, '적대적 생성 신경망'이라고 번역된다.

'적대적'이 이름에 들어가는 이유는, 말 그대로 적대적 관계에 있는 서로 다른 두 모델을 만들어 이용하는 것인데, 한 모델의 성능이 올라가면 다른 모델의 성능이 떨어지는 제로섬 게임처럼 설계 되어있어 결과적으로 두 모델 모두가 최상의 결과를 향해 학습을 하게 된다.

가장 유명한 예시로는 '경찰과 위조지폐범'이 있는데, 두 모델 중 한쪽은 위조지폐를 적발하는 경찰, 다른 한쪽은 실제 지폐와 가장 유사한 지폐를 만들어내는 위조지폐범과 같다는 비유이다.
위조지폐범 모델은 경찰이 판정하지 못하는(확률값이 0.5) 최적의 위조지폐를 만들기 위해 학습을 하며, 경찰 역시 잘못 판정한 데이터를 바탕으로 보다 위조지폐를 잘 탐지하도록 경쟁적으로 학습한다.

여기서 경찰을 **discriminator**, 위조지폐범을 **generator**라고 하며, discriminator의 예측값이 0.5에 수렴하게 되면 학습이 종료된다.

이러한 GAN은 라벨이 필요 없는 비지도 기반 생성 모델이며, 
일반적으로 실제(real)와 구별 할 수 없는 유사한 가짜 데이터(이미지 또는 비디오 등)를 생성하는 task를 수행한다.

# GAN의 구조

위에서 설명한 GAN의 구조를 간단히 그림으로 나타내면 다음과 같다.

![](/assets/img/posts/2024-01-17-gangenerative-adversarial-networks-개념-정리/img0.png)

이를 수학적으로 나타내면 다음과 같은 수식을 풀게된다.

![](/assets/img/posts/2024-01-17-gangenerative-adversarial-networks-개념-정리/img1.png)

- `D`가 discriminator, `G`가 Generator를 의미한다.
- `x`는 실제 데이터를 의미하고, `z`는 노이즈를 의미한다.
  - `x~p_data(x)`란 실제 데이터의 확률 분포에서 샘플링한 데이터를 의미한다.
  - `z~p_z(z)`란 가우시안 분포를 따르는 노이즈에서 하나를 샘플링한 데이터를 의미한다.


- discriminator(`D`)는 `V(D, G)`를 최대화 하도록 학습한다. 
  - 진짜 데이터(`x`)일 때 증가하고(첫번째 항), 가짜 데이터(`G(z)`)일 때 감소하도록(두번째 항) 하여 진짜 데이터에 대해 높은 확률값이 output으로 나오도록 학습된다.
- generator는(`G`)는 `V(D, G)`를 최소화 하도록 학습한다.
  - 해당 식에서 `V(D, G)`가 0이 되러면 `D(G(z)) = 1`이어야 한다. 즉 generator를 통해 생성된 가짜 데이터가 Discriminator를 통과했을 때 실제 데이터라고 판별하도록 학습한다.
  - generator가 실제 데이터와 동일한 데이터를 만들도록 학습하는건 아니라는 점에 유의하자.

이렇게 discriminator는 `V(D, G)`를 최대화하는 방향으로, generator는 최소화하는 방향으로 서로 적대적으로 학습하게 되며, 이러한 학습 방법을 **minmax problem**이라고 한다.

        