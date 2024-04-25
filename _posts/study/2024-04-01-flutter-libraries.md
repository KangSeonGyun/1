---
title: Flutter Libraries
tags: flutter
---

메타정보만 간단히 기록

## 라이브러리..

### 상태관리

* provider: ^6.1.2 - 상태관리 패키지 (9690)

  * 화면 일부분만 reBuild하려고 써봤다. StatefulBuilder 혹은 Stateful 과 GlobalKey로 하는 방법도 있는 듯 하다. https://vintageappmaker.tistory.com/510

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

### Carousel, Slide

* carousel_slider: ^4.2.1 - card_swiper 라이브러리와 비슷하다 (5078)

* card_swiper: ^3.0.1 - 무한루프의 Swiper/Carousel (1030)

* flutter_slidable - 전화번호부에서 좌우로 슬라이드 할 때 통화 or 메세지를 보내는 기능과 비슷 (5073)

### 디자인, 레이아웃, 위젯

* auto_size_text: ^3.0.0 - 지정한 범위 내에 완벽하게 맞도록 텍스트 크기를 자동으로 조정 (4381)

* table_calendar: ^3.1.1 - 달력 위젯 (2659)

* dropdown_button2: ^2.3.9 - 기본 DropdownButton은 item들의 높이가 kMinInteractiveDimension(48.0)으로 고정되어 있어서 위 라이브러리를 사용해봤다. DropdownButton을 다양하게 커스터마이징 할 수 있다. (1431)

* cupertino_icons: ^1.0.6 - Cupertino 위젯 에서 사용되는 기본 아이콘들 (766)

* stop_watch_timer: ^3.1.1 - 스톱워치 만들기 (248)

* easy_rich_text: ^2.1.0 - 첨자, Trademark 표현가능. targetString 혹은 정규식을 이용해 텍스트에 스타일을 바꾼다. (202)

* lazy_load_indexed_stack: ^1.1.0 - 메인 페이지 하단 내비바 및 탭구분 시 유용 (79)

* checkbox_grouped: ^2.0.0 - 체크박스 다중 선택, 전체 활성화 비활성화 등.. (41)

### 애니메이션

* animated_text_kit: ^4.2.2 - 텍스트에 애니메이션 효과 주기 (4763)

* animate_do: ^3.3.4 - 다양한 애니메이션 효과들을 줄 수 있다 (4068)

* lottie: ^3.1.0 - Lottie란? JSON 기반의 애니메이션 파일. 용량이 작고, 기기 호환성이 높으며, 크기를 조정하도 해상도가 낮아지지 않는다 (3579)

* animated_theme_switcher: ^2.0.10 - 애니매이션 효과주면서 테마 바꾸기 (442)

### 다국어, 현지화

* intl: ^0.19.0 - 날짜/숫자 형식 지정. 다국어 지원. (5072)

* flutter_localization: ^0.2.0 - Flutter SDK flutter_localizations 먼저 참고하자. 인앱 현지화 (189)

### SDK, Plug in

* kakao_flutter_sdk: ^1.9.1+2 - Kakao SDK (140)

* flutter_naver_login: ^1.8.0 - Naver Login SDK (62)

* google_sign_in: ^6.2.1 - A Flutter plugin for Google Sign In (2863)

### 기능

* flutter_native_splash: ^2.3.9 - 기본 앱(네이티브 앱)이 플러터를 로드하는 동안 스플래쉬 (7458)

* url_launcher: ^6.2.5 - 웹브라우저, 메일, 전화, 문자를 실행 (7034)

* dio: ^5.4.2+1 - HTTP 요청을 할 때 유용 (6824)

* permission_handler: ^11.3.1 - 런타임 환경에서 사용자에게 앱 기능을 위한 권한 요청 (4846)

* flutter_svg: ^2.0.9 - svg 라이브러리 (4804)

* path_provider: ^2.1.2 - 일반적으로 사용되는 파일 경로 찾기. 플랫폼에 가장 적합한 디렉토리를 찾는 데 도움. flutter_downloader와 같이 사용하는 예시 https://become-a-developer.tistory.com/119 (4604)

* file_picker: ^8.0.0+1 - 기본 파일 탐색기를 사용하여 단일 또는 여러 파일을 선택할 수 있다 (4009)

* infinite_scroll_pagination: ^4.0.0 - 무한 스크롤 + Lazily load (3114)

* logger: ^2.2.0 - 로거 (2874)

* device_info_plus: ^10.1.0 - Flutter 애플리케이션 내에서 현재 장치 정보 얻기 (2273)

* flutter_inappwebview: ^6.0.0 - 인앱 브라우저 띄우기 (2193)

* flutter_downloader: ^1.11.7 - 백그라운드에서 다운로드 받기 (1389)

* geocoding: ^3.0.0 - 주소를 위도 및 경도 좌표로 변환 or 위도와 경도 좌표를 주소로 변환 (1053)

* pointer_interceptor: ^0.10.1 - 하위 HtmlElementViews에서 클릭이 swallow되는 것을 방지하기 위한 위젯 (196)

* flutter_web_frame: ^1.0.0 - 웹이나 데스크탑과 같은 큰 장치에서 프레임의 너비/크기 제한. 이는 애플리케이션이 여러 장치/플랫폼에서 실행될 때 반응형 버전을 아직 사용할 수 없는 경우 콘텐츠 크기를 제한하는 데 사용하기에 매우 적합. (60)

* visibility_aware_state: ^1.0.5 - 사용자가 화면을 보고있는지 확인(?) (33)

