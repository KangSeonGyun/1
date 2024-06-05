---
title: Flutter Classes
tags: flutter
---

## 클래스들..

### Scaffold

  * extendBody: true - BottomNavigationBar의 모서리가 둥글게 깍이며 약간의 틈이 생겼다. body의 콘텐츠가 BottomNavigationBar밑에 깔리길 원했다.  

  * resizeToAvoidBottomInset: false - 키보드로 인해 bottom overflow가 발생해서 설정한 옵션. 이 설정을 하지 않아도 ScrollView면 overflow가 발생하지 않는다

  * extendBodyBehindAppBar: true - 배경위에 겹치는 앱바 만들기

  * persistFooterButtons - 스크롤 되는 경우에도 지속적으로 표시되는 위젯. BottomNavigationBar 위, body 아래에 렌더링 됨

### Appbar - kToolbarHeight 앱바 기본 높이
  ```dart
    scrolledUnderElevation: 0 // 스크롤 시 앱바 흐려지는 효과 없애기
  ```
### IntrinsicHeight - 자식 위젯의 높이를 자식 위젯들 중 가장 큰 높이에 맞춰주는 역할

### NestedScrollView - 한 화면에 같은 방향의 스크롤이 2개이상일 때 각각 스크롤 되는 것이 아닌 같이 스크롤 되길 원할 때 사용. TabBarView를 구현할 때 자주 쓰임

  * SliverOverlapAbsorber/SliverOverlapInjector

    pinned효과 등..으로 고정되어 있는 위젯(Appbar, SliverPersistentHeader 등)을 SliverOverlapAbsorber로 겹쳐지는 위젯으로 처리 할 수 있다. SliverOverlapInjector로 그 고정되어 있는 위젯의 크기를 삽입할 수 있다.     
    
    즉, "headerSliverBuilder"가 다음 Sliver와 겹치지 않는 위젯만 빌드하는 경우에는 이 작업이 필요하지 않다.   
  
  * SliverOverlapAbsorber와 SizedBox만 이용해 Sub SliverPersistentHeader를 고정시킨 예시
  
    https://honor-driven.dev/flutter%EB%A1%9C-shopping-%EC%95%B1-%EB%A7%8C%EB%93%A4%EA%B8%B0-3-nestedscrollview-cec18854f859

    body: TabBarView()를 TabBarDelegate(SliverPersistentHeader)로 겹쳐지는 위젯으로 만든 뒤 안보이는 SizedBox를 겹쳐놔 TabBarView()goryBreadcrumbs(SliverPersistentHeader)도 상단에 고정되게 했다.
  
  * 나만의 다양한 시도 기록..

    최상단 고정되어야할 앱바는 Scaffold appBar로 고정

    Scaffold appBar와 고정되어야할 header를 갖고있는 List 사이에 위치해야할 내용은 headerSliverBuilder

    고정되어야할 header를 갖고있는 List는 body

    핵심은 _buildBody의 List를 Expanded로 감싸는 것이다.

    Expanded가 남는 부분만 가져가므로 List위에 있는 Row 두 줄이 Scaffold appBar 밑 화면 상단에 고정된 효과를 낼 수 있다.

    문제점 - headerSliverBuilder 내용이 많아서 screenHeight를 넘어갔을 때 body Column이 일부분만 보일경우 overflow가 발생했다

    ```dart
      Scaffold(
        appBar: // 최상단 pinned 앱바 제작
        body: NestedScrollView(
          headerSliverBuilder: (BuildContext context, bool innerBoxIsScrolled) {
            return [
              // 최상단 앱바와 고정되어야할 Sub Widget 사이 내용들...
            ];
          },
          body: _buildBody() // 고정되어야할 header를 갖고있는 List
        ),
      );

    Widget _buildBody() {
      return Padding(
        padding: const EdgeInsets.fromLTRB(16, 16, 16, 32),
        child: Column(
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                const Text(
                  '검색',
                  style: TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.w600,
                    color: black,
                  ),
                ),
                InkWell(
                  child: const Icon(Icons.search),
                  onTap: () {},
                ),
              ],
            ),
            verticalSpace10,
            Row(
              mainAxisAlignment: MainAxisAlignment.start,
              children: [
                Container(
                  height: 30,
                  constraints: const BoxConstraints(minWidth: 70, maxWidth: 100),
                  decoration: BoxDecoration(
                    border: Border.all(color: const Color(0xFFCCCCCC)),
                    borderRadius: BorderRadius.circular(4),
                  ),
                  child: DropdownButton(
                    padding: const EdgeInsets.only(left: 6),
                    hint: const Text(
                      '힌트',
                      style: TextStyle(
                        fontSize: 14,
                        fontWeight: FontWeight.w400,
                        color: gray666,
                      ),
                    ),
                    value: null,
                    itemHeight: 48, // 최소 권장값
                    iconEnabledColor: const Color(0xFFCCCCCC),
                    items: [
                      ...List.generate(
                        items.length,
                        (index) => DropdownMenuItem(
                          value: index,
                          child: Text(
                            items[index],
                            // items는 원래 코드에서 인수로 넘겨줬었다.
                          ),
                        ),
                        growable: false,
                      )
                    ],
                    underline: const SizedBox(),
                    onChanged: ((v) {
                      // 생략
                    }),
                  ),
                ), 
              ],
            ),
            verticalSpace24,
            Expanded(
              child: GridView.builder(
                gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                    crossAxisCount: 2, crossAxisSpacing: 10, mainAxisSpacing: 10),
                itemBuilder: (context, index) => Container(
                  decoration: BoxDecoration(
                      border: Border.all(color: black, width: 1),
                      color: Colors.transparent),
                ),
              ),
            )
          ],
        ),
      );
    }
    ```

    SliverPersistentHeader - 얘를 사용하는게 좋아보이나.. minExtent, maxExtent로 헤더 크기를 지정해줘야 하는 것이 싫다. 자식 컨텐츠 크기와 패딩으로 조절됐으면 좋겠는데.. 방법을 찾아봐야 겠다.

  * 또다른 문제..

    **문제**

    NestedScrollView → body → TabBarView에 첫 번째 탭에는 상단에 고정되어야할 헤더가 있고, 두 번째 탭에는 상단에 고정되어야할 헤더가 없는 경우

    TabBarView에 SliverPersistentHeader를 넣으면 앱바 밑에 깔리는 등 제대로 고정되지 않았다.

    **해결**

    NestedScrollView → headerSliverBuilder에 첫 번째 탭 헤더도 같이 넣고 첫 번째 탭일 때만 보여주게 했다.

  * 또다른 문제..

    **문제**

    AppBar와 TabBar 사이에 다른 위젯들을 넣어야했고, AppBar의 bottom 속성에 TabBar를 만들지 못했다.
    또한 TabBarView의 리스트가 다른 상단에 고정된 위젯들 밑에 깔리는 등 문제가 발생했다. 이와 비슷하다. https://github.com/flutter/flutter/issues/22393

    **해결**

    AppBar bottom속성에 TabBar가 있는 경우 body의 TabBarView와 CustomScrollView의 범위가 AppBar를 **포함**한 처음부터 남은화면 까지 였다.
    
    AppBar bottom속성에 TabBar가 있는 경우가 아닌 사이에 다른 위젯들이 있는경우, body의 TabBarView와 CustomScrollView의 범위가 AppBar를 **제외**한 TabBar부터 남은 화면까지 였다. SliverOverlapAbsorber에 pinned: true로 고정된 위젯(SliverAppBar, SliverPersistentHeader)이 아래로 스크롤 중 다른 고정된 위젯을 만나면 SliverOverlapAbsorber가 그 크키만큼 커졌다.

    ```dart
    @override
    Widget build(BuildContext context) {
      SliverOverlapAbsorberHandle appBar = SliverOverlapAbsorberHandle();

      return Scaffold(
        body: NestedScrollView(
          controller: scrollController,
          headerSliverBuilder: (context, innerBoxIsScrolled) {
            return [
              SliverOverlapAbsorber(
                handle: appBar,
                sliver: // 슬리버 앱바. 앱바도 투명배경이라 겹쳐야 해서 SliverOverlapAbsorber로 겹치기만 했다.
              ),
              // 슬리버들...
              SliverOverlapAbsorber(
                handle: NestedScrollView.sliverOverlapAbsorberHandleFor(context),
                sliver: MultiSliver(
                  children: [
                    // 슬리버들...

                    // 상단에 고정되어야 할 위젯은 SliverAppBar, SliverPersistentHeader 및 속성을 pinned: true로 만들었다
                  ],
                ),
              ),
              // 슬리버들...
            ];
          },
          body: TabBarView(
            controller: tabController,
            children: [
              Builder(
                builder: (context) {
                  return CustomScrollView(
                    slivers: [
                      SliverOverlapInjector(
                        handle: NestedScrollView.sliverOverlapAbsorberHandleFor(
                            context),
                      ),
                      // tab0에 위치할 다른 슬리버들..
                    ],
                    key: const PageStorageKey<String>('tab0'),
                  );
                },
              ),
              Builder(
                builder: (context) {
                  return CustomScrollView(
                    shrinkWrap: true,
                    slivers: [
                      SliverOverlapInjector(
                        handle: NestedScrollView.sliverOverlapAbsorberHandleFor(
                            context),
                      ),
                      // tab1에 위치할 다른 슬리버들..
                    ],
                    key: const PageStorageKey<String>('tab1'),
                  );
                },
              ),
            ],
          ),
        ),
      );
    }
    ```

