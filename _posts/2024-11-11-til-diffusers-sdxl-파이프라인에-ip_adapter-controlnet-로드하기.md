

---
title: [TIL] diffusers SDXL 파이프라인에 ip_adapter, controlnet 로드하기
date: 2024-11-11 07:11:52.260 +0000
categories: [TIL]
tags: ['til']
description: diffusers 라이브러리를 사용할 때 대부분의 기본값이 sd1.5를 기준으로 적용되어 있기 때문에 sdxl로 포팅할 때 여러가지 오류가 발생하는 경우가 많습니다.

controlnet고 ip_adapter 로드 역시 이러한 경우에 속하기 때문에 오류 없이 로드하는 


---

diffusers 라이브러리를 사용할 때 대부분의 기본값이 sd1.5를 기준으로 적용되어 있기 때문에 sdxl로 포팅할 때 여러가지 오류가 발생하는 경우가 많습니다.

controlnet고 ip_adapter 로드 역시 이러한 경우에 속하기 때문에 오류 없이 로드하는 방법을 정리하고자 합니다.

# ControlNet

![img](/assets/posts/2024-11-11-til-diffusers-sdxl-파이프라인에-ip_adapter-controlnet-로드하기/img0.png)

일반적으로 SDXL controlnet은 이렇게 safetensors 단일 파일만 존재하는 경우가 많습니다.
AUTOMATIC1111 또는 ComfyUI에서는 단일 파일로 곧바로 사용할 수 있지만, diffusers에서 사용하려면 모델의 구조에 대한 정보를 담고 있는 `config.json`이 필요합니다.

즉 위와 같이 `config.json`이 없는 저장소의 경우 `from_pretrained()`에 저장소 id만 전달하면 오류가 발생할 가능성이 높습니다. (오류 메세지 정황상 sd 1.5의 설정으로 불러오는 듯 합니다)
이 경우, diffusers가 로드할 수 있도록 별도로 형식을 맞춰주어야 합니다.

```
model_name
-  config.json
-  diffusion_pytorch_model.safetensors (or diffusion_pytorch_model.bin)
```

이런 형식으로 파일을 구성하면 된다.

### config.json 구성
![img](/assets/posts/2024-11-11-til-diffusers-sdxl-파이프라인에-ip_adapter-controlnet-로드하기/img1.png)

ControlNet은 original unet의 인코더 블록을 복제하여 사용하는데, 블록의 일부만 복제하여 효율성을 높인 버전도 있습니다.

controlnet의 종류가 다르더라도 아키텍처는 모두 동일하기 때문에 본인이 사용할 모델과 일치하는 config.json 파일을 복사하여 사용하면 됩니다.
[diffuser 공식 저장소](https://huggingface.co/collections/diffusers/sdxl-controlnets-64f9c35846f3f06f5abe351f)에 각각 small, mid, full 버전의 `config.json`을 포함한 저장소를 제공하니 여기서 가져다 쓰시면 됩니다.

이렇게 파일 구성을 완료했다면 다음과 같이 ControlNet을 로드할 수 있습니다.
```python
# ControlNet load 예시
from diffusers import ControlNetModel

controlnet = ControlNetModel.from_pretrained(
	"models/diffusers_xl_canny_full", torch_dtype=self.dtype
).to(self.device)
```

# IP-Adapter
IP-Adapter도 ControlNet과 원리는 동일하지만, CLIP보다 다소 복잡해집니다.

우선 IP-Adapter를 사용하기 위해서는 224x224 이미지에서 이미지 임베딩을 추출하는 `image_encoder`가 필요합니다. sd1.5의 경우는 CLIP 텍스트 임베딩과 동일한 768차원의 임베딩을 추출하지만, sdxl의 경우 1280차원 또는 1664차원의 임베딩을 사용하기 때문에 그대로 로드할 경우 어텐션 연산 중 에러가 발생하게 됩니다.

때문에 `image_encoder`를 별도로 먼저 로드한 후, 파이프라인을 로드할 때 기본값 대신 로드한 `image_encoder`를 사용하도록 지정해주어야 합니다.

```python
# load image_encoder
self.image_encoder = CLIPVisionModelWithProjection.from_pretrained(
	"h94/IP-Adapter", subfolder="sdxl_models/image_encoder"
)

# Create model
pipe = StableDiffusionXLPipeline.from_single_file(
	model_key,
    image_encoder=self.image_encoder,
    torch_dtype=self.dtype,
)

# load ip-adapter
pipe.load_ip_adapter(
	"h94/IP-Adapter",
    subfolder="sdxl_models",
    weight_name="ip-adapter_sdxl_vit-h.safetensors",
)
```

이제 ip_adapter가 적용된 파이프라인을 로드할 수 있습니다.

# 참고자료
[issue: IP_Adapters shape mismatch when generating images on v0.25.0_dev using SDXL?](https://github.com/huggingface/diffusers/issues/6162)
[diffusers controlnet+sdxl  공식 파이프라인](https://github.com/huggingface/diffusers/blob/main/src/diffusers/pipelines/controlnet/pipeline_controlnet_sd_xl.py)
[diffusers 공식 ip_adapter 테스트 코드](https://github.dev/huggingface/diffusers/blob/main/src/diffusers/pipelines/stable_diffusion_xl/pipeline_stable_diffusion_xl.py)

        