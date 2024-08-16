---
title: Flutter Libraries
tags: flutter
---

메타정보만 간단히 기록

## 상태관리

### get: ^4.6.6 (14307)

#### RX

```dart
RxList<bool> isChecked = RxList<bool>(List.generate(labels.length, (_) => false));
// RxList에 Iterable 리스트 넣기
isChecked.assignAll(List.generate(isChecked.length, (index) => isAllChecked));
```

#### Page 이동

```dart
Future<void> signOut() async {
  await Get.offAll(() => const SignInView());
  userRepository.logout();
  await FirebaseAuth.instance.signOut();
}
```

위와같이 페이지 이동에 await를 걸어두면 GetxController가 제거되며 뒤에있는 함수들(logout, signOut)이 실행되지 않았다. 주의해야 할듯 하다.

### provider: ^6.1.2 (9690)

* 화면 일부분만 reBuild하려고 써봤다. StatefulBuilder 혹은 Stateful과 GlobalKey로 하는 방법도 있는 듯 하다. https://vintageappmaker.tistory.com/510

* Provider.of<T>(context) 는 watch와 유사하게 동작
* Provider.of<T>(context, listen: false) 는 read와 유사하게 동작

### stacked: ^3.4.0 (1388)

MVVM 구조를 지원하는 라이브러리

<details>
<summary>stacked 정리</summary>
<div markdown="1">

  ## View Models

  그냥 ChangeNotifier를 확장한 Dart 클래스

  사용자가 View와 상호 작용할 때 작업을 수행하는 Dart 클래스

  ### BaseViewModel

  기본적인 ViewModel
    
  ViewModel의 상태(busy)를 컨트롤 할 수 있으며, 전달된 객체를 기반으로 상태를 설정할 수 있다.

  Future를 반환하는 함수를 사용한다면 자동으로 상태(busy)가 true, false가 되는 듯 하다.

  ### Busy handling

  키를 할당하여 각 객체별로 고유하게 사용 중 인지(viewModel.busy(BusyObjectKey)) 식별할 수 있다.

  ### Error Handling

  runBusyFuture() 또는 runErrorFuture()에서 에러 발생 시 사용자가 사용할 수 있게 ViewModel 또는 전달된 키에 예외를 저장한다.

  viewModel.error(BusyObjectKey) 오류 검색

  viewModel.hasErrorForKey(BusyObjectKey) 사용한 키에 에러가 있는지 간단히 확인
  
  runBusyFuture()를 통해 키가 제공되지 않았다면 viewModel.hasError를 통해 에러있는지 확인 가능

  viewModel.modelError를 통해 실제 에러 가져올 수 있다.

  미래의 오류에 대응하려면 해당 미래에 사용한 예외와 키를 반환하는 viewModel.onFutureError(error, key)를 재정의할 수 있다

  ## Special View Models

  상용구 코드를 줄이는 특수한 ViewModel

  ### ReactiveViewModel

  BaseViewModel을 확장하고 ViewModel에서 사용 중인 서비스를 수신할 수 있는 기능이 있다. 즉 ViewModel이 서비스에 반응한다.

  ### StreamViewModel

  ### FutureViewModel

  BaseViewModel을 확장하여 데이터를 가져오는 Future를 쉽게 수신할 수 있는 기능을 제공

  항목을 선택한 후 사용자에게 보여줄 추가 데이터를 가져와야 하는 Details 뷰에 적절

</div>
</details>

### stacked_services: ^1.1.0 (261)
stacked 라이브러리. NavigationService, DialogService, SnackbarService, BottomSheetService 제공

### stacked_shared: ^1.4.0 (2)
stacked 패키지 간의 공유 코드 모음

## 저장

### shared_preferences: ^2.2.2 (8883)
간단판 내용을 파일로 저장해두고 앱을 시작할 때 파일을 읽어온다. 디바이스 디스크의 데이터가 유지된다는 보장이 없으므로 중요한 데이터 저장은 권장하지 않는다

### hive: ^2.2.3 (5552)
기기의 내부저장소를 사용하는 키-값 데이터베이스

#### hive와 frezzed 같이 사용하기

```dart
import 'package:flutter/foundation.dart';
import 'package:freezed_annotation/freezed_annotation.dart';
import 'package:hive_flutter/adapters.dart';
part 'user_summary.freezed.dart';
part 'user_summary.g.dart';

@freezed
abstract class UserSummary extends HiveObject with _$UserSummary {
  UserSummary._();

  @HiveType(typeId: 1, adapterName: 'UserSummaryAdapter')
  factory UserSummary({
    @HiveField(0) String? email, // 이메일
    @HiveField(1) required String username, // 회원 아이디
  }) = _UserSummary;

  factory UserSummary.fromJson(Map<String, Object?> json) =>
      _$UserSummaryFromJson(json);
}
```

출처 : https://stackoverflow.com/questions/60383178/combine-freezed-hive

* adapterName을 지정안해도 되지만 지정 안하면 UserSummaryImplAdapter 이런식으로 자동생성 됐다.

```dart
userSummary.username = 'Lucas';
userSummary.save(); // Update object
```

하지만 만들고 보니, 위와 같이 객체 내부 속성 변경이 불가능했다. @freezed는 변경 불가능한 모델을 만드는 것이므로..

즉, Hive 공식 문서대로 사용해 보려면 @unfreezed로 변경이 필요했다.


### sqflite: ^2.3.3 (4614)
SQLite 플러그인. 모든 폰에는 SQLite 데이터베이스가 있다. 복잡한 데이터를 기기에 저장할 수 있다.

## Carousel, Slide, 이미지 관련

### image_picker: ^1.1.1 (6614)
갤러리에서 사진, 동영상을 가져오거나 카메라로 사진, 동영상을 촬영하여 바로 쓸 수 있다.
안드로이드에선 메모리 부족시 앱을 종료한다고 한다. 이를 대비한 코드도 설명되어 있으니 참고하자