### CustomScrollView - 슬리버를 사용하여 사용자 정의 스크롤 효과를 생성하는 ScrollView 위젯.

  * NestedScrollView와 아주 비슷하다.

  * slivers: [] 바로 밑이 아닌 SliverToBoxAdapter → GridView 이와 같은 구조라면 전체 스크롤 공유가 안된다.

### Sliver란? 스크롤 가능한 영역

  * SliverMainAxisGroup - 고정되어야 할 서브 앱바가 있는 경우 유용할 듯 하다

  * SliverPrototypeExtentList - 필수 속성인 prototypeItem에 위젯을 주면 이 위젯의 크기로 리스트들의 크기가 정해지는 듯 하다

  * SliverPersistentHeader - 앱바처럼 고정된 헤더를 만들 수 있다.

    * minExtent 값이 너무 작으면 overflow가 발생하니 주의하자

    * shouldRebuild()를 false로 반환하면 다른 곳에서 상태를 변경해도 이 헤더는 변하지 않는다.

  * DecoratedSliver - sliver를 자식으로 갖는 꾸미기 박스
  

### Chip - Material Design 3
  ```dart
    // 칩 기본 패딩을 없애려고 해본 것
    visualDensity: const VisualDensity(horizontal: 0.0, vertical: -4),
    labelPadding: const EdgeInsets.all(0),
    // 칩 기본 마진을 없애려고 해본 것
    materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
  ```

