

---
title: Stable Diffusion web UI API로 사용하기
date: 2024-10-18 08:35:40.166 +0000
categories: [TIL]
tags: []
description: 알아두면 유용할 것 같은 GUI 없이 webui 사용하기
image: /assets/posts/2024-10-18-stable-diffusion-web-ui-api로-사용하기/thumbnail.png
math: true
---

![](/assets/posts/2024-10-18-stable-diffusion-web-ui-api로-사용하기/img0.png) <small>우리가 흔히 보는 SD web UI의 모습</small>

[Stable Diffusion web UI](stable-diffusion-webui)(이하 webui)는 git이나 CLI에 조금만 친숙한 사람이면 복잡한 코드 없이 여러 기법이 적용된 Stable Diffusion 모델을 사용할 수 있는 라이브러리입니다.

사용하기 쉬운 GUI에 자동화 기능까지 제공하기 때문에 원하는 이미지를 빠르게 뽑기 위해 더할 나위 없이 좋은 방법이죠.

하지만 webui로 생성한 이미지를 이용해서, 특정 작업을 수행하는 파이프라인을 구축하기는 어렵습니다. GUI는 유저의 수동 입력을 전제로하기 때문에 자동화하 작업에는 적합하지 못하기 때문입니다.

이러한 이유로 webui는 GUI 없이 API를 이용해서 원하는 설정, 모델, 프롬프트로 이미지를 생성하는 기능을 제공하고 있습니다.

하지만 webui API를 쓰는 사람이 많이 없어서 그런지, API 문서가 많이 불친절했고 생각보다 정보를 찾기 어려웠습니다. 
앞으로 API를 사용할 때 해메지 않기 위해 한 번 정리를 해두려고 합니다.

# 기본 사용법
webui를 API로 사용하기 위해서는 실행시 `api` 옵션을 주어야 합니다.
```bash
bash webui.sh --api
```

webui API는 FastAPI를 기반으로 동작하기 때문에 `127.0.0.1:7860/docs` 주소에서 FastAPI에서 기본으로 만들어준 명세를 참조할 수 있습니다. (포트 번호 7860은 기본값, 유저 설정에 따라 바뀔 수 있음)

![](/assets/posts/2024-10-18-stable-diffusion-web-ui-api로-사용하기/img1.png)

기본적으로는 이렇게 docs를 참조해서 원하는 API를 찾아 사용하시면 됩니다.
예를 들어 image-to-image 모델 사용하고 싶다면 `sdapi/v1/img2img`에 POST 요청을 보내어 이미지 응답을 받을 수 있습니다.

그런데 위 사진에서 볼 수 있듯이, config로 요구하는 값들이 어마어마하게 많습니다.
각 config가 무엇인지 별도로 설명되어있는 곳도 없기 때문에, 하나하나 찾아가면서 설정하기도 어렵습니다.
다행히 어떤 친절한 분께서 config를 쉽게 구할 수 있는 익스텐션을 개발해 주셨습니다.