이미지를 선택해올 때 limit를 줘도 안드로이드 일정버전 이하에선 잘 실행되지 않았다.

### cached_network_image: ^3.3.1 (5719)
인터넷 이미지 표시 및 캐시 디렉터리에 보관

### carousel_slider: ^4.2.1 (5078)
card_swiper 라이브러리와 비슷하다

### flutter_svg: ^2.0.9 (4804)
svg 라이브러리 

### photo_view: ^0.15.0 (2762)
이미지 슬라이드 및 터치하여 이미지 확대 가능

<details>
<summary>무한스크롤 PhotoViewGallery</summary>
<div markdown="1">

```dart
class CustomPhotoView extends StatefulWidget {
  const CustomPhotoView(
      {super.key, required this.images, required this.itemIndex});
  // 이미지 주소만 String 배열로 받았다
  final List<String> images;
  // itemIndex는 예를들어 사용자가 0 ~ 3 이미지 배열이 있을 때 두 번째 이미지를 터치했다면 index 1을 전달하는 변수
  final int itemIndex;

  @override
  State<CustomPhotoView> createState() => _CustomPhotoViewState();
}

class _CustomPhotoViewState extends State<CustomPhotoView> {
  late PageController _pageController;
  late int currentIndex;

  @override
  void initState() {
    _pageController = PageController(initialPage: widget.images.length * 500);
    currentIndex = widget.itemIndex;
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: PhotoViewGallery.builder(
        scrollPhysics: widget.images.length == 1 // 이미지 1개면 좌우스크롤 불가
            ? const NeverScrollableScrollPhysics()
            : const BouncingScrollPhysics(),
        customSize: Size(screenWidth(context), screenWidth(context)),
        builder: (BuildContext context, int index) {
          return PhotoViewGalleryPageOptions(
            imageProvider: NetworkImage(widget
                .images[(index + widget.itemIndex) % widget.images.length]),
          );
        },
        itemCount: widget.images.length * 1000,
        loadingBuilder: (context, event) => Center(
          child: SizedBox(
            width: 20.0,
            height: 20.0,
            child: CircularProgressIndicator(
              value: event == null
                  ? 0
                  : event.cumulativeBytesLoaded / event.expectedTotalBytes!,
            ),
          ),
        ),
        pageController: _pageController,
        onPageChanged: (value) => setState(
          () {
            currentIndex = (value + widget.itemIndex) % widget.images.length;
          },
        ),
      ),
    );
  }

  @override
  void dispose() {
    _pageController.dispose();
    super.dispose();
  }
}
```
</div>
</details>

### flutter_image_compress: ^2.2.0 (1320)
이미지를 압축하여 전송. minWidth, minHeight를 지정하면 이미지 비율은 깨지지 않은채 내가 지정한 이미지 크기와 최대한 비슷하게 맞춰준다

### card_swiper: ^3.0.1 (1030)
무한루프의 Swiper/Carousel

### easy_image_viewer: ^1.4.1 (231)
풀 화면에서 두 손가락으로 줌 인, 줌 아웃, 여러 이미지 스와이프 가능

### flutter_image_slideshow: ^0.1.6 (163)
간단한 이미지 슬라이드쇼 위젯입니다. 주로 이미지 위젯용이지만 다른 위젯도 사용할 수 있다.

## 디자인, 레이아웃, 위젯

### auto_size_text: ^3.0.0 (4381)
지정한 범위 내에 완벽하게 맞도록 텍스트 크기를 자동으로 조정 

* table_calendar: ^3.1.1 - 달력 위젯 (2659)

* flutter_rating_bar: ^4.0.1 - 별점 주는 위젯 (2172)

* dropdown_search: ^5.0.6 - 하위 항목 검색 기능을 갖춘 드롭다운 (1656)

* dropdown_button2: ^2.3.9 - 기본 DropdownButton은 item들의 높이가 kMinInteractiveDimension(48.0)으로 고정되어 있어서 위 라이브러리를 사용해봤다. DropdownButton을 다양하게 커스터마이징 할 수 있다. (1431)

* cupertino_icons: ^1.0.6 - Cupertino 위젯 에서 사용되는 기본 아이콘들 (766)

* flutter_layout_grid: ^2.0.6 - 복잡한 사용자 인터페이스 디자인에 최적화된 그리드 (676)

* stop_watch_timer: ^3.1.1 - 스톱워치 만들기 (248)

* easy_rich_text: ^2.1.0 - 첨자, Trademark 표현가능. targetString 혹은 정규식을 이용해 텍스트에 스타일을 바꾼다. (202)

* flutter_adaptive_ui: ^0.8.0+1 - 다양한 플랫폼에 반응형 UI 구현을 돕는다 (144)

* draggable_bottom_sheet: ^1.0.2 - 화면에 유지되며 끌어서 전체 화면을 덮을 수 있는 바텀 시트 (111)

* roundcheckbox: ^2.0.5 - 동그란 체크박스 (91)

* lazy_load_indexed_stack: ^1.1.0 - 메인 페이지 하단 내비바 및 탭구분 시 유용 (79)

* checkbox_grouped: ^2.0.0 - 체크박스 다중 선택, 전체 활성화 비활성화 등.. (41)

* simple_animation_progress_bar: ^1.8.2 - 진행률을 표시하는 막대 위젯 (17)

## 애니메이션

* animations: ^2.0.11 - 사용자 정의 애니메이션을 만들거나 미리 만들어진 애니메이션을 사용할 수 있다 (5789)

* animated_text_kit: ^4.2.2 - 텍스트에 애니메이션 효과 주기 (4763)

