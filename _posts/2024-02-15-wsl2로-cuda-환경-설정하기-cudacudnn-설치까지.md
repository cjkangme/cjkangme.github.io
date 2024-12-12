

---
title: WSL2로 CUDA 환경 설정하기 (CUDA+cuDNN 설치까지)
date: 2024-02-15 13:17:09.111 +0000
categories: [TIL]
tags: ['wsl', 'cuda', 'ubuntu']
description: WSL

WSL(Windows Subsystem for Linux)은 윈도우 환경에서 리눅스 환경을 실행할 수 있는 프로그램이다.

딥러닝 관련 오픈소스 모델을 사용하다보면 Linux Installer만 존재한다거나 각종 버전 호환성 문제가 발생할 수 있는데, 이 경우 WSL을 이용하여 Linux 환경에서 딥러닝 환경을 구축할 수 있다.

이 글을 쓰게된 ...

math: true
---

# WSL

WSL(Windows Subsystem for Linux)은 윈도우 환경에서 리눅스 환경을 실행할 수 있는 프로그램이다.

딥러닝 관련 오픈소스 모델을 사용하다보면 Linux Installer만 존재한다거나 각종 버전 호환성 문제가 발생할 수 있는데, 이 경우 WSL을 이용하여 Linux 환경에서 딥러닝 환경을 구축할 수 있다.

이 글을 쓰게된 계기도 윈도우 11 환경에서 어려움을 겪다가 WSL로 딥러닝 환경을 구축하기 위해 시작하였다.

### 장단점

WSL의 장점은 윈도우와 Linux를 동시에 사용할 수 있다는 것이고
VM Ware와 같은 가상 머신 대비 훨씬 컴퓨팅 비용을 덜 쓴다는 것이다.

개인적으로 느낀 WSL의 단점은 윈도우 기반에서 실행되다보니 `systemctl`과 같은 명령어를 사용할 수 없어서 `service`와 같은 대체 명령어를 사용해야 한다. 이런 부분들이 실제 리눅스와는 다르기 때문에 인터넷에서 검색한 리눅스 관련 문제 해결법이 잘 적용되지 않을 때가 있었다.
또 아직 내가 사용한 적은 없지만 SSH 사용성이 다소 떨어진다고 한다.
## WSL2 설치

