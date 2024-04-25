---
title: Flutter Classes
tags: flutter
---

## 클래스들..

* Scaffold

  * extendBody: true - BottomNavigationBar의 모서리가 둥글게 깍이며 약간의 틈이 생겼다. body의 콘텐츠가 BottomNavigationBar밑에 깔리길 원했다.  

  * resizeToAvoidBottomInset: false - 키보드로 인해 bottom overflow가 발생해서 설정한 옵션. 이 설정을 하지 않아도 ScrollView면 overflow가 발생하지 않는다

* Appbar - kToolbarHeight 앱바 기본 높이
  ```dart
    scrolledUnderElevation: 0 // 스크롤 시 앱바 흐려지는 효과 없애기
  ```
* IntrinsicHeight - 자식 위젯의 높이를 자식 위젯들 중 가장 큰 높이에 맞춰주는 역할

* NestedScrollView - 한 화면에 같은 방향의 스크롤이 2개이상일 때 각각 스크롤 되는 것이 아닌 같이 스크롤 되길 원할 때 사용. TabBarView를 구현할 때 자주 쓰임

  <details>
  <summary>SliverOverlapAbsorber/SliverOverlapInjector</summary>
  <div markdown="1">
  pinned효과 등..으로 고정되어 있는 위젯(Appbar, SliverPersistentHeader 등)을 SliverOverlapAbsorber로 겹쳐지는 위젯으로 처리 할 수 있다. SliverOverlapInjector로 그 고정되어 있는 위젯의 크기를 삽입할 수 있다.     
  
  즉, "headerSliverBuilder"가 다음 Sliver와 겹치지 않는 위젯만 빌드하는 경우에는 이 작업이 필요하지 않다.   
  
  * SliverOverlapAbsorber와 SizedBox만 이용해 Sub SliverPersistentHeader를 고정시킨 예시 https://honor-driven.dev/flutter%EB%A1%9C-shopping-%EC%95%B1-%EB%A7%8C%EB%93%A4%EA%B8%B0-3-nestedscrollview-cec18854f859  
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

  </div>
  </details>
   

* CustomScrollView - 슬리버를 사용하여 사용자 정의 스크롤 효과를 생성하는 ScrollView 위젯.

  * NestedScrollView와 아주 비슷하다.

  * slivers: [] 바로 밑이 아닌 SliverToBoxAdapter → GridView 이와 같은 구조라면 전체 스크롤 공유가 안된다.

* Sliver란? 스크롤 가능한 영역

  * SliverMainAxisGroup - 고정되어야 할 서브 앱바가 있는 경우 유용할 듯 하다

  * SliverPrototypeExtentList - 필수 속성인 prototypeItem에 위젯을 주면 이 위젯의 크기로 리스트들의 크기가 정해지는 듯 하다

  * SliverPersistentHeader - 앱바처럼 고정된 헤더를 만들 수 있다.

    * minExtent 값이 너무 작으면 overflow가 발생하니 주의하자

    * shouldRebuild()를 false로 반환하면 다른 곳에서 상태를 변경해도 이 헤더는 변하지 않는다.

  * DecoratedSliver - sliver를 자식으로 갖는 꾸미기 박스
  

* Chip - Material Design 3
  ```dart
    // 칩 기본 패딩을 없애려고 해본 것
    visualDensity: const VisualDensity(horizontal: 0.0, vertical: -4),
    labelPadding: const EdgeInsets.all(0),
    // 칩 기본 마진을 없애려고 해본 것
    materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
  ```

* TextField, TextFormField

  * 화면 터치시 키보드 내려가게 하기
    ```dart
    onTapOutside: (event) => FocusManager.instance.primaryFocus?.unfocus(),
    ```

* AnnotatedRegion<SystemUiOverlayStyle\> - 앱의 시스템 UI(상태 표시줄, 네비게이션 바) 오버레이 스타일 지정
  * SafeArea 적용하고 각 페이지 별로 시스템 UI 색상이 다르다면 SafeArea의 top: false, bottom: false 속성을 이용해보자

* OverflowBox - 부모 위젯보다 더 큰 높이를 주면 Overflow를 발생시킨다. 이상하게 위로 넘치게 하고 싶으면 alignment: Alignment.bottomCenter를 줘야 했다

* InputDecoration

  **문제**
  
  suffixIcon에 IconButton을 넣고 IconButton의 constraints을 조정해도 아무효과가 없었다.

  **해결**

  알고보니 InputDecoration에도 constraints가 있었다.