* animate_do: ^3.3.4 - 다양한 애니메이션 효과들을 줄 수 있다 (4068)

* loading_animation_widget: ^1.2.1 - 로딩 중 애니메이션 표시 (1353)

* lottie: ^3.1.0 - Lottie란? JSON 기반의 애니메이션 파일. 용량이 작고, 기기 호환성이 높으며, 크기를 조정하도 해상도가 낮아지지 않는다 (3579)

### animated_theme_switcher: ^2.0.10 (442)
애니매이션 효과주면서 테마 바꾸기

## 다국어, 현지화

### intl: ^0.19.0 (5072)

날짜/숫자 형식 지정. 다국어 지원.

## 소셜 로그인

### google_sign_in: ^6.2.1 (2948)
구글 로그인

* IOS 설정 방법 - https://pub.dev/packages/google_sign_in_ios

  * GoogleService-Info.plist 파일이 필요 없다.

### kakao_flutter_sdk_user: ^1.9.2 (11)

카카오 로그인

* Firebase OpenID 연동 방법

  새 제공업체 추가 → 새 OIDC 제공업체 정의

  이름 - 카카오

  클라이언트 ID - 네이티브 앱 키 (여기에 REST API 키를 넣고 했다가 안됐는데 검색해보니 네이티브 키였다...)

  발급자(URL) - https://kauth.kakao.com

  클라이언트 보안 비밀번호 - kakao developers → 카카오 로그인 → 보안 → Client Secret 생성 및 복붙

## Firebase

### firebase_core: ^3.0.0 (3400)

https://firebase.google.com/docs/flutter/setup?hl=ko&authuser=0&_gl=1

Firebase CLI 설치 → FlutterFire CLI 설치 → firebase login
  ```
  Firebase CLI 설치 방법
  npm 없이 설치 curl -sL https://firebase.tools | bash
  설치경로 /usr/local/bin/firebase

  npm으로 설치 npm install -g firebase-tools
  설치경로 /Users/gyun/.nvm/versions/node/v20.14.0/bin/firebase

  FlutterFire CLI 설치 방법
  dart pub global activate flutterfire_cli
  ```

Flutter 프로젝트 디렉토리로 이동 → flutterfire configure 명령 실행 → firebase.json, firebase_options.dart, google-services.json 및 각종 파일들 자동 생성 → main()에 아래와 같은 함수 추가 → flutter run
  ```dart
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
  ```

이후 플러그인 설치방법
  ```
  flutter pub add PLUGIN_NAME
  flutterfire configure
  flutter run
  ```

flutterfire configure 이란?
  ```
  이렇게 flutterfire configure를 처음 실행한 후에는 다음 경우에 언제든지 명령어를 다시 실행해야 합니다.

  • Flutter 앱에서 새 플랫폼 지원을 시작합니다.

  • 특히 Google, Crashlytics, Performance Monitoring, Realtime Database로 로그인을 시작할 때와 같이 Flutter 앱에서 새 Firebase 서비스 또는 제품을 사용합니다.

  명령어를 다시 실행하면 Flutter 앱의 Firebase 구성이 최신 상태로 유지되고 Android의 경우 필요한 Gradle 플러그인이 앱에 자동으로 추가됩니다.
  ```

etc

  * Apple 플랫폼에서 클라우드 메시징을 사용하려면 다음 기본 요건을 충족해야 합니다.
    ```
    실제 Apple 기기를 설정합니다.
    Apple 개발자 계정의 Apple 푸시 알림 인증 키를 가져옵니다.
    Xcode의 App(앱) > Capabilities(기능)에서 푸시 알림을 사용 설정합니다.
    ```

### firebase_auth: ^5.0.0 (3755)

에뮬레이터 사용방법

```
firebase init emulators
firebase emulators:start
```

그럼 firebase.json에 아래와 같은 설정이 추가된다.

```
{
  "emulators": {
    "auth": {
      "port": 9099
    },
    "ui": {
      "enabled": true
    },
    "singleProjectMode": true
  }
}
```

메인함수에 추가

```dart
await FirebaseAuth.instance.useAuthEmulator('localhost', 9099);
```

만약 emulators를 start하지 않고 위 코드를 그대로 넣어두면 아래와 같은 에러가 뜬다.

```
[ERROR:flutter/runtime/dart_vm_initializer.cc(41)] Unhandled Exception: [firebase_auth/network-request-failed] Network error (such as timeout, interrupted connection or unreachable host) has occurred.
```

로그인 방법은 공식문서에 쉽게 나와있다.

https://firebase.google.com/docs/auth/flutter/start?hl=ko&authuser=0&_gl=1

### Naver Login + Firebase Auth 연동방법

카카오는 OpenID 지원을 해줘서 쉽게 Firebase에 연동할 수 있었지만 네이버는 왜...

flutter_naver_login: ^1.8.0 라이브러리를 이용해 쉽게 연동할 수 있다.

#### Flutter 소스

```dart
Future<void> signIn() async {
  const String url = '여기에 클라우드 함수 주소 넣기';

  final NaverLoginResult result = await FlutterNaverLogin.logIn();

  Response<String> customToken = await dio.post(
    url,
    data: {
      'id': result.account.id,
      'name': result.account.name,
      'email': result.account.email
    },
    options: Options(
      headers: {
        'Content-Type': 'application/json',
      },
    ),
  );

  FirebaseAuth.instance.signInWithCustomToken(customToken.toString());
}
```

#### Firebase Functions 소스