[공식 설치 가이드](https://learn.microsoft.com/ko-kr/windows/wsl/install)

CUDA를 원활하게 사용하기 위해서는 WSL2 버전을 설치해야 하는데,
Window 10, 11의 특정 빌드 이상을 요구하므로 되도록 **윈도우를 최신버전으로 업데이트**하고 설치하는 것이 좋다.

### 1. WSL 사용 설정 켜기

![img](/assets/posts/2024-02-15-wsl2로-cuda-환경-설정하기-cudacudnn-설치까지/img0.png)

검색창 → Windows 기능 켜기/끄기에서,

- Hyper-V
- Linux용 Windows 하위 시스템

위 두가지 기능을 활성화 한다.

### 2. WSL 설치

PowerShell을 관리자로 실행하여 다음과 같은 명령어를 입력한다.

```
wsl --install
```

웬만하면 WSL2로 Ubuntu 22.04 버전이 설치가 된다. (글 작성일 기준)
버전을 확인하거나, 특정 버전을 설치하고 싶은 경우 앞서 링크한 [공식 설치 가이드](https://learn.microsoft.com/ko-kr/windows/wsl/install)를 참고하여 진행하자

설치 과정에서 운영체제의 사용자 이름 및 비밀번호를 설정하게 되고
설치가 완료되면 바로 Ubuntu가 부팅되는데, PowerShell에서 하는 것 보다는 WSL 터미널 또는 Ubuntu 터미널에서 실행하는 것이 훨씬 가독성이 좋다. 
터미널이 없으면 Microsoft Store에서 설치가 가능하다.

# Ubuntu

## 초기 설정

Ubuntu를 실행하면 사용자 요구에 따라서 이것저것 설치를 먼저 진행하게 된다.
여기서는 CUDA 설정을 위해 필수적인 것들만 설치하고 넘어가도록 하자

### nvidia driver 확인

CUDA를 설치 및 설정하기 위해서는 반드시 nvidia driver가 있어야 한다.
여기서 조심해야 할 것이 WSL 환경에서는 윈도우에 설치된 nvidia driver를 사용하므로 **Ubuntu 내부에서 nvidia driver를 설치하면 안된다.**  
- [CUDA 공식문서](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)에 'users must not install any NVIDIA GPU Linux driver within WSL 2'이라고 강조되어 있다.

실제로 아무 드라이버도 설치하지 않아도 윈도우에 nvidia driver가 설치되어있다면 다음 명령어로 조회할 수 있다.

```shell
$ nvidia-smi

Thu Feb 15 20:30:56 2024
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 545.23.07              Driver Version: 546.12       CUDA Version: 12.3     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA GeForce RTX 4060 Ti     On  | 00000000:01:00.0  On |                  N/A |
|  0%   51C    P8              11W / 160W |   1059MiB /  8188MiB |      8%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|    0   N/A  N/A        29      G   /Xwayland                                 N/A      |
+---------------------------------------------------------------------------------------+
```

### build-essential 설치

build-essential 패키지는 개발에 필요한 기본 라이브러리와 헤더파일 등을 갖고 있는 패키지이다.

각종 실행파일 실행에 필요한 `cmake`, `gcc`, `g++` 등의 라이브러리를 제공한다.

다음 명령어를 순서대로 실행해서 설치할 수 있다.

```shell
$ sudo apt update
$ sudo apt install build-essential
```

이 정도만 확인했다면 이제 CUDA와 cuDNN을 설정할 준비가 끝났다.

## CUDA Toolkit 다운로드 및 설치

[CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive)에서 원하는 버전의 CUDA Toolkit을 다운로드 할 수 있다.

각 오픈소스 별로 사용하는 python의 버전이 다르기 때문에 내 경우는 
- python 3.7 ~ 3.10 + pyTorch 1.12.1에 대응하는 CUDA 11.6 버전
- python 3.8 ~ 3.11 + pyTorch 2.1.2(최신버전)에 대응하는 CUDA 12.1 
이 두가지 버전을 설치하려고 한다.

먼저 처음으로 설치할 CUDA Toolkit은 설치 가이드를 그대로 따르면 된다.

![img](/assets/posts/2024-02-15-wsl2로-cuda-환경-설정하기-cudacudnn-설치까지/img1.png)

```shell
$ wget https://developer.download.nvidia.com/compute/cuda/12.1.0/local_installers/cuda_12.1.0_530.30.02_linux.run
$ sudo sh cuda_12.1.0_530.30.02_linux.run
```

설치가 끝나면 다음 명령어로 CUDA가 정상적으로 설치가 끝났는지 확인할 수 있다.

```shell
$ ls /usr/local/ | grep cuda
cuda
cuda-12.1
```

두번째 설치때는 `wget`으로 다운로드까지만 설치 가이드를 따라한다.

```shell
$ wget https://developer.download.nvidia.com/compute/cuda/11.6.2/local_installers/cuda_11.6.2_510.47.03_linux.run
```

이제는 CUDA 전체를 설치하는 것이 아니라 필요한 버전의 toolkit만 필요한 것이기 때문에 `sh`로 쉘 스크립트를 바로 호출하는 대신, toolkit만 설치하도록 옵션을 주어 직접 실행한다.

```shell
$ chmod +x cuda_11.6.2_510.47.03_linux.run
$ sudo ./cuda_11.6.2_510.47.03_linux.run --silent --toolkit

$ ls /usr/local/ | grep cuda
cuda
cuda-11.6
cuda-12.1
```

## cuDNN 다운로드 및 설정

다음은 CUDA 버전과 호환되는 cuDNN을 다운로드하고, 직접 CUDA 폴더에 넣어주는 설정 과정이 필요하다.

### 다운로드

[cuDNN 다운로드 페이지](https://developer.nvidia.com/cudnn-downloads)에서 먼저 다운로드를 받을 수 있다.
CUDA 11버전, 12버전과 호환되는 cuDNN이 각각 다르므로 모두 다운받아주어야 한다. ~~12.1 말고 11.8 받을걸...~~

![img](/assets/posts/2024-02-15-wsl2로-cuda-환경-설정하기-cudacudnn-설치까지/img2.png)

이 때 Ubuntu의 deb 파일 대신 tar.xz 형태의 Tarball 파일을 받도록 하자

```shell
$ wget https://developer.download.nvidia.com/compute/cudnn/redist/cudnn/linux-x86_64/cudnn-linux-x86_64-9.0.0.312_cuda11-archive.tar.xz
$ wget https://developer.download.nvidia.com/compute/cudnn/redist/cudnn/linux-x86_64/cudnn-linux-x86_64-9.0.0.312_cuda12-archive.tar.xz
```

### 압축 풀기

이제 다운로드 받은 tar.xz 압축파일의 압축을 해제해주자

```shell
tar -xvf cudnn-linux-x86_64-9.0.0.312_cuda11-archive.tar.xz
tar -xvf cudnn-linux-x86_64-9.0.0.312_cuda12-archive.tar.xz
```

만약 압축을 푸는 과정을 보고 싶지 않다면 `-v` 옵션을 빼고 `-xf` 옵션으로 실행하면 된다.
`-x`는 압축을 푸는 옵션, `-f`는 파일 이름을 지정하는 옵션이다.

### 올바른 CUDA 폴더에 넣기

이제 cuDNN 파일을 각각 CUDA 폴더 안에 올바르게 넣어주어야 한다.
`cp` 명령어를 사용해서 파일을 복사해주자

```shell
$ cd cudnn-linux-x86_64-9.0.0.312_cuda11-archive
$ ls
LICENSE  include  lib
$ sudo cp include/cudnn*.h /usr/local/cuda-11.6/incl
ude
$ sudo cp lib/libcudnn* /usr/local/cuda-11.6/lib64

$ cd ../cudnn-linux-x86_64-9.0.0.312_cuda12-archive
$ ls
LICENSE  include  lib
$ sudo cp include/cudnn*.h /usr/local/cuda-12.1/include
$ sudo cp lib/libcudnn* /usr/local/cuda-12.1/lib64
```

폴더 이름이 `lib` 또는 `lib64`인 경우가 있어서 `ls` 명령어로 리스트를 확인하였다.

### PATH 설정하기

CUDA Toolkit 설정이 완료되었다면 `nvcc --version` 명령어로 현재 설정된 CUDA 버전을 확인할 수 있다.

```shell
$ nvcc -v
Command 'nvcc' not found, but can be installed with:
sudo apt install nvidia-cuda-toolkit
```

아마 위처럼 에러가 리턴될텐데 이는 PATH 설정이 이루어지지 않았기 때문이다.

그런데 CUDA 버전이 여러개일 때, 다른 CUDA 버전을 사용하려면 매번 PATH 설정을 새롭게 변경해주어야 한다는 번거로움이 있다.

이를 해결하기 위해 명령어 한번으로 CUDA 버전을 변경하고, 변경 결과를 확인할 수 있는 함수를 만드는 것이 훨씬 편할 것이다.

`~/.bashrc`를 편집하여 해당 함수를 추가하자

```shell
$ vi ~/.bashrc
```
```shell
... 기존 .bashrc 내용들

function _switch_cuda {
   v=$1
   export PATH=$$ PATH:/usr/local/cuda- $$v/bin
   export CUDADIR=/usr/local/cuda-$v
   export LD_LIBRARY_PATH=$$ LD_LIBRARY_PATH:/usr/local/cuda- $$v/lib64
   nvcc --version
}
```

`:wq`로 저장한 다음 변경사항을 적용하면 함수를 호출할 수 있다.

```shell
$ source ~/.bashrc
$ _switch_cuda 12.1
```
```
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on Tue_Feb__7_19:32:13_PST_2023
Cuda compilation tools, release 12.1, V12.1.66
Build cuda_12.1.r12.1/compiler.32415258_0
```

함수에 들어있는 `nvcc --version`이 잘 실행된 것을 확인할 수 있다.

+) 위와는 별개로 WSL을 켰을 때 기본으로 설정될 CUDA 버전은 별도로 지정해주어야 한다.

```bash
# ~/.bashrc
export PATH=$PATH:/usr/local/cuda-{버전}/bin
export CUDADIR=/usr/local/cuda-{버전}
```

위와 같이 `~/.bashrc` 파일에 기본으로 CUDA 디렉토리를 설정하는 명령어를 넣어주자
적용 후에는 WSL를 껐다 다시 키거나 `source ~/.bashrc` 명령어로 리부트 할 수 있다.

이제 딥러닝 라이브러리 실행에 필요한 python, pytorch 등등을 설치해주어야 하는데
아마 conda를 이용하여 설치하는 것이 편할 것 같다.

만약 과정이 복잡하다면 이것도 따로 포스팅하자

# 참고자료

- [Managing Multiple CUDA Versions on a Single Machine: A Comprehensive Guide](https://towardsdatascience.com/managing-multiple-cuda-versions-on-a-single-machine-a-comprehensive-guide-97db1b22acdc)
- [Installing multiple CUDA + cuDNN versions in the same machine for Tensorflow and Pytorch](https://towardsdatascience.com/managing-multiple-cuda-versions-on-a-single-machine-a-comprehensive-guide-97db1b22acdc)
- [garg-aayush/Steps_multiple_cuda_environments.md](https://gist.github.com/garg-aayush/156ec6ddda3d62e2c0ddad00b7e66956)

        