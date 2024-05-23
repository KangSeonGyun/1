---
title: Flutter Libraries
tags: flutter
---

메타정보만 간단히 기록

## 라이브러리..

### 상태관리

* provider: ^6.1.2 - 상태관리 패키지 (9690)

  * 화면 일부분만 reBuild하려고 써봤다. StatefulBuilder 혹은 Stateful과 GlobalKey로 하는 방법도 있는 듯 하다. https://vintageappmaker.tistory.com/510

  * Provider.of<T>(context) 는 watch와 유사하게 동작
  * Provider.of<T>(context, listen: false) 는 read와 유사하게 동작

* stacked: ^3.4.0 - MVVM 구조를 쉽게 만들어주는 라이브러리 (1388)

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

* stacked_services: ^1.1.0 - stacked 라이브러리. NavigationService, DialogService, SnackbarService, BottomSheetService 제공 (261)

* stacked_shared: ^1.4.0 - stacked 패키지 간의 공유 코드 모음 (2)

### 저장

* shared_preferences: ^2.2.2 - 간단판 내용을 파일로 저장해두고 앱을 시작할 때 파일을 읽어온다. 디바이스 디스크의 데이터가 유지된다는 보장이 없으므로 중요한 데이터 저장은 권장하지 않는다 (8883)

* hive: ^2.2.3 - 기기의 내부저장소를 사용하는 키-값 데이터베이스 (5552)

* sqflite: ^2.3.3 -  SQLite 플러그인. 모든 폰에는 SQLite 데이터베이스가 있다. 복잡한 데이터를 기기에 저장할 수 있다. (4614)

### Carousel, Slide, 이미지 관련

* cached_network_image: ^3.3.1 - 인터넷 이미지 표시 및 캐시 디렉터리에 보관 (5719)

* carousel_slider: ^4.2.1 - card_swiper 라이브러리와 비슷하다 (5078)

* flutter_svg: ^2.0.9 - svg 라이브러리 (4804)

* photo_view: ^0.15.0 - 이미지 슬라이드 및 터치하여 이미지 확대 가능 (2762)

  <details>
  <summary>무한스크롤 PhotoViewGallery</summary>
  <div markdown="1">

  사진이 1개 일때도 무한스크롤이 되니 이걸 원하지 않으면 따로 처리를 해야한다

  ```dart
  class CustomPhotoView extends StatefulWidget {
    const CustomPhotoView(
        {super.key, required this.images, required this.itemIndex});
    // 이미지 주소만 String 배열로 받는다
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
          scrollPhysics: const BouncingScrollPhysics(),
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

* flutter_image_compress: ^2.2.0 이미지를 압축하여 전송. minWidth, minHeight를 지정하면 이미지 비율은 깨지지 않은채 내가 지정한 이미지 크기와 최대한 비슷하게 맞춰준다 (1320)

* card_swiper: ^3.0.1 - 무한루프의 Swiper/Carousel (1030)

* easy_image_viewer: ^1.4.1 - 풀 화면에서 두 손가락으로 줌 인, 줌 아웃, 여러 이미지 스와이프 가능 (231)

* flutter_image_slideshow: ^0.1.6 - 간단한 이미지 슬라이드쇼 위젯입니다. 주로 이미지 위젯용이지만 다른 위젯도 사용할 수 있다 (163)

### 디자인, 레이아웃, 위젯

* auto_size_text: ^3.0.0 - 지정한 범위 내에 완벽하게 맞도록 텍스트 크기를 자동으로 조정 (4381)

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

### 애니메이션

* animations: ^2.0.11 - 사용자 정의 애니메이션을 만들거나 미리 만들어진 애니메이션을 사용할 수 있다 (5789)

* animated_text_kit: ^4.2.2 - 텍스트에 애니메이션 효과 주기 (4763)

* animate_do: ^3.3.4 - 다양한 애니메이션 효과들을 줄 수 있다 (4068)

* loading_animation_widget: ^1.2.1 - 로딩 중 애니메이션 표시 (1353)

* lottie: ^3.1.0 - Lottie란? JSON 기반의 애니메이션 파일. 용량이 작고, 기기 호환성이 높으며, 크기를 조정하도 해상도가 낮아지지 않는다 (3579)

* animated_theme_switcher: ^2.0.10 - 애니매이션 효과주면서 테마 바꾸기 (442)

### 다국어, 현지화

* intl: ^0.19.0 - 날짜/숫자 형식 지정. 다국어 지원. (5072)

* flutter_localization: ^0.2.0 - Flutter SDK flutter_localizations 먼저 참고하자. 인앱 현지화 (189)

### 기능

* flutter_native_splash: ^2.3.9 - 기본 앱(네이티브 앱)이 플러터를 로드하는 동안 스플래쉬 (7458)

* http: ^1.2.1 - HTTP 요청을 위한 API입니다. (7439)

* url_launcher: ^6.2.5 - 웹브라우저, 메일, 전화, 문자를 실행 (7034)

* dio: ^5.4.2+1 - HTTP 요청을 할 때 유용 (6824)

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

* logger: ^2.2.0 - 로거 (2874)

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

### SDK, Plug in

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