### TextField, TextFormField

  * 화면 터치시 키보드 내려가게 하기

    ```dart
    onTapOutside: (event) => FocusManager.instance.primaryFocus?.unfocus(),
    ```

  * 여러 TextField, TextFormField에서 완료를 누르거나, 포커스를 잃을 경우 validate

    * onFieldSubmitted, key, FocusNode()가 필요할 듯 하다.

### AnnotatedRegion<SystemUiOverlayStyle\> - 앱의 시스템 UI(상태 표시줄, 네비게이션 바) 오버레이 스타일 지정

  * 위 방법 외에도 SafeArea 적용하고 각 페이지 별로 시스템 UI 색상이 다르다면 SafeArea의 top: false, bottom: false 속성을 이용해보자

  **문제**

  * SafeArea로 인해 bottomNavigationBar 밑에 Scaffold 배경색이 들어갔다

  **해결**

  ```dart
    bottomNavigationBar: Container(
                          color: Color...,
                          child: SafeArea(
                            child: ...
  ```

  https://stackoverflow.com/questions/53873270/flutter-background-of-unsafe-area-in-safearea-widget

### OverflowBox - 부모 위젯보다 더 큰 높이를 주면 Overflow를 발생시킨다. 이상하게 위로 넘치게 하고 싶으면 alignment: Alignment.bottomCenter를 줘야 했다