```javascript
const {onRequest} = require("firebase-functions/v2/https");
const logger = require("firebase-functions/logger");
const {onDocumentCreated} = require("firebase-functions/v2/firestore");

// const {initializeApp} = require("firebase-admin/app");
const {getFirestore} = require("firebase-admin/firestore");
const admin = require('firebase-admin');
const functions = require('firebase-functions');

admin.initializeApp();

exports.createCustomToken = functions.region('asia-northeast3').https.onRequest(async (req, res) => {

  const uid = req.body.id;
  const name = req.body.name;
  const email = req.body.email;

  try {
    await admin.auth().getUser(uid);
  } catch (error) {
    if (error.code === 'auth/user-not-found') {
      await admin.auth().createUser({
        uid: uid,
        displayName: name,
        email: email,
        emailVerified: true
      });
    } else {
      logger.info("Error : ", error);
    }
  }

  res.send(await admin.auth().createCustomToken(uid));
});
```

flutter_naver_login가 Authorization Code를 얻어 Access token을 발급받아 주는 것 까지 해결해 주는 듯 하다.

또한 로그인에 성공하면 기본적으로 유저 정보를 리턴하므로 그 정보를 이용해 난 Firebase에 유저를 만들고 Firebase Token발급만 했다.

마지막으로 Firebase Functions에서 토큰을 발급하려면 Firebase Admin SDK에서 사용하는 서비스 계정에 토큰 발급 권한이 필요하다.

서비스 계정은 일반적으로 {project-id}@appspot.gserviceaccount.com 형식이며 필요한 권한은 Service Account Token Creator(서비스 계정 토큰 생성자)이다.

### Cloud Functions

이건 따로 PlugIn을 설치할 필요없다.

```
Flutter 프로젝트에 Firebase Cloud Functions 설치. Flutter 프로젝트에 functions폴더가 생긴다
firebase init functions
```

Function 테스트는 emulators사용을 적극 권장한다. 함수안에서 무한루프로인해 **요금**이 청구될 수 있기 때문이다.

```
아래와 같이 실행됐을 때 http://127.0.0.1:4002/ 에서 Logs탭에서 함수 로그를 볼 수 있다.
또한 아래에 나와있듯이 Functions의 Host:Port가 127.0.0.1:5001 이므로 http://localhost:5001/MY_PROJECT/us-central1/helloWorld 처럼 요청을 보내면 된다.
┌─────────────────────────────────────────────────────────────┐
│ ✔  All emulators ready! It is now safe to connect your app. │
│ i  View Emulator UI at http://127.0.0.1:4002/               │
└─────────────────────────────────────────────────────────────┘

┌────────────────┬────────────────┬─────────────────────────────────┐
│ Emulator       │ Host:Port      │ View in Emulator UI             │
├────────────────┼────────────────┼─────────────────────────────────┤
│ Authentication │ 127.0.0.1:9099 │ http://127.0.0.1:4002/auth      │
├────────────────┼────────────────┼─────────────────────────────────┤
│ Functions      │ 127.0.0.1:5001 │ http://127.0.0.1:4002/functions │
├────────────────┼────────────────┼─────────────────────────────────┤
│ Firestore      │ 127.0.0.1:8080 │ http://127.0.0.1:4002/firestore │
└────────────────┴────────────────┴─────────────────────────────────┘
```

```
실제 프로젝트에 배포 방법
firebase deploy --only functions
```

### firebase_messaging: ^15.0.3 (3495)

앱 알림 메시지를 보낼 수 있다

메시지를 사용자 앱에 보낼 때 기기 상태에 따라 메세지는 다르게 처리된다.

* Foreground - 앱이 열려있고 사용자가 앱을 보고있는 상태
* Background - 앱이 열려있지만 백그라운드(최소화), '홈' 혹은 다른 앱을 보고 있는 상태
* Terminated - 기기가 잠겨있거나 내 앱이 실행되고 있지 않은 상태

#### iOS, macOS 및 웹 알림 권한 받기

Android는 사용자가 운영체제 설정을 통해 앱 알림을 중지하지 않은 경우, 알림은 기본 허용이다.

```dart
FirebaseMessaging messaging = FirebaseMessaging.instance;

NotificationSettings settings = await messaging.requestPermission(
  alert: true,
  announcement: false,
  badge: true,
  carPlay: false,
  criticalAlert: false,
  provisional: false,
  sound: true,
);
```

#### 백그라운드 메시지

메시지가 수신되면 격리가 생성됩니다(Android 전용, iOS/macOS에는 별도의 격리가 필요 없음). 이를 통해 애플리케이션이 실행되지 않고 있더라도 메시지를 처리할 수 있습니다.

핸들러는 애플리케이션 컨텍스트 외부에서 독립적으로 실행되므로 애플리케이션 상태를 업데이트하거나 UI에 영향을 주는 로직을 실행할 수 없습니다. 그러나 HTTP 요청과 같은 로직을 수행하고 IO 작업(예: 로컬 스토리지 업데이트)을 수행하며 다른 플러그인과 통신할 수 있습니다.

가능한 한 빨리 로직을 완료하는 것이 좋습니다. 집약적이고 오래 실행되는 태스크를 실행하면 기기 성능이 영향을 받고 OS에서 프로세스를 종료할 수 있습니다. 태스크가 30초 넘게 실행되면 기기에서 자동으로 프로세스를 종료할 수 있습니다.

```dart
@pragma('vm:entry-point')
Future<void> _firebaseMessagingBackgroundHandler(RemoteMessage message) async {
  // If you're going to use other Firebase services in the background, such as Firestore,
  // make sure you call `initializeApp` before using other Firebase services.
  await Firebase.initializeApp();

  print("Handling a background message: ${message.messageId}");
}

void main() {
  FirebaseMessaging.onBackgroundMessage(_firebaseMessagingBackgroundHandler);
  runApp(MyApp());
}
```

위 내용은 Firebase 공식문서를 긁어왔다. 나는 아직 사용해보진 않았지만 나중에 사용해 보면 좋을 듯 하다.

#### 메시지 수신

