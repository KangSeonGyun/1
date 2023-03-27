---
title: Rest
tags: java
---

rest는 html이 아닌 javaspript가 이 값들을 이용한다.

처음 page를 블러올 땐 @controller를 이용하고 이후 값만 바꿀 땐 @RestController를 이용한다

	@GetMapping("/menus/{id}")
	public String get(
			@PathVariable("id") int menuId) {
		return "menu " + menuId;
	}
	
메소드의 인자는 쿼리스트링에서 찾는다. 쿼리스트링이 아닌 url에 있는 값을 받고 싶을 수 있다.   
url의 경로의 id를 메소드의 파라미터 menuId로 받는 방법은 @PathVariable이다. return은 url경로의 id가 출력된다.

	@GetMapping("/menus/{id}")
	public Menu get(
			@PathVariable("id") int id) {
		Menu menu = service.getById(id);
		return menu;
	}
	
원래는 반환형식을 객체(Menu)로 던지면 안 되지만 spring이 알아서 객체를 json형태로 반환한다.

	@PutMapping
	public String edit(
		@PathVariable("id") int id) {
		return "menu edit:" + id;
	}
	


script는 브라우저 내에서만 이용가능하다 브라우저 밖으로 벗어나는 일은 금기 였다.
com component는 모든 언어에서 이용가능했다. javascript가 이걸로 windows library를 이용해 밖에서 데이터를 가져올 수 있게됐다.
com component가 activeX다.
javascript를 사용하는 브라우저가 동기와 비동기를 지원한다


window.addEventListener("load", function() {
	let ul = this.document.querySelector(".menu-category>ul");

	ul.onclick = function(e) {
		e.preventDefault();
		let tagName = e.target.tagName;
		if (tagName != "LI" && tagName != 'A')
			return;

		const request = new XMLHttpRequest();
		request.open("GET", "http://localhost:8080/menus", true);
		//	브라우저에서 url을 요청하는 것과 같다
		//	비동기형(true)이 기본이다. false는 동기형

		request.send();
		//	동기형은 send 후 응답올때까지 아무것도 못한다. 응답이 온 뒤 아래 console.log가 실행된다
		console.log(request.responseText);
		//	비동기형은 아직 응답이 오지 않았음에도 console.log가 실행되므로 빈 텍스트가 찍힌다. 응답이 오기전에 다른 행동을 할 수 있다
	};
	
	//	function만 지역화. 람다는 지역화가 안된다?
	//	tagName은 태그명을 대문자로 보여준다
	//	preventDefault. a링크와 onclick 이벤트가 겹쳤다. a링크를 클릭해도 페이지를 다시 로드하지 않는다

	//	LI 나 A태그를 클릭하지 않으면 if 후에 코드를 실행하지 않는다

	-----------------------------------------------------
	
		const request = new XMLHttpRequest();
		request.onload = function(){
			console.log(request.responseText);
		}
		
		request.open("GET", "http://localhost:8080/menus", true);
		request.send();
		
		비동기형의 문제점 해결. 응답이 온 뒤에 console.log를 찍는다. 콜백함수
	-----------------------------------------------------
	

       
        
        ----------------------------admin menu list 3/27

        
        	@GetMapping("list")
	public String list(
			@RequestParam(name = "p", defaultValue = "1") int page,
			@RequestParam(name="c", required = false) Integer categoryId,
			@RequestParam(name = "q", required = false) String query,
			Model model
			) throws UnsupportedEncodingException {

		
		List<MenuView> list = service.getViewList(page, categoryId, query);
		model.addAttribute("list", list);

		return "admin/menu/list";

	}
	
	-----------------------------------------------------------------
	
	window.addEventListener("load", function () {
    const menuList = document.querySelector(".menu-list");
    let ul = document.querySelector(".menu-category>ul");
    ul.onclick = function (e) {

        e.preventDefault();

        let tagName = e.target.tagName;

        if (tagName != 'LI' && tagName != 'A')
            return;

        // 데이터 수집을 해야함
        let elLi = (tagName === 'LI') ? e.target : e.target.parentNode;

        console.log(elLi.dataset.cid);
        // th:attr="data-cid=${c.id}"
        // 타임리프를 통해 html의 dataset을 심어줬다.

        let categoryId = elLi.dataset.cid;

        const request = new XMLHttpRequest();
        request.onload = function () {
            console.log(request.responseText);

            let menus = JSON.parse(request.responseText);

            // 목록을 지우는 방법
            // menuList.children[0].remove();
            // menuList.firstElementChild.remove();
            // menuList.removeChild(menuList.firstElementChild);

            // 목록을 지우는 방법2
            while (menuList.firstElementChild)
                menuList.firstElementChild.remove();

        }

        request.open("GET", 'http://localhost:8080/menus?c=${categoryId}', true);
        request.send();
        console.log(request.responseText);

    };
});
	
	
	