### TextButton

  **문제**

  * 버튼의 height를 48보다 작게하고 싶었다

  **해결**

  padding. minimumSize, tapTargetSize을 조정하자

  ```
  TextButton(
    onPressed: () => Navigator.pop(context),
    style: TextButton.styleFrom(
        padding: EdgeInsets.zero,
        minimumSize: Size.zero,
        tapTargetSize: MaterialTapTargetSize.shrinkWrap,
        alignment: Alignment.centerLeft),
    child: Icon(
      CupertinoIcons.back,
      color: Colors.black,
      size: 18,
    ),
  ),
  ```

  https://stackoverflow.com/questions/52628215/flutter-remove-padding-in-buttons-flatbutton-elevatedbutton-outlinedbutton

### InputDecoration

  **문제**
  
  TextField를 꾸미던 중 suffixIcon에 IconButton을 넣고 IconButton의 constraints을 조정해도 아무효과가 없었다.

  **해결**

  알고보니 InputDecoration에도 constraints가 있었다.

### InkWell

  **문제**

  InkWell을 적용했을 때 잉크 퍼지는 효과는 보이지 않고, onTap()메소드는 정상작동 했다.

  **해결**

  https://api.flutter.dev/flutter/material/InkWell-class.html

  InkWell은 Material바로 위에 효과를 보여주므로 Material과 InkWell 사이에 다른 색깔이 있는 위젯이 있으면 잉크 퍼지는 효과는 보이지않는다.

  ```dart
  Material(
    // color: Colors.teal[900],
    child: Ink(
      color: Colors.blue,
      child: InkWell(
      onTap: () {},
        child: Container(
          // color: ,
          child: Icon(
            Icons.abc,
            size: 32,
          ),
        ),
      ),
    ),
  ),
  ```

  여기서 포인트는 Container 위젯에 color를 주지 않는 것이다. Container의 색상을 InkWell의 위(Ink)로 올려 InkWell효과가 보인다.

  Ink 위젯을 생략하고 Material의 color를 줘도 동일한 효과가 나왔다.

### Row

  **문제**

  Row에서 Expanded를 하면 가로방향(width)는 확장되지만 세로방향(height)는 확장되지 않았다.

  난 박스 2도 높이를 지정하지 않고 박스 1과 같은 높이를 갖길 원했다

  ```dart
  // 예시코드
  Row(
    children: [
      Expanded(
        child: Container(
          height: 50,
          width: 50,
          color: Colors.red,
          child: const Text("박스 1"),
        ),
      ),
      Expanded(
        child: Container(
          width: 50,
          color: Colors.blue,
          child: const Text("박스 2"),
        ),
      ),
    ],
  ),
  ```

  **해결**

  IntrinsicHeight를 사용하면 된다고 한다.

  하지만 IntrinsicHeight는 비용이 많이 든다고 하니 두 번째 답변인 Table을 사용하는 게 좋아보인다.

  https://stackoverflow.com/questions/51326170/flutter-layout-row-column-share-width-expand-height

### ExpansionTile과 ExpansionPanelList, ExpansionPanel

  하위 항목을 표시하거나 숨기기 위해 타일을 확장하거나 축소하는 확장 화살표 아이콘이 있는 위젯

  * ExpansionPanelList.radio - 한 번에 하나의 패널만 열리는 위젯