```dart
Future<void> setupInteractedMessage() async {
  // Get any messages which caused the application to open from
  // a terminated state.
  // Terminated 상태에서 앱을 켰을 때 메시지 수신
  RemoteMessage? initialMessage =
      await FirebaseMessaging.instance.getInitialMessage();

  if (initialMessage != null) {}

  // Also handle any interaction when the app is in the background via a
  // Stream listener
  // Background 상태에서 메시지 수신
  FirebaseMessaging.onMessageOpenedApp.listen(
    (RemoteMessage message) {

    },
  );

  // foreground 상태에서 메시지 수신
  FirebaseMessaging.onMessage.listen((RemoteMessage message) {
    Logger().d('Got a message whilst in the foreground!');
    Logger().d('Message data: ${message.data}');

    if (message.notification != null) {
      Logger().d(
          'Message also contained a notification: ${message.notification}');
    }
  });
}
```

**주의**

```dart
@override
void initState() {
  super.initState();

  // Run code required to handle interacted messages in an async function
  // as initState() must not be async
  setupInteractedMessage();
}
```

initState() 메서드 자체를 비동기 함수로 만들면 안되기 때문에, etupInteractedMessage()라는 비동기 함수로 따로 분리하여 비동기 작업(예: 메시지 수신 대기, 데이터 초기화)을 처리한다.

생명 주기 메서드를 비동기 작업으로 처리할 경우 호출 순서가 보장되지 않는다.

### cloud_firestore: ^5.0.1 (3375)

Firestore Database

IOS 에서 첫 빌드시 Firestore SDK 때문에 5분 이상 걸릴 수 있다고 한다.

선택사항이지만 ios/Podfile 에 다음과 같은 코드를 추가하면 첫 빌드시간을 줄일 수 있다.

```
target 'Runner' do
  use_frameworks!
  use_modular_headers!

  pod 'FirebaseFirestore',
    :git => 'https://github.com/invertase/firestore-ios-sdk-frameworks.git',
    :tag => 'IOS_SDK_VERSION'

  flutter_install_all_ios_pods File.dirname(File.realpath(__FILE__))
  target 'RunnerTests' do
    inherit! :search_paths
  end
end

IOS_SDK_VERSION을 firebase_core의 firebase_sdk_version.rb 파일에 지정된 Firebase iOS SDK 버전으로 바꾼다.

.pub-cache/hosted/pub.dev/firebase_core-3.1.0/ios/firebase_sdk_version.rb
```

#### db에 값 넣기 add, set

```dart
final db = FirebaseFirestore.instance;

final user = <String, dynamic>{
  "first": "Ada",
  "last": "Lovelace",
  "born": 1815
};

db.collection('users').add(user);
db.collection('users').doc('alovelace').set(user);
```

대표적인 2가지 방법이 있다.

차이점

* collection수준에서 add()를 하게되면 doc의 ID가 고유한 값으로 자동 생성된다. 따라서 같은 유저정보라도 doc의 ID가 중복되지 않으므로 add()를 할 때 마다 doc이 계속 추가된다.

* doc수준에서 set()을 하게되면 doc의 ID를 이미 지정해 줬으므로 해당 ID(여기선 alovelace)에 계속 덮어쓰기 된다.

#### 인터넷이 없는 상태에서 set

* set()

  ```dart
  await docRef.set(user);

  Logger().d('set 끝'); // 출력 안됨
  ```

  인터넷이 없는 상태에서 await를 걸어두면 그 이후 코드들은 실행이 안되고 에러도 나지 않는다. stream으로 db에 값이 입력될 때 까지 기다린다. db에 값을 넣을 때 await는 사용하지 말자.

#### db에서 값 가져오기 get

get() 호출은 데이터베이스에서 최신 문서 스냅샷 가져오기를 시도한다. 오프라인 작동이 지원되는 플랫폼에서 클라이언트 라이브러리가 네트워크를 사용할 수 없거나 요청 시간이 초과되면 오프라인 캐시를 사용한다고 한다.

```dart
DocumentSnapshot doc = await docRef.get(const GetOptions(source: Source.server));
DocumentSnapshot doc = await docRef.get(const GetOptions(source: Source.cache));
```

GetOptions을 통해 어디에서만 값을 가져올지 정할 수도 있다.

```dart
if (doc.exists) {
  final data = doc.data() as Map<String, dynamic>;
  Logger().d(data);
}
```

doc(Document)의 id는 현재 서버에 존재하지 않을 수 있다. 이를 대비해 위와같이 if문을 사용하는 것도 좋은 듯 하다.

* collection 수준에서 get()

  collection id를 기준으로 collection에 속한 모든 doc을 가져온다.

    ```dart
  QuerySnapshot a = await db
      .collection("cities")
      .doc("SF")
      .collection("landmarks")
      .get();
  Logger().d(a.docChanges);
  Logger().d(a.docs);
  ```

  <span style="color:#2D3748; background-color:#fff5b1">인터넷 연결 + 실존하는 collection</span> : 모든 doc를 가져온다. 에러 안남.

  <span style="color:#2D3748; background-color:#fff5b1">인터넷 연결 + 존재하지 않는 collection</span> : 빈 배열을 가져온다. 에러 안남.

  <span style="color:#2D3748; background-color:#fff5b1">인터넷 미연결 + 실존하는 collection</span> : 빈 배열을 가져온다. 단, 로컬 캐쉬에 collection이 저장되어 있다면 그 collection의 모든 doc를 가져온다. 에러 안남.

  <span style="color:#2D3748; background-color:#fff5b1">인터넷 미연결 + 존재하지 않는 collection</span> : 빈 배열을 가져온다. 에러 안남.

