

        ---
        title: 첫 오픈소스 기여 도전 (허깅페이스 diffusers)
        date: 2024-11-30 06:14:18.058 +0000
        categories: [etc]
        tags: []
        description: 2년째 해보고 싶다고 생각만 한 오픈소스 기여를 진짜로 해보자
        image: /assets/posts/2024-11-30-첫-오픈소스-기여-도전-허깅페이스-diffusers/thumbnail.png
        
        ---

        # 도전 계기

예전부터 오픈소스 기여를 해보고 싶다는 생각을 하고 있었습니다. 하지만 오픈소스 기여에 대한 막연한 두려움이 있었고, 또 어떤 기여를 할 것인지도 마땅히 정하지 못했었습니다.

그러다 최근 업무 관련해서 Stable Diffusion을 활용할 일이 많아졌고, 허깅페이스의 이미지 생성 오픈소스 라이브러리인 `diffusers`를 많이 사용하게 되었습니다.

diffusers를 기반으로 기능 구현에 필요한 커스텀 로직을 직접 개발하던 중, webui의 익스텐션인 [regional-prompter](https://github.com/hako-mikan/sd-webui-regional-prompter)를 diffusers 라이브러리에서 작동하도록 구현해야 했는데, 혹시 다른 사람이 구현한 것이 있나 찾아보던 와중 `diffusers`의 community pipeline에 대해 알게 되었습니다.
# diffusers community pipeline

[커뮤니티 파이프라인에 대한 설명 글](https://github.com/huggingface/diffusers/issues/841)

커뮤니티 파이프라인은 유저들이 직접 구현한 커스텀 파이프라인을 의미합니다. diffusers에서는 이러한 커스텀 파이프라인도  라이브러리에 포함하여 `diffusers.examples.community`에서 import하여 사용할 수 있습니다. 이러한 커스텀 파이프라인을 커뮤니티 파이프라인이라고 합니다.
#### 사용 예시
```python
from examples.community.regional_prompting_stable_diffusion import RegionalPromptingStableDiffusionPipeline

pipe = RegionalPromptingStableDiffusionPipeline.from_single_file(model_path, vae=vae)

images = pipe(
    prompt=prompt,
    negative_prompt=negative_prompt,
    guidance_scale=7.5,
    height=768,
    width=512,
    num_inference_steps=20,
    num_images_per_prompt=1,
    rp_args=rp_args
    ).images
```

[diffusers 공식 컨트리뷰션 가이드라인](https://github.com/huggingface/diffusers/blob/main/CONTRIBUTING.md)에서도 여섯번째 단계로 커뮤니티 파이프라인 기여를 추천하고 있으며, diffusers 라이브러리가 근본적으로 어떻게 동작하는지 이해할 수 있는 좋은 기회가 될 것이라고 하고 있습니다.

앞서 제가 구현하려 시도한 Regional Prompter 역시 커뮤니티 파이프라인으로 존재하고 있었고, 이를 곧바로 사용해보려고 했습니다. 그런데...
## community pipeline 수정하기
아무래도 사용자들이 자유롭게 만든 커스텀 파이프라인이다 보니 곧바로 사용하기가 어려웠습니다.

주요 이슈를 정리하면 다음과 같았습니다.

1. 마이너 에러 존재
2. 버전 호환성
3. 일부 기능 미구현

### 1. 마이너 에러 존재
자유로운 기여의 특성상 테스트 및 디버깅이 엄격히 되지 않았을 것이기 때문에 몇가지 오류가 존재했습니다.

```python
prompts = prompt if isinstance(prompt, list) else [prompt]
n_prompts = negative_prompt if isinstance(prompt, str) else [negative_prompt]
```

문제가 보이시나요?

위 코드는 프롬프트가 문자열로 들어왔을 때 리스트로 변환하여 자료형을 통일해주는 코드입니다.
그런데 `n_prompts`는 문자열이 들어오면 그대로 통과되고, 리스트로 들어오면 `[[negative prompt]]` 이렇게 만들어버리는 오류가 발생하고 있습니다.

수정은 정말 간단합니다. 그냥 `str`로 검사하는 부분을 `list`로 바꾸어주면 됩니다.

```python
prompts = prompt if isinstance(prompt, list) else [prompt]
# str -> list
n_prompts = negative_prompt if isinstance(prompt, list) else [negative_prompt]
```

정말 쉽죠? 이런 오류 정도만 찾아서 고칠 수 있어도 오픈소스 기여가 가능합니다!

이것 외에 문자열 처리에 대해 에러를 발생시키지는 않지만, 처리가 완벽히 안되는 몇가지 이슈가 있었고 진짜 간단하게 메서드 몇 개를 더 사용하거나 바꾸는 방식으로 수정이 가능했습니다.
### 2. 버전 호환성
해당 커뮤니티 파이프라인은 작년즈음에 개발된 것이었고, 그 사이에 diffuser의 버전이 업그레이드 되면서 문법이 달라진 점이 있었습니다.

대략 두 줄 정도의 코드가 문제였는데, 무턱대고 수정하기는 좀 그래서 어디서 문법이 바뀐건지, 왜 바뀌었던 건지를 찾느라 시간이 꽤 걸렸던 것 같습니다. 😅
결국 해당 부분을 수정해도 문제가 없다는 판단이 되어 수정하였습니다.

### 3. 일부 기능 미구현
![](/assets/posts/2024-11-30-첫-오픈소스-기여-도전-허깅페이스-diffusers/img0.png)

원본 regional prompter에서는 Base prompt라는 기능이 있는데요 커뮤니티 파이프라인에서는 해당 기능의 구현이 생략되어 있었습니다.

저는 해당 기능을 사용하고 싶어서 원본 코드에서 어떻게 해당 기능을 처리하는지 분석하였고, base prompt를 별도로 분리하여 따로 임베딩 한 뒤, `base_ratio`에 따라 기존 프롬프트 임베딩에 블렌딩하면 된다는 것을 알게 되었습니다.

이를 위해 기존 regional prompter와 동일한 문법으로 base prompt 기능을 사용할 수 있도록 문자열 처리를 추가하고, 관련 코드를 추가 하였습니다.

# 기여 도전하기 (PR 작성하기)

사실 이때까지만 해도 제가 사용하기 위해 코드를 수정한 것이었습니다. 그러다가 문득 '이거 그대로 기여할 수 있지 않나?'라는 생각이 들게 되었죠.

하지만 제가 수정한 부분이 너무 미미한 것이 아닌가 걱정이 되어 기존의 기여 내역을 살펴 보았습니다.

![](/assets/posts/2024-11-30-첫-오픈소스-기여-도전-허깅페이스-diffusers/img1.png)

무려 regional prompter를 처음 개발하신 분이 기여를 해주셨네요!
변경 내역은 별로 많지 않습니다. 어떤 것을 수정하신 걸까요?

![](/assets/posts/2024-11-30-첫-오픈소스-기여-도전-허깅페이스-diffusers/img2.png)

확인해보니 코드 가독성을 위해 일부 변수 이름을 변경하고, 코드를 조금 더 직관적으로 변경한 내용이었습니다.

기여라고해서 꼭 거창한게 아니라, 이렇게 간단한 리팩토링 및 단순 오류 수정과 같은 기여가 굉장히 많음을 알 수 있었습니다.
여기서 자신감을 얻어 PR을 작성하게 되었습니다.

### PR 작성하기
이런 오픈소스 라이브러리에 PR을 작성하는 것은 처음이라 긴장이 많이 되었는데요. 최대한 기존의 커뮤니티 파이프라인 PR을 많이 참조해서 작성했습니다.

![](/assets/posts/2024-11-30-첫-오픈소스-기여-도전-허깅페이스-diffusers/img3.png)

- 간단한 인사 & PR 취지 소개
- 변경점 요약
- 사용 예시

영어는 젬병이라.. DeepL 번역기와 Claude의 도움을 많이 받았습니다. 한글로 작성한 것을 DeepL로 한번 번역하고 적당히 수정한 다음, Claude에게 자연스러운 문장인지 검토를 요청하는 식으로요.

![](/assets/posts/2024-11-30-첫-오픈소스-기여-도전-허깅페이스-diffusers/img4.png)![](/assets/posts/2024-11-30-첫-오픈소스-기여-도전-허깅페이스-diffusers/img5.png)

저는 제가 코드를 엉망으로 짰을까에 대한 걱정을 더 많이 했는데요, 리뷰어 분께서 주로 요청&지적한 부분은 바로 문서화였습니다.

이건 제가 놓쳤던 부분인데, diffusers를 비롯한 많은 오픈소스 라이브러리가 문서화를 중요하게 생각하고 있다는 것을 알 수 있었습니다. 라이브러리를 처음 접하는 신규 사용자가 사용법을 알기 위해 가장 먼저 확인하는 것이 공식 문서와 examples이기 때문에 기능이 추가되면 반드시 문서화를 해야한다고 하네요.
### 문서화 (README 작성)
![](/assets/posts/2024-11-30-첫-오픈소스-기여-도전-허깅페이스-diffusers/img6.png)![](/assets/posts/2024-11-30-첫-오픈소스-기여-도전-허깅페이스-diffusers/img7.png)

PR을 작성할 때와 마찬가지로 번역기와 AI의 도움을 받아서 작성했습니다.
다행히 이미 Regional Prompting 파이프라인과 관련되어 문서화가 되어있었기 때문에 추가된 기능에 대해서만 추가하였습니다. 이런게 문서화의 중요성인가 봅니다.
### 스타일 포맷팅

여럿이서 개발할 때 또 중요한 것이 코드 컨벤션입니다.
마지막으로 받은 요청 사항이 `make style` 명령어를 이용해서 코드 스타일 포맷팅을 수행한 뒤 다시 push하라는 것이었는데요.
처음엔 사용법을 몰라서 당황했는데 `make`를 설치한 후, 라이브러리의 루트 디렉토리에서 설정값을 가져와 포맷팅을 수행할 파일 디렉토리를 지정하면 되는거였습니다.
# PR merge
![](/assets/posts/2024-11-30-첫-오픈소스-기여-도전-허깅페이스-diffusers/img8.png)

얼마전 드디어 머지가 이루어졌고, 깃허브에서 오픈소스 기여 뱃지도 받을 수 있었습니다.

댓글 알림이 올 때마다 긴장도 하고, 열심히 영어로 소통하면서 이런저런 문제를 해결해보는 귀중한 경험을 할 수 있었네요.

# 배운 점

- 오픈소스 기여에 대한 자신감을 얻을 수 있었습니다. (실력이 뛰어나지 않아도 충분히 가능!)
- 다른 사람이 작성한 코드에서 오류를 찾고 수정해보는 경험을 할 수 있었습니다.
- 다른 오픈소스 개발자와 직접 소통하는 경험을 해볼 수 있었습니다.
# 다음 기여?

diffusers는 오픈소스 기여를 적극적으로 받아들이고 있고, 또 커뮤니티 파이프라인이라는 좋은 창구가 있었기에 쉽게 입문할 수 있었던 것 같습니다.
여기서 얻은 자신감을 바탕으로 다른 라이브러리에서 직접 오류를 고치거나 기능을 추가하는 기여를 해보고자 합니다.

현재 관심이 있는것은 webui 익스텐션인데요, ControlNet 익스텐션을 사용하는 도중 버그를 발견했는데, 오랫동안 수정이 되지 않는 것 같아 직접 수정을 시도해보려 합니다.

        