### RadioListTile

  **기본 패딩 지우기 (Radio도 해당)**

  contentPadding, visualDensity 활용

  ```dart
    contentPadding: const EdgeInsets.symmetric(
      horizontal: 4,
      vertical: 4,
    ),
    visualDensity: const VisualDensity(
      horizontal: VisualDensity.minimumDensity,
      vertical: VisualDensity.minimumDensity,
    ),
  ```

  https://stackoverflow.com/questions/50317072/how-can-i-remove-internal-padding-on-a-radiolisttile-so-i-can-use-3-radiolisttil

### Column

  **문제**

  다중 위젯(childern)을 동시에 정렬할 수 있는건 Column, Row 뿐인가? Column, Row 사용 시 차지할 수 있는 공간이 있다면 전부 차지해 버리는 문제가 있다.

  **해결**
  
  mainAxisSize: MainAxisSize.min 속성을 이용해 보자

### Table

### 함수 showModalBottomSheet()

  **문제**

  backgroundColor에 흰색을 줘도 약간 흐린 흰색이 나왔다

  **해결**

  elevation: 0

  **문제**

  모달 내부 상태값 변경

  **해결**

  StatefulBuilder

  https://stackoverflow.com/questions/52414629/how-to-update-state-of-a-modalbottomsheet-in-flutter

### Wrap

  runSpacing - 교차 축 간격

### Divider

  DashedDivider

  ```dart
  class MySeparator extends StatelessWidget {
    const MySeparator({Key? key, this.height = 1, this.color = Colors.black})
        : super(key: key);
    final double height;
    final Color color;

    @override
    Widget build(BuildContext context) {
      return LayoutBuilder(
        builder: (BuildContext context, BoxConstraints constraints) {
          final boxWidth = constraints.constrainWidth();
          const dashWidth = 10.0;
          final dashHeight = height;
          final dashCount = (boxWidth / (2 * dashWidth)).floor();
          return Flex(
            children: List.generate(dashCount, (_) {
              return SizedBox(
                width: dashWidth,
                height: dashHeight,
                child: DecoratedBox(
                  decoration: BoxDecoration(color: color),
                ),
              );
            }),
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            direction: Axis.horizontal,
          );
        },
      );
    }
  }
  ```

  https://stackoverflow.com/questions/54019785/how-to-add-line-dash-in-flutter

### Transform

  stack widget 없이 겹치려고 사용했다. 하지만 얘는 100px 상자를 위로 20px올리면 아래공간은 유지되므로 총 120px 공간을 차지한다.

  ```dart
    Transform.translate(
      offset: const Offset(0, -20),
      child: ... 
  ```

### Semantics

  위젯의 의미에 대한 설명으로 위젯 트리에 주석을 추가하는 위젯. 보조 기술, 검색 엔진 및 기타 의미 분석 소프트웨어에서 애플리케이션의 의미를 결정하는 데 사용

### ListView

  **문제**

  가로방향 스크롤을 만드려고 scrollDirection: Axis.horizontal을 사용하니 ListView의 높이를 정해줘야 했고, 그 높이를 벗어나는 위젯 혹은 InkWell 효과는 화면에서 짤렸다.

  **해결**

  clipBehavior: Clip.none, ListView외에 다른 위젯에서도 짤리는 일이 있다면 이 속성을 기억하자  


### Stack

  이미지 위에 Stack, Positioned를 이용해 Container를 겹치고 이미지의 width와 Container의 width를 동일하게 하고 싶어서 width: double.infinity를 했으나 에러가 났다. 신기하게 bottom: 0 만 해서는 안되고 left, right도 0으로 해줘야 에러가 안났다.

  ```dart
  Positioned(
  bottom: 0,
  left: 0,
  right: 0,
  child: Container(
    width: double.infinity,
    padding: const EdgeInsets.fromLTRB(18, 5, 0, 3),
    decoration: BoxDecoration(
        color: Colors.white60,
        borderRadius: BorderRadius.circular(12)),
      child: Text(
        '텍스트',
      ),
    ),
  ),
  ```

  파트너가 곤충업체 업체마다 갖고있는 카테고리가 다르다.

  이미지가 커져야 한다.