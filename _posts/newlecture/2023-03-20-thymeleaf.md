---
title: Thymeleaf layout
tags: java
---

## 기초 사용법

```
th:text="메뉴"
```
단순 text 메뉴가 출력된다.

```
th:text="${menu.name}"
```
모델에 추가된 menu객체의 name속성이 출력된다. menu객체의 name속성에는 getter가 있어야 한다.   
view단에서는 getter가 필요하지만 MyBatis Mapper엔 getter가 없어도 속성명이 일치하면 됐다.

```
th:each="menu : ${list}"
```
list 배열에 있는 객체 하나하나를 menu라는 이름으로 뽑아 쓸 수 있다.

```
th:utext="${menu.description}"
```
예를들어 menu.description의 값이 <span style="color:red;font-weight:bold;">중요</span>일 때 텍스트 그대로 출력되는 것이 아닌 html, css속성이 적용된 채로 출력된다.

```
th:text="${#numbers.formatInteger(menu.price,3,'COMMA')} + '원'"
```
html에 있는 "4,500원"을 th:text="${menu.price}"로 대체할 경우가 있다.   
menu.price의 값이 서식이 있는 4,500원이 아닌 4500일 경우 서식을 붙여주는 방법이다.

```
th:href="@{detail(id=${menu.id}, n=${menu.name})}"
```
@{detail}에 쿼리스트링(id=${menu.id})을 붙인걸로 생각하자.   
?(쿼리스트링)를 쓸 경우.

```
th:href="@{detail(id=${param.id}, op=lg)}"
```
op가 고정값(lg)이다.

```
th:src="'/image/menu/' + ${menu.img}"   
th:src="@{/image/menu/{img}(img=${menu.img})}"
```
?를 쓰지 않을 경우. ()안에 있는 img=${menu.img}가 {img}를 채운다.

```
th:class="(${param.c}==${c.id})? 'menu-selected'"
```
3항연산자. 조건이 참이면 .menu-selected 스타일을 추가한다. 하지만 위와같이 비교하면 정확한 비교가 되지 않는다.

```
th:class="${#strings.equals(param.c, c.id)}? 'menu-selected'"
th:class="${not #strings.equals(param.c, c.id)}? 'menu-selected'"
```
정확한 비교 방법. not을 붙이면 반대되는 값이 나온다.

```
<input type="hidden" name="c" th:value="${param.c}">
<input type="text" name="q" th:value="${param.q}">
```
두 번째 text input에 검색을 하면 이전 쿼리스트링이 날아가며 검색한 text도 날아간다.   
c값을 저장하기 위해 보이지 않는 input으로 c를 입력한다.   
검색을 한다면 input에 입력한 텍스트가 쿼리스트링으로 전달되며 input에는 텍스트가 사라진다. value를 통해 쿼리스트링에서 q를 가져온다.

```
th:classappend="d-none"
```
기존 class를 대체하지 않고 class에 d-none을 추가한다

```
?c=1&p=
?c=1
```
결과는 같으나 첫 번째 쿼리스트링은 p에 빈 문자열이 전달된다. p를 검사해야 하므로 성능이 느려진다.   
두 번째는 p가 없으므로 null이다.

```
?c=1&op=df
?c=1
```
op(option)을 생략하는 방식으로 default를 구현하거나 값을 명시해서 op=df(default)로 구현할 수 있다.

Circular View Path Error(Spring Thymeleaf view resolver Error)   
https://www.baeldung.com/spring-circular-view-path-error

## layout

```
<jsp:include page="../menu/list.jsp"></jsp:include>
```

jsp 초창기에는 액션 태그로 header footer 등을 불러왔다.   
하지만 여러 페이지가 생길 때 include가 반복됐다.

그 후 tile 라이브러리가 생겼다.

Thymeleaf를 사용하는 나는 Thymeleaf의 기능을 이용해서 페이지 분리를 하려고 한다.

### layout.html

```html
<html
	xmlns="http://www.thymeleaf.org"
	xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
>

<head>
<!-- 모든 page의 css가 위치하는 공간 -->
</head>

<body>
    
  	<!-- header 부분 -->
  	<div th:replace="~{inc/header}"></div>
  	<!-- 경로를 이용해 가져오기 때문에 각 header.html footer.html에서는 thymeleaf를 쓰지 않았다 -->
  	<!-- replace 들어갈 곳을 div태그로 찜한다 -->
  	<!-- 뭘 가져오려면 태그(div)가 필요하다 / div 태그가 남아있다. -->
  	
  	<!-- replace 대신 insert도 사용가능 -->
  	<!-- <div th:insert="~{index::header}"></div> -->
  	<!-- 다른 html에서 th:fragment="header"를 하고 위 코드로 그 header를 가져올 수 있다 -->
  	<!-- 하지만 header태그 여러개라면 전부 가져온다 -->
  	<!-- replace와 달리 div 태그가 남지 않는다 -->
  	
  	<!-- ---------------------------------------------------------------------- -->   
  	
  	<!-- main 부분 -->
  	<div layout:fragment="main"></div>
  	
    <!-- ---------------------------------------------------------------------- -->   
	
  	<!-- footer 부분 -->
  	<div th:replace="~{inc/footer}"></div>

</body>
```

공통적으로 들어가야 할 것은 layout.html에 세팅을 해두고 각 page엔 main태그로 묶인 부분만 남겨둔다.   
그 후 각 page에서 다음과 같이 layout.html을 불러오는 것

```html

<html lang="en"
	xmlns="http://www.thymeleaf.org"
	xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
	layout:decorate="inc/layout"
>

<main layout:fragment="main">

```