* doc 수준에서 get()

  doc id를 기준으로 doc 속한 모든 data(필드(key) + 값(value))를 가져온다.

  <span style="color:#2D3748; background-color:#fff5b1">인터넷 연결 + 존재하지 않는 doc</span> :

  ```dart
  DocumentSnapshot doc = await docRef.get();

  Logger().d(doc.data()); // null
  ```

  인터넷에 연결됐을 때는 null이 리턴된다.

  <span style="color:#2D3748; background-color:#fff5b1">인터넷 미연결 + 존재하지 않는 doc</span> :

  ```dart
  DocumentSnapshot doc = await docRef.get();

  // [cloud_firestore/unavailable] The service is currently unavailable. This is a most likely a transient condition and may be corrected by retrying with a backoff.

  Logger().d(doc.data());
  ```

  인터넷에 연결안됐을 때 로컬에 캐쉬가 없으면 위와같은 에러가 났다.   
  만약 서버에 값이 없고 로컬에만 값이있다면 위 코드는 정상적으로 작동한다.

* DocumentSnapshot 수준에서 get('1')

  doc의 필드값(key값)으로 value를 가져온다.

#### 인터넷이 없는 상태에서 set 에러처리

인터넷이 없는 상태에서 db에 값을 넣으면 따로 에러가 나지 않았다. 찾아보니 급방 다시 인터넷이 재연결될 것을 가정하고, stream을 통해 계속해서 db에 add() 혹은 set()을 반복하고 있었다. 즉 try-catch로 잡을 수 없었다.

여러가지 방법을 시도해봤다.

```dart
// 둘 다 같은 의미
final docRef = db.collection("users").doc("alovelace");
final documentRef = db.doc("users/alovelace");

docRef
    .set(user)
    .timeout(const Duration(seconds: 1))
    .onError(
  (error, stackTrace) {
    Logger().d("에러입니다", error: error);
    docRef.delete();
  },
);
```

이 방법은 1초동안 응답이 없으면 timeout에러를 발생시키고 delete()또한 예약해 둔다.   
set() stream을 죽이면 좋겠지만 이 방법을 몰라 delete()를 예약시켰다.   
하지만 이 방법도 이상해보인다. 무조건 set() 후 일정시간(여기선 1초)를 기다려야 했고, 난 시간과 관계없이 set()에서 에러가 났을 때 에러처리를 하고 싶었다.

```dart
docRef.snapshots(includeMetadataChanges: true).listen((event) {
  Logger().d(event.metadata.isFromCache); // t f f
  Logger().d("${event.metadata.hasPendingWrites} data: ${event.data()}"); // t t f
});
```

위 방법으로도 해 봤지만 여간 백엔드에 쓰기가 완료됐는지 확인하기 어려웠다...

내가 모르는 방법이 있을 수 있지만, 일단 DB insert가 중요한 데이터는 Cloud Firestore를 사용하지 말아야 겠다.

### firebase_remote_config: ^5.0.2 (527)

```dart
final remoteConfig = FirebaseRemoteConfig.instance;

await remoteConfig.setDefaults(const {
    "example_param_1": 42,
    "example_param_2": 3.14159,
    "example_param_3": true,
    "example_param_4": "Hello, world!",
});
```

firebase remote config기본 값을 지정해 줄 수 있다. 실제로 Firebase Console에 없는 변수여도 괜찮다. 무조건 Firebase에서 값을 가져와 사용할 거라면 위 과정은 필요없어 보인다.

```dart
await remoteConfig.fetchAndActivate();
```

내가 Firebase Console Remote Config에 설정한 값들을 가져오는 함수다.   
원격 구성 백엔드에서 매개변수 값을 가져오기 - remoteConfig.fetch()   
가져온 매개변수 값을 앱에 적용하기 - remoteConfig.activate()   
위 두 과정을 따로따로 할 수 있었다.

```dart
remoteConfig.getInt('min_version');

remoteConfig.getBool('min_version');

remoteConfig.getDouble('min_version');

remoteConfig.getString('min_version');

remoteConfig.getAll();

RemoteConfigValue value = remoteConfig.getValue('min_version');
```

백엔드에서 설정한 값들을 앱에서 사용할 변수 값으로 가져오는 방법들이다. 만약 <span style="color:#2D3748; background-color:#fff5b1">백엔드에서 변수값을 가져오지 않았거나 fetchAndActivate() 필요</span>, <span style="color:#2D3748; background-color:#fff5b1">타입이 불일치하는 경우</span> 혹은 <span style="color:#2D3748; background-color:#fff5b1">변수명이 존재하지 않는 경우</span> 등 기본값으로 설정된다.   
각 타입별 기본값은 RemoteConfigValue Class를 보면 알 수 있다.

getAll()과 getValue('key')의 경우 RemoteConfigValue 인스턴스로 리턴된다. 이를 Decode하는 방법은 다음과 같다.

```dart
value.asBool();
value.asDouble();
value.asInt();
value.asString();
```

이 또한 RemoteConfigValue Class를 보면 알 수 있다.

```dart
await remoteConfig.setConfigSettings(RemoteConfigSettings(
  fetchTimeout: const Duration(minutes: 1),
  minimumFetchInterval: const Duration(hours: 12),
));
```

위 설정은 Firebase에서 remote config를 가져오는 간격을 설정한다.

만약 await remoteConfig.fetchAndActivate();로 변수값들을 가져온지 12시간이 지나지 않았다면 아무리 await remoteConfig.fetchAndActivate();를 실행해도 Firebase console에서 업데이트한 값이 오지 않는다.

앱 데이터를 다 날려버리면 시간에 관계없이 다시 값을 받아온다.

### firebase_crashlytics: ^4.0.0 (1119)

