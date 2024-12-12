

---
title: [Flutter-Android] Enabling multidex support
date: 2023-05-26 09:03:22.448 +0000
categories: [error]
tags: []
description: 정말 간단한 오류인데...
image: /assets/posts/2023-05-26-flutter-android-enabling-multidex-support/thumbnail.png

---

# 오류 발생
![img](/assets/posts/2023-05-26-flutter-android-enabling-multidex-support/img0.png)

플러터로 Api 코드를 작성하다보니 다음과 같은 오류가 발생하였다.

이는 앱에서 다양한 라이브러리를 사용하면서, 사용하는 메서드가 64K(65,536개)보다 많아지면 Android 빌드 아키텍처의 한도에 도달하기 때문에 출력되는 오류이다.

이때 Multidex를 사용하면 메서드를 담은 dex 파일을 여러개로 쪼개어 불러오는 것으로 문제를 해결할 수 있다.

# 해결 방법

https://docs.flutter.dev/deployment/android#enabling-multidex-support

`minSdkVersion`이 21 이상인 경우는 multidex를 기본적으로 사용하므로 `multiDexEnabled true`를 지정하지 않아도 된다.

`minSdkVersion`이 20 이하인 경우에는 multidex를 사용할 것임을 명시해주어야 한다.

build.gradle 폴더에서 아래와 같은 내용을 추가해주면 된다.


```
# build.gradle

defaultConfig {
        ... (중략) ...
        multiDexEnabled true
    }

... (중략) ...

dependencies {
    ...
    # androidx를 사용하는 경우
    implementation 'androidx.multidex:multidex:2.0.1'
    # androidx를 사용하지 않는 경우
    implementation 'com.android.support:multidex:1.0.3'
}
```

### androidx

androidx를 사용하는지 아닌지는 `ROOT/android/gradle.properties`에서 `android.useAndroidX`이 true인지 false인지 여부로 확인할 수 있다.

implementation이 맞지 않는 경우 여전히 multidex를 사용하는 것으로 인식하지 못하는 경우가 많으니 꼭 주의해서 확인하자.

        