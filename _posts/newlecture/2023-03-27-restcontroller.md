---
title: RestController
tags: java
---

## RestController

단순히 css, 값만 변경될 때 url을 수정하고 page를 다시 요청하면 서버에서 page를 만들어 클라이언트에게 오기까지 시간이 걸리며 page가 새로고침 된다.   
요즘은 rest방식으로 값만 가져와 page를 다시 요청하지 않고 javascript로 값만 바꾼다.

값을 가져올 때 View는 목록을 위해서만 만들어라 단일값을 가져올 땐 각자 가져오자. 값 하나가 필요해도 레코드 단위로 가져와서 getter를 사용하자.

Rest 방식은 기존 방식과 달리 url에도 차이가 있다.

**@Controller -> 문서명**   
/menu/list   
/menu/detail?id=3

**@RestController -> data**   
/menus		method:GET 메뉴 전체 줘   
/menus/3	method:GET(get) 3번 메뉴 줘   
/menus/3	method:DELETE(delete)   
/menus/3	method:PUT(edit)   
/menus		method:POST(insert)   

web이 지원하는 메소드는 많으나 html이 지원하는 메소드는 get, post뿐이다

```java
	@GetMapping("/menus/{id}")
	public String get(
			@PathVariable("id") int menuId) {
		return "menu " + menuId;
	}
```

메소드의 인자는 쿼리스트링에서 찾는다. 쿼리스트링이 아닌 url에 있는 값을 받고 싶을 수 있다.   
url의 경로의 id를 메소드의 파라미터 menuId로 받는 방법은 @PathVariable이다. return은 url경로의 id가 출력된다.

```java
	@GetMapping("/menus/{id}")
	public Menu get(
			@PathVariable("id") int id) {
		Menu menu = service.getById(id);
		return menu;
	}
```
	
원래는 반환형식을 객체(Menu)로 던지면 안 되지만 spring이 알아서 객체를 json형태로 반환한다.

```java
	@PutMapping
	public String edit(
		@PathVariable("id") int id) {
		return "menu edit:" + id;
	}
```

script는 브라우저 내에서만 이용가능하다 브라우저 밖으로 벗어나는 일은 금기 였다.   
com component는 모든 언어에서 이용가능했다. javascript가 이걸로 windows library를 이용해 밖에서 데이터를 가져올 수 있게됐다.   
com component가 activeX다.   
javascript를 사용하는 브라우저가 동기와 비동기를 지원한다.

```js
window.addEventListener("load", function() {
	let ul = this.document.querySelector(".menu-category>ul");

	ul.onclick = function(e) {
		//	function만 지역화. 람다는 지역화가 안된다?
		
		e.preventDefault();
		//	preventDefault. a링크와 onclick 이벤트가 겹쳤다. a링크를 클릭해도 페이지를 다시 로드하지 않는다
		
		let tagName = e.target.tagName;
		//	tagName은 태그명을 대문자로 보여준다
		
		if (tagName != "LI" && tagName != 'A')
			return;
		//	LI 나 A태그를 클릭하지 않으면 if 후에 코드를 실행하지 않는다

		const request = new XMLHttpRequest();
		request.open("GET", "http://localhost:8080/menus", true);
		//	브라우저에서 url을 요청하는 것과 같다
		//	비동기형(true)이 기본이다. false는 동기형

		request.send();
		//	동기형은 send 후 응답올때까지 아무것도 못한다. 응답이 온 뒤 아래 console.log가 실행된다.
		
		console.log(request.responseText);
		//	비동기형은 아직 응답이 오지 않아도 console.log가 실행되므로 빈 텍스트가 찍힐 수 있다. 응답이 오기전에 다른 행동을 할 수 있다
		//	request.responseText - 응답이 온 text는 JSON형식으로 온다
	};
```

```js
		const request = new XMLHttpRequest();
		request.onload = function(){
			console.log(request.responseText);
		}
		
		request.open("GET", "http://localhost:8080/menus", true);
		request.send();
```

비동기형의 문제점 해결. 응답이 온 뒤에 console.log를 찍는다. 콜백함수

## rest로 데이터를 요청하고 이용하는 연습
	
