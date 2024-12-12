

        ---
        title: [Flutter-Android] 백그라운드에서 위치 정보 받아오기 + Kakao REST API 
        date: 2023-05-22 10:34:12.348 +0000
        categories: [K-DT해커톤]
        tags: ['aivle', 'flutter', '공모전/경진대회']
        description: 사용자 위치정보를 백그라운드에서 받아와 사용해보자
        image: /assets/img/posts/2023-05-22-flutter-android-백그라운드에서-위치-정보-받아오기-kakao-rest-api/thumbnail.png
        math: true
        ---

        > KT 에이블스쿨 수강생들과 함께 참여한 공모전을 위해 개발한 내용에 대해 작성한 게시글입니다.

# 백그라운드에서 위치 정보 수집

## 권한

### 수집 허용 관련
우선 유저가 위치정보를 수집하는 것을 허용해야 한다.
특히 백그라운드에서 위치 정보를 수집하기 위해서는 사용자가 직접 권한 설정으로 이동하여 '항상 허용'을 선택해야 한다.

또한 **매니패스트 파일**에 해당 권한을 사용할 것임을 명시해야 한다.
> #### 매니패스트 파일
> 앱 프로젝트와 관련한 모든 정보를 Android 빌드 도구, Android 운영체제, Google Play Store 등에 설명해주는 파일이다.
> 
> 쉽게 말하면 앱과 관련한 메타데이터가 저장되어있는 파일이며, 다음과 같은 정보들로 구성된다.
> - 앱의 패키지 이름
> - 앱의 구성 요소 (컴포넌트)
> - 앱 구동에 필요한 권한
> - 기기 호환성

백그라운드에서 위치 정보를 수집하기 위해서는 다음과 같이 선언해주어야 한다.

```xml
<!-- AndroidManifest.xml -->
<manifest>
  <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION"/>
</manifest>
```
### 수집 정확도 관련

안드로이드의 위치 정보는 크게 `대략적인 위치 정보`와 `정확한 위치 정보`로 나눌 수 있다.