```dart
// @pragma('vm:entry-point')
// Future<void> _firebaseMessagingBackgroundHandler(RemoteMessage message) async {
//   await Firebase.initializeApp(
//     options: DefaultFirebaseOptions.currentPlatform,
//   );
// }

void main() {
  runZonedGuarded(
    () async {
      WidgetsFlutterBinding.ensureInitialized();

      await Firebase.initializeApp(
        options: DefaultFirebaseOptions.currentPlatform,
      );

      FlutterError.onError =
          FirebaseCrashlytics.instance.recordFlutterFatalError;

      PlatformDispatcher.instance.onError = (error, stack) {
        FirebaseCrashlytics.instance.recordError(
          error,
          stack,
          fatal: true,
          information: ['main'],
        );
        return true;
      };

      Isolate.current.addErrorListener(RawReceivePort((pair) async {
        final List<dynamic> errorAndStacktrace = pair;
        await FirebaseCrashlytics.instance.recordError(
          errorAndStacktrace.first,
          errorAndStacktrace.last,
          fatal: true,
        );
      }).sendPort);

      ErrorWidget.builder = (FlutterErrorDetails details) {
        return Scaffold(body: errorWidget);
      };

      // FirebaseMessaging.onBackgroundMessage(
      //     _firebaseMessagingBackgroundHandler);

      KakaoSdk.init(
        nativeAppKey: kakaoNativeAppKey,
        javaScriptAppKey: kakaoJavaScriptKey,
      );

      // await FirebaseAuth.instance.useAuthEmulator('localhost', 9099);

      runApp(const MainApp());
    },
    (error, stack) {
      if (error is DioException) {
        if (error.response != null) {
          FirebaseCrashlytics.instance.recordError(error, stack, fatal: true);
        }
      } else {
        FirebaseCrashlytics.instance.recordError(error, stack);
      }
    },
  );
}
```

기본적으로 firebase_crashlytics를 위와같이 적용해봤다. DioException은 Network Error를 제외한 것만 기록하고 싶어 위 처럼 if문을 넣어봤다.

#### firebase_crashlytics가 기록안되는 경우?

즉, runZonedGuarded까지 에러가 올라가지 않으면 기록이 되지 않는 듯 하다.

FutureBuilder에서 error가 났을 때, errorWidget을 return하면서 Exception을 상위로(runZonedGuarded)올리지 않는 듯 하다. 그래서 아래와 같이 errorWidget을 return 하기 전에 recordError()를 넣었다.

```dart
Widget defaultFutureBuilder(
    Rxn<Future> future, Future Function() futureFunction, Widget widget) {
  return Obx(
    () => FutureBuilder(
      future: future.value,
      builder: (BuildContext context, AsyncSnapshot snapshot) {
        switch (snapshot.connectionState) {
          case ConnectionState.active:
          case ConnectionState.waiting:
            return Center(child: defaultProgressIndicator);
          case ConnectionState.done:
            if (snapshot.hasError) {
              if (snapshot.error is DioException) {
                final DioException dioError = snapshot.error as DioException;
                if (dioError.response == null) {
                  return Center(
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        errorImg,
                        verticalSpace(16),
                        const Text('인터넷 연결이 불안정해요.', style: textStyle16400999),
                        verticalSpace(7),
                        const Text('잠시 후 다시 시도해 주세요.',
                            style: textStyle14400CCC),
                        verticalSpace(16),
                        BtnTypeB(
                          type: ButtonTypeB.transparentMain,
                          text: '새로고침',
                          onPressed: () {
                            future.value = futureFunction();
                          },
                        ),
                        verticalSpace(120),
                      ],
                    ),
                  );
                }
                FirebaseCrashlytics.instance.recordError(
                  snapshot.error,
                  snapshot.stackTrace,
                  fatal: true,
                );
                return errorWidget;
              }
              FirebaseCrashlytics.instance.recordError(
                snapshot.error,
                snapshot.stackTrace,
                fatal: true,
              );
              return errorWidget;
            } else {
              return widget;
            }
          default:
            return Center(child: defaultProgressIndicator);
        }
      },
    ),
  );
}
```

## 기능

### flutter_native_splash: ^2.3.9 (7458)
기본 앱(네이티브 앱)이 플러터를 로드하는 동안 스플래쉬

* http: ^1.2.1 - HTTP 요청을 위한 API입니다. (7439)

* url_launcher: ^6.2.5 - 웹브라우저, 메일, 전화, 문자를 실행 (7034)

* dio: ^5.4.2+1 - HTTP 요청을 할 때 유용 (6824)

```dart
dio.post('/path', data: 객체);
```
Http 요청을 할때 data(body)에 객체를 넣으면 알아서 직렬화를 해주는 듯 하다. 또한 응답 data도 Map객체로 바꿔주는 듯 하다.