```js
window.addEventListener("load", function() {
	const menuList = document.querySelector(".menu-list");
	let ul = document.querySelector(".menu-category>ul");
	const form = document.querySelector(".search-header form");
	const findButton = form.querySelector(".icon-find");

	findButton.onclick = function(e) {
		// findButton을 클릭했을 때

		e.preventDefault();
		//	태그에서 발생하는 기본행위 submit을 막는다

		const queryInput = form.querySelector("input[name=q]");
		let query = queryInput.value;
		//	text input박스의 입력값을 가져왔다

		const request = new XMLHttpRequest();
		request.onload = function() {
			let menus = JSON.parse(request.responseText);
			bind(menus);
			// bind로 공통화
		};

		request.open("GET", `http://localhost:8080/menus?q=${query}`, true);
		request.send();
	}



	ul.onclick = function(e) {

		e.preventDefault();

		let tagName = e.target.tagName;
		// tagName은 LI 혹은 A여야 한다
		// li태그가 클릭됐다면 LI가 저장되며 a태그를 클릭됐다면 A를 저장한다

		if (tagName != 'LI' && tagName != 'A')
			return;

		// --------------------------------------------------------

		// 데이터 수집을 해야함
		let elLi = (tagName === 'LI') ? e.target : e.target.parentNode;
		// LI가 클릭됐다면 e.target은 li다. <li data-cid="4"> <a href="list?c=4">쿠키</a> </li>
		// A가 클릭 a의 부모 li다. <li data-cid="4"> <a href="list?c=4">쿠키</a> </li>
		// li와 a어떤것을 클릭해도 결국 elLi에는 li가 선택된다 

		//	elLi.className = "menu-selected"
		//	이렇게 class를 넣으면 안된다. 다른 class가 존재할 수 있기 때문

		let curLi = ul.querySelector("li.menu-selected");

		if (curLi === elLi)
			return;
		// 현재 menu-selected가 있는 li와 내가 클릭한 li가 같을 땐 data도 요청하지 않는다

		curLi.classList.remove("menu-selected");
		elLi.classList.add("menu-selected");
		//	현재 menu-selected가 있는 li와 내가 클릭한 li가 같지 않을 때 class 수정
		//	클릭한 li에 선택됐다는 효과의 class 추가

		// --------------------------------------------------------

		let categoryId = elLi.dataset.cid;
		// th:attr="data-cid=${c.id}"
		// 타임리프를 통해 html의 li에 dataset을 심어줬다.
		// 클릭한 li의 data-cid(카테고리 아이디)를 가져온다

		const request = new XMLHttpRequest();
		request.onload = function() {
			
			let menus = JSON.parse(request.responseText);
			//	JSON 형식으로 온 text를 배열로 만들었다.
			bind(menus);

		};
		request.open("GET", `http://localhost:8080/menus?c=${categoryId}`, true);
		// ''가 아닌 ``backtick 이다
		request.send();
	};

	function bind(menus) {
		//	카테고리 클릭시 메뉴 하나를 지우는 방법
		//	menuList.children[0].remove();
		//	menuList.firstElementChild.remove();
		//	menuList.removeChild(menuList.firstElementChild);

		//	카테고리 클릭시 메뉴 전체를 지우는 방법
		//	while (menuList.firstElementChild)
		//		menuList.firstElementChild.remove();
		//	모든 자식이 지워지며 자식이 없다면 null을 반환한다. Thuthy, falsy 기억하자

		//	카테고리 클릭시 텍스트를 추가하는 방법
		//	menuList.textContent = "<span style='color:blue;'>text</span>";
		//	menuList.innerText = "<span style='color:blue;'>text</span>";
		//	menuList.innerHTML = "<span style='color:blue;'>text</span>";
		//	첫 번째, 두 번째는 span태그가 text로 전부 출력되지만 마지막은 style이 적용된 text가 나온다

		//	카테고리 클릭시 메뉴 전체를 지우는 또다른 방법
		menuList.innerHTML = "";

		// ------------------------------------------------------------

		//	목록 만들어 채우기
		//	1. DOM 객체를 만들어 추가
		let menuSection = document.createElement("section");
		menuSection.className = "menu";
		//	엘리먼트를 만들고 class를 부여했다

		let form = document.createElement("form");
		form.className = "";
		//	엘리먼트를 만들고 class를 부여했다. 하지만 class는 없다

		//	menuSection.appendChild(form); // Node interface의 기능
		menuSection.append(form); // Element interface의 기능
		//	section과 form을 합치는 방법 두가지.

		menuList.append(menuSection);
		//	menuList에 menuSection을 붙였다

		// ------------------------------------------------------------

		//	목록 만들어 채우기
		//	2. 문자열을 이용한 객체 생성

		for (let m of menus) {
			var template = `<section class="menu">
			            <!-- getViewList()로 생성된 10개 메뉴 리스트가 만큼 메뉴가 출력된다 -->
			                <form class="">
			                    <h1><span>${m.name}</span>/<span>${m.categoryName}</span></h1> 
			                    <div class="menu-img-box">
			                        <a href="detail?id=${m.id}"> <img class="menu-img" src="/image/product/${m.img}"></a>
			                        <!-- url에 쿼리스트링으로 menu의 id가 나온다 -->
			                    </div>    
			                    <div class="menu-price">${m.price}</div>
			                    <div class="menu-option-list">
			                        <span class="menu-option">
			                            <input class="menu-option-input" type="checkbox">
			                            <label>ICED</label>
			                        </span>            
			                        <span class="menu-option ml-2">
			                            <input class="menu-option-input" type="checkbox">
			                            <label>Large</label>
			                        </span>
			                    </div>
			                    <div class="menu-button-list">
			                        <input class="btn btn-fill btn-size-1 btn-size-1-lg" type="submit" value="담기">
			                        <input class="btn btn-line btn-size-1 btn-size-1-lg ml-1" type="submit" value="주문하기">
			                    </div>
			                </form>
			            </section>`;
			//	''는 줄 바꿈이 있는 문자열을 변수에 넣을 수 없다
			//	``은 변수에 여러줄의 문자열을 넣을 수 있다

			//	menuList.innerHTML += template;
			//	메뉴 하나를 html문서에 출력했다. 하지만 이렇게 하면 안된다
			//	문자열을 누적하면 오버헤드가 일어난다.

			menuList.insertAdjacentHTML("beforeend", template);
			//	template를 누적하는 좋은 예시
			//	beforeend는 section 뒤에 section을 누적하기 위한 옵션이다. 검색해보자
			//	성능면에서 체감할 수 있을정도로 엄청난 차이가 난다.
		}

	}

});
```


	
	
	