> [huchenlei/sd-webui-api-payload-display](https://github.com/huchenlei/sd-webui-api-payload-display)

해당 익스텐션을 설치하고 GUI에서 원하는 설정을 적용한 뒤 실행하면, 어떤 payload를 사용해야하는지 JSON 형태로 출력해줍니다.

![](/assets/posts/2024-10-18-stable-diffusion-web-ui-api로-사용하기/img2.png)![](/assets/posts/2024-10-18-stable-diffusion-web-ui-api로-사용하기/img3.png)

Copy 버튼을 눌러 편하게 복사해서 사용하시면 됩니다.
`base64image placeholder` 부분이 이미지 데이터가 들어갈 자리로, 이미지를 base64로 인코딩해서 넣으면 됩니다.

예를 들어 다음과 같이 구현할 수 있습니다.
```python
url = "http://127.0.0.1:7860/sdapi/v1/img2img"
payload = { ... }  # payload config

def img2img(img: np.array, prompt: str):
    image_pil = Image.fromarray(image)
    bytes_io = io.BytesIO()
    image_pil.save(bytes_io, format="PNG")
    image_base64 = base64.b64encode(bytes_io.getvalue()).decode("utf-8")
    
    payload["init_images"] = [base64_image]
    payload["prompt"] = prompt
    response = requests.post(url, json=payload)
```

응답 역시 base64로 인코딩된 이미지이므로, 디코딩하여 사용하시면 됩니다.

# 익스텐션 추가하기
익스텐션도 기본적으로는 GUI로 사용해 보고, `sd-webui-api-payload-display`의 출력 결과를 확인해서 어떻게 payload를 설정해야하는지 확인할 수 있습니다.

일부 익스텐션은 README에 API 사용법을 설명해 주는 경우도 있으니 확인해보시면 좋습니다.
예를 들어 [Mikubill/sd-webui-controlnet](https://github.com/Mikubill/sd-webui-controlnet) 익스텐션의 경우 다음과 같이 예제를 제공해 주네요.

```python
def build_body(self):
        self.body = {
            "prompt": self.prompt,
            "negative_prompt": "",
            "batch_size": 1,
            "steps": 20,
            "cfg_scale": 7,
            "alwayson_scripts": {
                "controlnet": {
                    "args": [
                        {
                            "enabled": True,
                            "module": "none",
                            "model": "canny",
                            "weight": 1.0,
                            "image": self.read_image(),
                            "resize_mode": "Crop and Resize",
                            "lowvram": False,
                            "processor_res": 64,
                            "threshold_a": 64,
                            "threshold_b": 64,
                            "guidance_start": 0.0,
                            "guidance_end": 1.0,
                            "control_mode": "Balanced",
                            "pixel_perfect": False
                        }
                    ]
                }
            }
        }
```

익스텐션이 API 가이드를 제공하지 않는 경우에도 issue 탭에서 누군가 질문한 것을 참고할 수 있습니다.
정 없다면, 눈치껏 파악해서 적용해야 합니다... 일반적으로 GUI에서 설정 가능한 옵션과 1:1 대응하기 때문에 이를 바탕으로 대략적으로 파악할 수 있습니다.

# GUI 없이 webui 세팅하기
만약 CLI 환경이라 GUI를 사용할 수 없는 경우에는 직접 파일을 넣어 webui에 필요한 모델/익스텐션을 설치하거나 API를 이용하여 설정을 변경할 수 있습니다.
## 모델 설치
webui에 원하는 모델을 넣기 위해서는 별도의 설치과정 없이 정해진 디렉토리에 모델 파일을 올리면 됩니다.

`.../stable-diffusion-webui/models`

![](/assets/posts/2024-10-18-stable-diffusion-web-ui-api로-사용하기/img4.png)

SD 모델은 `Stable-diffusion` 디렉토리에, ControlNet 모델은 `ControlNet` 디렉토리에 넣는 등, 정해진 위치에 넣으시면 됩니다.

## 익스텐션 설치
익스텐션 역시 모델과 동일하게 정해진 디렉토리에 익스텐션 파일을 넣으면 됩니다.

`.../stable-diffusion-webui/extensions`

위 경로에서 원하는 익스텐션의 레포지토리를 clone 하시면 익스텐션 설치가 완료 됩니다.
## Settings 변경
마지막으로 설정을 변경하기 위해 `/sdapi/v1/options` API를 사용할 수 있습니다.

![](/assets/posts/2024-10-18-stable-diffusion-web-ui-api로-사용하기/img5.png)

먼저 GET 요청으로 옵션 목록을 json으로 가져온 다음
json에서 원하는 옵션을 변경하거나 추가하여 POST 요청으로 다시 전송하면 됩니다.

```python
# 예시
async def initialize():
    getUrl = "/sdapi/v1/options"
    response = requests.get(url)
    
    opt = response.json()
    opt["sd_model_checkpoint"] = "dreamshaper_8.safetensors"
    opt["control_net_model_cache_size"] = 3
    
    postUrl = "/sdapi/v1/options"
    requests.post(url, json=opt)
```
위 예시는 webui가 사용할 SD 모델을 dreamshaper_8로 설정하고, ControlNet의 cache를 3으로 설정하는 코드입니다.

### 어떤 옵션을 변경해야하는지 확인하는 방법
GET으로 가져온 json에는 익스텐션 관련 설정이 포함되어있지 않기 때문에, 어떤 설정을 바꿔야하는지 모르는 경우가 생길 수 있습니다. (control_net_model_cache_size 등)

이 경우 `config.json` 파일을 이용하시면 됩니다.
webui를 한번이라도 실행했다면, 루트 폴더에 `config.json` 파일이 생성되어 있을 겁니다.

![](/assets/posts/2024-10-18-stable-diffusion-web-ui-api로-사용하기/img6.png)

해당 파일에 webui의 설정값들이 나열되어 있으므로, 변경 해야 할 key를 찾아서 json에 추가하시면 됩니다.

# webui 배포하기
webui를 실행하는 도커 이미지들이 이미 많이 존재하는데 저는 커스텀을 쉽게 하기 위해 따로 `Dockerfile`과 `compose.yaml`을 작성했습니다.

### Dockerfile 예시
```
# Reference:
# https://github.com/cvpaperchallenge/Ascender
# https://github.com/nerfstudio-project/nerfstudio

FROM pytorch/pytorch:2.4.1-cuda12.4-cudnn9-devel

ARG USER_NAME=cjkang

# apt install by root user
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    build-essential \
    curl \
    git \
    libegl1-mesa-dev \
    libgl1-mesa-dev \
    libgles2-mesa-dev \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1 \
    python-is-python3 \
    python3.10-dev \
    python3-venv \
    python3-pip \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Change user to non-root user
RUN adduser --disabled-password --gecos "" ${USER_NAME}
RUN mkdir -p /home/$$ {USER_NAME}/stable-diffusion-webui && chown -R  $${USER_NAME}:$$ {USER_NAME} /home/ $${USER_NAME}
USER ${USER_NAME}
WORKDIR /home/${USER_NAME}/stable-diffusion-webui

RUN git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui .
RUN git clone https://github.com/hako-mikan/sd-webui-regional-prompter extensions/sd-webui-regional-prompter
RUN git clone https://github.com/Mikubill/sd-webui-controlnet extensions/sd-webui-controlnet
```
webui를 실행하기 위해 필요한 환경을 갖춘 다음, webui를 공식 저장소에서 클론하고 나머지 익스텐션을 차례대로 clone하는 설정 파일입니다.

### compose.yaml 예시
```yaml
services:
  webui:
    build:
      context: .
      dockerfile: ./docker/Dockerfile
    ports:
      - "[PORT]:[PORT]"
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              device_ids: ["2"]
              capabilities: [gpu]
    volumes:
      - type: volume
        source: [VOLUME_NAME or PATH]
        target: /models
    command: sh -c "
    cp -r /models/stable-diffusion-webui/* ~/stable-diffusion-webui/models/ &&
    bash webui.sh --api --xformers --listen --no-download-sd-model"
```
GPU를 사용하기 위해 gpu 관련 설정을 해주고, 포트도 연결해주어 외부에서 접근 가능하도록 하였습니다.

그리고 명령어 부분은 volume을 우선 루트 경로에 마운트 한 뒤에 `cp -r` 명령어를 이용해서 정해진 경로에 넣고, `webui.sh`를 실행합니다.

`--xformers`는 성능이 약간 감소하는 대신 훨씬 빠르고 가벼운 attention을 사용하는 옵션이고
`--listen`은 호스트를 0.0.0.0으로 설정해 외부에서 접근 가능하도록 하는 옵션입니다.
`--no-download-sd-model`은 webui 실행 시에 기본 모델 다운로드를 막는 옵션입니다.

이렇게 docker로 webui를 띄우고 API를 이용하면 GPU 자원이 없어도 잠깐 빌려서 webui를 띄워 사용할 수 있습니다.
아니면 서버에 띄워놓고 API로 이미지를 받아서 downstream task에 활용할 수도 있겠네요.

        