* flutter_launcher_icons: ^0.13.1 - 앱 아이콘 자동생성 (6697)

  **문제**

  안드로이드 앱 아이콘이 원형으로 바뀌면서 위 라이브러리 사용 시 아이콘이 확장되서 보인다.

  **해결**

  1. flutter_launcher_icons.yaml에서 adaptive_icon_background, adaptive_icon_foreground로 아이콘 생성

  2. android/app/src/main/res/mipmap-anydpi-v26/launcher_icon.xml 파일을 다음과 같이 수정

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <adaptive-icon xmlns:android="http://schemas.android.com/apk/res/android">
    <background android:drawable="@color/ic_launcher_background"/>
   <foreground>
      <inset
        android:drawable="@drawable/ic_launcher_foreground"
        android:inset="16%" />
    </foreground>
  </adaptive-icon>
  ```

  https://github.com/fluttercommunity/flutter_launcher_icons/issues/96#issuecomment-1325387518


* flutter_local_notifications: ^17.1.0 - 알림 보내기. firebase_messaging이랑 비슷한 기능인 듯? (6091)

* flutter_slidable - 전화번호부에서 좌우로 슬라이드 할 때 통화 or 메세지를 보내는 기능과 비슷 (5073)

* permission_handler: ^11.3.1 - 런타임 환경에서 사용자에게 앱 기능을 위한 권한 요청 (4846)

* path_provider: ^2.1.2 - 일반적으로 사용되는 파일 경로 찾기. 플랫폼에 가장 적합한 디렉토리를 찾는 데 도움. flutter_downloader와 같이 사용하는 예시 https://become-a-developer.tistory.com/119 (4604)

* file_picker: ^8.0.0+1 - 기본 파일 탐색기를 사용하여 단일 또는 여러 파일을 선택할 수 있다 (4009)

* json_serializable 6.8.0 - (3336) - 클래스에 주석을 달아 JSON과의 변환을 위한 코드를 자동으로 생성.

  *  json_annotation: ^4.9.0 - (1022)

* infinite_scroll_pagination: ^4.0.0 - 무한 스크롤 + Lazily load (3114)

* connectivity_plus: ^6.0.3 - Android 및 iOS에서 네트워크(WiFi 및 모바일/셀룰러) 연결 상태를 확인 (3059)

* share_plus: ^9.0.0 - Android에서는 ACTION_SEND 인텐트를 사용하고 iOS에서는 UIActivityViewController를 사용하여 플랫폼 공유 UI를 통해 콘텐츠를 공유(?) (3019)

### logger: ^2.2.0 (2874)

로거

### pull_to_refresh: ^2.0.0 (2653)

#### 에러 기록

StatelessWidget에 컨트롤러를 생성하고 생성자에 const를 떼고 써봤다. 실제기기에서 멈추는 현상이 있었다.

* device_info_plus: ^10.1.0 - Flutter 애플리케이션 내에서 현재 장치 정보 얻기 (2273)

* flutter_inappwebview: ^6.0.0 - 인앱 브라우저 띄우기 (2193)

* uuid: ^4.4.0 - 강력한 암호화, 비암호화 난수 생성 (2151)

* package_info_plus: ^8.0.0 - 애플리케이션 패키지에 대한 정보를 쿼리하기 위한 API를 제공 (2090)

* flutter_html: ^2.2.1 - html css를 랜더링 하는 플러터 위젯 (1877)

* upgrader: ^10.2.0 - 스토어에 최신 버전의 앱이 있을 때 사용자에게 업그레이드하라는 메시지를 표시하는 Flutter 패키지 (1849)

* crypto: ^3.0.3 - SHA, MD5 및 HMAC 암호화 기능 구현 (1536)

* uni_links: ^0.5.1 - App/Deep Links (Android), Universal Links and Custom URL schemes (iOS) 링크 허용 (1501)

* flutter_downloader: ^1.11.7 - 백그라운드에서 다운로드 받기 (1389)

* path: ^1.9.0 - 문자열 기반 경로 조작. Web에서 자주 쓰는듯? (1387)

* geocoding: ^3.0.0 - 주소를 위도 및 경도 좌표로 변환 or 위도와 경도 좌표를 주소로 변환 (1053)

* in_app_update: ^4.2.2 - 공식 Android API를 사용하여 Android에서 앱 내 업데이트 체크 및 업데이트 기능. IOS는 지원하지 않는다. 이와 비슷한 upgrader도 있다 (978)

* scroll_to_index: ^3.0.1 - 스크롤 가능한 위젯의 특정 하위 항목으로 스크롤 (724)

* app_links: ^4.0.1 - Android 앱 링크, 딥 링크, iOS 범용 링크 및 맞춤 URL (518)

* string_validator: ^1.0.2 - 문자열, 특히 사용자 입력의 문자열을 검증하고 삭제 (355)

* flutter_sms: ^2.3.3 - iOS 및 Android에서 SMS 및 MMS를 보내기 (334)

* version: ^3.0.2 - 버전을 비교하기 위해 비교 가능한 Version 객체를 만들 수 있다 (230)

* pointer_interceptor: ^0.10.1 - 하위 HtmlElementViews에서 클릭이 swallow되는 것을 방지하기 위한 위젯 (196)

* regexpattern: ^2.6.0 - 문자열 검증을 위한 정규식 패턴을 모아놨다 (120)

* mutex: ^3.1.0 - mutex(?) (87)

* flutter_web_frame: ^1.0.0 - 웹이나 데스크탑과 같은 큰 장치에서 프레임의 너비/크기 제한. 이는 애플리케이션이 여러 장치/플랫폼에서 실행될 때 반응형 버전을 아직 사용할 수 없는 경우 콘텐츠 크기를 제한하는 데 사용하기에 매우 적합. (60)

* visibility_aware_state: ^1.0.5 - 사용자가 화면을 보고있는지 확인(?) (33)

* flutter_exit_app: ^1.1.2 - exit(0)를 호출하지 않고 앱을 종료하는 가장 좋은 방법을 제공. (28)

* string_to_hex: ^0.2.2 - (18) String or/and Hash 를 HEX로 변환

## SDK, Plug in

* firebase_auth: ^4.19.4 - Firebase Auth plugin (3701)

* firebase_messaging: ^14.9.1 - Firebase Cloud Messaging plugin (3413)

* firebase_core: ^2.30.1 - firebase core plugin (3331)

* firebase_analytics: ^10.10.4 - firebase analytics plugin (1100)

* firebase_crashlytics: ^3.5.4 - firebase crashlytics plugin (1079)

* firebase_dynamic_links: ^5.5.4 - Dynamic Links plugin. 2025년 8월 25에 지원 중단 (738)

* firebase_remote_config: ^4.4.4 - firebase remote config plugin (510)

* google_sign_in: ^6.2.1 - A Flutter plugin for Google Sign In (2863)

* kakao_flutter_sdk: ^1.9.1+2 - Kakao SDK (140)

* flutter_naver_login: ^1.8.0 - Naver Login SDK (62)