- 대략적인 위치 정보는 약 3km 반경의 대략적인 위치 정보를 말한다.
- 정확한 위치 정보는 약 50m 수준의 정확한 위치 정보를 말한다. 얼마나 정확하게 받을지 선언하는 것에 따라 반경이 달라질 수 있다.
- 참고 : [안드로이드 공식 문서](https://developer.android.com/training/location/permissions?hl=ko)

대략적인 위치 정보는 `ACCESS_COARSE_LOCATION`이, 정확한 위치 정보는 `ACCESS_FINE_LOCATION`이 담당한다.

```xml
<manifest>
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
</manifest>
```

## 패키지 설치

사실 API를 요청하는 패키지의 종류는 정말 많고, 아마 내가 사용한 것보다 훨씬 더 좋은 패키지들이 많을 것이다.
다만 Flutter를 처음 접하는 초보자로써 그나마 유명하고(자료가 많고), 사용하기 좋은 것 같았던 패키지를 선택하였다.

Flutter에서 위치 정보를 요청하고, 결과를 받기 위해 다음과 같은 패키지를 사용하였다.

```yaml
background_fetch: ^1.1.6
geolocator: ^9.0.2
```

### background_fetch 설정

background_fetch를 사용하기 위해서는 운영체제에 맞는 설정 과정이 필요하다.
Android의 경우 Manifest 파일과 Build.gradle 파일을 수정해야했다.

공식문서에 자세히 설명되어 있으니 그대로 따라하면 무리 없이 동작한다.

> [Flutter - BackgroundFetch 공식 문서](https://pub.dev/documentation/background_fetch/latest/background_fetch/BackgroundFetch-class.html)

## 사용 (테스트)

본격적으로 앱에 적용하기 전에 잘 작동하는지, 어떤 형태로 데이터를 받아오는지 확인하기 위해 테스트과정을 진행하였다.

여기서는 앱의 상태를 크게 두 가지로 구분하였다.

1. 앱 프로세스가 실행 중인 상태 (백그라운드 포함)
2. 사용자가 앱을 종료한 상태 (terminated)

### 1. 앱 프로세스가 실행 중인 경우

`main.dart`의 `main()` 함수에서 initState를 설정하여 주기적으로 위치정보를 가져올 수 있도록 하였다.

코드는 다음과 같다.

- `BackgroundFetchConfig(<설정값>, <실행할 Fetch 함수>, <TimeOut시 처리>)`

```dart
testapi.dart

Future<void> initLocationState() async {
  int status = await BackgroundFetch.configure(
    BackgroundFetchConfig(
      // 최소 시간이 15분임
      minimumFetchInterval: 15,
      stopOnTerminate: false,
      enableHeadless: true,
      startOnBoot: true,
      requiredNetworkType: NetworkType.ANY,
      requiresBatteryNotLow: false,
      requiresCharging: false,
      requiresStorageNotLow: false,
      // 아래는 필요 없을 수도 있음
      requiresDeviceIdle: false,
    ),
    _onBackgroundFetch,
    _onBackgroundFetchTimeout,
  );
}
```

```dart
// main.dart
Future<void> main() async {
  // test api 실행
  initLocationState();
  runApp(MyApp());
}
```

안드로이드의 설정인지, 패키지가 제한하는 것인지는 모르지만 Fetch의 간격은 최소 15분이다. 더 낮은 수치를 줘도 15로 재조정된다.

안드로이드 공식 문서에서도 다양한 이유로 한 시간에 몇 번만 위치 정보 요청이 가능하다고 명시하고 있다.

```dart
// testapi.dart

Future<Position> getLocation() async {
  var currentPosition = await Geolocator.getCurrentPosition(
      desiredAccuracy: LocationAccuracy.bestForNavigation);
  return currentPosition;
}

void _onBackgroundFetch(String taskId) async {
  print("[BackgroundFetch] Event received $taskId");
  var currentLocation = await getLocation();
  print(currentLocation);
  BackgroundFetch.finish(taskId);
}

void _onBackgroundFetchTimeout(String taskId) async {
  print("[BackgroundFetch] TIMEOUT: $taskId");
  BackgroundFetch.finish(taskId);
}
```

Fetch가 실행하는 함수는 `getLocation()` 사용자 정의 함수를 실행하고,
`getLocation()`은 geolocator 패키지가 제공하는 `getCurrentPosition()`함수를 이용해 위치 정보를 받아온다.
- `LocationAccuracy.bestForNavigation`은 가장 높은 정확도의 위치 정보를 받아오겠다고 명시한 것이다.

![](/assets/img/posts/2023-05-22-flutter-android-백그라운드에서-위치-정보-받아오기-kakao-rest-api/img0.png)

함수의 반환값은 Position 객체로, 위도와 경도 데이터를 담고 있다.

**주의할 점은 TimeOut이 발생했을 때 반드시 task 종료를 선언해야 한다.** 그렇지 않으면 태스크가 삭제되지 않아 다양한 문제가 발생할 수 있다.

### 2. 앱 프로세스가 종료된 경우

Android에서는 `Headless task`라는 작업을 통해 프로세스가 종료된 이후에도 Fetch를 보내는 방법이 있다고 한다.
다만 에뮬레이터에서는 앱을 종료하면 디버깅이 같이 끝나 버리는 것 같아 아직 테스트를 해보지는 못했다.

`Headless task` 외에도 `Alarm Manager`라는 것을 이용해 백그라운드에서 앱의 기능을 실행할 수 있다고 하니, 만약 테스트가 잘 안되면 이를 이용하는 것도 고려해보자.

- 우선 Headless task를 이용하기 위해서는 initState에서 다음 두 가지를 선언해주어야 한다.

```dart
stopOnTerminate: false,
enableHeadless: true,
```

- 이후 main 함수에서 앱 실행 후 Headless task를 등록하는 과정을 거친다.

```dart
// main.dart
Future<void> main() async {
  // test api 실행
  initLocationState();
  runApp(MyApp());
 // Headless task 등록
 BackgroundFetch.registerHeadlessTask(backgroundFetchHeadlessTask);
}
```
- Headless task는 앱 프로세스가 실행될 때의 Fetch 함수와 동일한 동작을 한다.

```
void backgroundFetchHeadlessTask(HeadlessTask task) async {
  var taskId = task.taskId;
  var timeout = task.timeout;

  if (timeout) {
    print('[BackgroundFetch] Headless task timed-out: $taskId');
    BackgroundFetch.finish(taskId);
    return;
  }

  print('[BackgroundFetch] Headless event received.');

  if (taskId == "flutter_background_fetch") {
    var currentLocation = await getLocation();
    print(currentLocation);
    BackgroundFetch.finish(taskId);
    return;
  }

  BackgroundFetch.finish(taskId);
}
```

# Kakao REST API를 이용한 장소 추출

좌표 정보를 바탕으로 주변에 있는 장소들을 검색하는 기능을 구현해보고자 하였다.

원래 의도했던 것은 장소의 카테고리(음식점, 병원 등)에 상관없이 모든 장소 정보를 가져오고 있었는데, 구글은 이를 유료로 제공하고 있고, 네이버와 카카오는 카테고리를 반드시 지정해야 한다는 제한이 있었다.

우선은 무료로 사용할 수 있는 카카오 REST API를 이용하였다.

```dart
Future<void> main() async {
  Future<http.Response> getPlaces(var latitude, var longitude) async {
    var key = "YOUR_KEY";
    var baseUrl = "https://dapi.kakao.com/v2/local/search/category.json";
    // 정렬기준에는 accuracy와 distance가 있다는데 무슨 차이인지 모르겠음
    // group_code 참조 : https://developers.kakao.com/docs/latest/ko/local/dev-guide#search-by-category-request-category-group-code
    var uri = Uri.parse(
        "$$ baseUrl?category_group_code=FD6&x= $$longitude&y=$latitude&radius=10&sort=distance");
    var response =
        await http.get(uri, headers: {"Authorization": "KakaoAK $key"});

    print(response.body);
    return response;
  }
```

장소를 검색하는 `getPlaces()` 함수는 위도와 경도 데이터를 입력받아 KAKAO REST API에 POST요청을 넣어 반경 n미터에 있는 장소 정보를 받아오는 함수이다.

# 앞으로 할일

## Google Place API 사용

알고보니 Google Place API를 무료로 사용할 수 있는 방법이 있었다.

```
maxprice
Restricts results to only those places within the specified range. 
Valid values range between 0 (most affordable) to 4 (most expensive), inclusive. 
The exact amount indicated by a specific value will vary from region to region.
```

구글의 장소 검색은 정확도에 따라 과금 정책이 결정되는데 Price가 0인 검색은 가장 기본적인 정보만을 가져오는 대신 무료이다.
`maxprice` 파라미터를 통해 무료 검색만을 사용하게 끔 설정할 수 있다.

## Weather API

위치 정보를 가져오고, 또 가져오는 시점의 시간또한 알 수 있으니 이를 이용해 사용자가 위치한 곳의 날씨 정보도 가져올 수 있을것 같아 이를 가져오는 기능을 개발해보고자 한다.

# 코멘트

Flutter를 처음써보기 때문에 UI 구현보다는 Fetch와 같이 데이터를 주고 받은 과정이 그나마 익숙할 것 같아 이런 기능을 구현하는 역할을 맡게 되었다.

주말동안 노마드 코더를 통해 Flutter 벼락치기를 하고, 구글을 끼고 살며 코드를 작성했는데, 다행히 목표했던 기능이 모두 동작이 되는 것 같아 다행이다.

앞으로도 내가 맡은 부분을 무사히 완수할 수 있다면 좋겠다.

        