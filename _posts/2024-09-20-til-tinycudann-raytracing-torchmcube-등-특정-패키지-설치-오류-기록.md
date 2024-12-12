

---
title: [TIL] tinycudann, raytracing, torchmcube 등 특정 패키지 설치 오류 기록
date: 2024-09-20 05:44:59.149 +0000
categories: [TIL]
tags: []
description: 패키지 설치부터 어려워..
image: /assets/posts/2024-09-20-til-tinycudann-raytracing-torchmcube-등-특정-패키지-설치-오류-기록/thumbnail.png

---

![](/assets/posts/2024-09-20-til-tinycudann-raytracing-torchmcube-등-특정-패키지-설치-오류-기록/img0.png)

오픈소스 코드를 쓰려고 하다보면 이렇게 clone을 해서 패키지를 설치해야 하는 경우가 많은데
생각없이 클론 후 `pip install`을 하면 바로 빌드 과정에서 오류가 나는 경우가 많다.

이 경우 가장 좋은 것은 container로 아예 환경을 새로 구축하는 것이다.

tinycudann, raytracing 과 같은 패키지들은 빌드 파일을 기반으로 컴파일 하기 때문에 `conda` 정도의 가상환경으로는 제대로 격리가 안된다.
즉 기존 환경과 충돌할 위험이 매우 높다.


그러니 되도록 tinycudann 등을 요구하는 상위 레포지토리 요구사항 대로 새롭게 container를 구축해서 설치하자.
레포지토리에서 Dockerfile이나 이미지를 제공한다면 그대로 사용하면 된다.


만약 상위 라이브러리에 환경이 제대로 명시가 안되어있다면 tinycudann을 제공하는 공식 레포지토리의 추천 환경을 사용하자

```
gcc -pthread -B /opt/conda/compiler_compat -Wno-unused-result -Wsign-compare -DNDEBUG -fwrapv -O2 -Wall -fPIC -O2 -isystem /opt/conda/include -fPIC -O2 -isystem /opt/conda/include -fPIC -I/tmp/pip-req-build-mmwfaaw2/include -Ieigen-3.3.7 -I/opt/conda/lib/python3.10/site-packages/torch/include -I/opt/conda/lib/python3.10/site-packages/torch/include/torch/csrc/api/include -I/opt/conda/lib/python3.10/site-packages/torch/include/TH -I/opt/conda/lib/python3.10/site-packages/torch/include/THC -I/usr/local/cuda/include -I/opt/conda/include/python3.10 -c /tmp/pip-req-build-mmwfaaw2/src/bindings.cpp -o build/temp.linux-x86_64-cpython-310/tmp/pip-req-build-mmwfaaw2/src/bindings.o -O3 -std=c++14 -DTORCH_API_INCLUDE_EXTENSION_H -DPYBIND11_COMPILER_TYPE=\"_gcc\" -DPYBIND11_STDLIB=\"_libstdcpp\" -DPYBIND11_BUILD_ABI=\"_cxxabi1011\" -DTORCH_EXTENSION_NAME=_raytracing -D_GLIBCXX_USE_CXX11_ABI=0
      In file included from /tmp/pip-req-build-mmwfaaw2/src/bindings.cpp:3:
      /opt/conda/lib/python3.10/site-packages/torch/include/pybind11/eigen.h:12:10: fatal error: eigen/matrix.h: No such file or directory
         12 | #include "eigen/matrix.h"
            |          ^~~~~~~~~~~~~~~~
      compilation terminated.
      error: command '/usr/bin/gcc' failed with exit code 1
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for raytracing
  Running setup.py clean for raytracing
  Building wheel for clip (setup.py) ... done
  Created wheel for clip: filename=clip-1.0-py3-none-any.whl size=1369490 sha256=d05d3fbf7a6348dccb720b4a7e573709b1db1d0b17036f332e43c72a2d4a7d3a
  Stored in directory: /tmp/pip-ephem-wheel-cache-qxff1uk4/wheels/da/2b/4c/d6691fa9597aac8bb85d2ac13b112deb897d5b50f5ad9a37e4
  Building wheel for pycollada (setup.py) ... done
  Created wheel for pycollada: filename=pycollada-0.8-py3-none-any.whl size=127517 sha256=d3a1b1cca208e1e528da4033cb1100c2c09dd395e697ffdb8bf158465781e747
  Stored in directory: /root/.cache/pip/wheels/11/92/79/6e8add42e1e207a97d435169cdc705e0a5dd6fb182f4368f3d
Failed to build raytracing
ERROR: Could not build wheels for raytracing, which is required to install pyproject.toml-based projects
```

새롭게 컨테이너를 파도 이렇게 오류가 출력되는 경우가 있는데,
이를 해결하기 위해서는 일차적으로 해당 레포지토리의 issue를 뒤져보는 것이 빠르다.

`eigen/matrix.h: No such file or directory`

이렇게 오류 메세지에서 키워드를 찾아서 issue에 검색해보고, 없다면 그 다음으로 구글에 검색해보는 것이 좋다.

![](/assets/posts/2024-09-20-til-tinycudann-raytracing-torchmcube-등-특정-패키지-설치-오류-기록/img1.png)

이번 경우에는 비슷한 문제를 겪는 사람들이 많았고, issue에서 해결책을 찾을 수 있었다.

# 코멘트

매번 똑같은 오류를 겪으면서 매번 해결책을 다시 찾다보니 그냥 아예 기록하게 되었다.
버전을 처음부터 빡세게 관리한다면 이런 문제가 없겠지만 아직 gcc 컴파일과 같은 부분에서는
그냥 컨테이너를 새로 파는게 제일 편하다는 것만 기억하자..

        