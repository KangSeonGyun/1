---
title: Spring Validation
tags: spring thymeleaf
---

/admin으로 시작하는 url처럼 아무나 접근할 수 없는 url이 있다.   
인증과 권한을 라이브러리 없이 만들어 볼 것이다.   
우선 세션에 대한 이해를 돕기위해 예제를 첨부했다.

## 세션 ID 발급

세션 ID/쿠키를 이용해서 사용자가 인증되었던 적이 있는지 확인한다.   
문서를 요청하고 받게되면 세션 ID가 생긴다. 다음부턴 이 세선 ID를 이용해 인증된 사용자인지 확인한다.   
이 세션 ID는 일정시간(30분 or 변경가능)이 지나면 파기된다.   
Spring을 통해 세션 ID를 부여하는 방법은 다음과 같다.   

```java
@Controller("adminHomeController")
@RequestMapping("admin")
public class HomeController {

	@GetMapping("index")
	public String index(HttpServletRequest request) {
		request.getSession().setAttribute("test", "ksg0000");
		
		return "admin/index";
	}
}
```

첫 /admin/index를 요청하면 Response Headers에서 Set-Cookie를 확인할 수 있다.   
그후 다시 /admin/index를 요청하면 Request Headers에서 Cookie를 확인할 수 있다.

세션 ID 확인방법 : 브라우저 개발자 도구 → Network → 요청한 문서 클릭

### 권한 확인

세션 ID가 잘 발급 되었다면 다음과 같은 과정이 필요하다.

1) 로그인 여부를 확인하여 로그인이 되지 않았으면 로그인 페이지르 보낸다.

```java
if(session.getAttribute("isAuth")==null)
	return "redirect:/uesr/login";
//	if(너 로그인 했니?)
//	아니라면 로그인 페이지(/uesr/login)로 보낸다.
```

2) 로그인 한 계정의 권한이 admin인지 확인해야 한다.

```java
if (session.getAttribute("isAuth") == null) {
    return "redirect:/user/login";
} else {
    // 권한 확인 로직 추가
    String userRole = (String) session.getAttribute("userRole");
    if (userRole.equals("admin")) {
        // 권한에 따른 처리
        // ...
    } else {
        // 권한이 없는 경우에 대한 처리
        // ...
    }
}
```

## 세션 부여

다음 예제는 [이전글](https://ksg0000.github.io/2023/04/10/spring-login.html)과 이어진다.

```java
@Controller
@RequestMapping("/user")
public class UesrController {

	@Autowired
	private MemberService memberservice;

	@GetMapping("login")
	public String login() {
		return "user/login";
	}

	@PostMapping("login")
	public String login(String uid, String pwd, String returnURL, HttpSession session) {

		boolean vaild = memberservice.isValid(uid, pwd);

		if (vaild) {
//			vaild가 true라는 것은 로그인에 성공한 경우이다.
		
			session.setAttribute("username", uid);
//			세션에 인증 받은 사용자의 아이디를 넣는다.
//			세션에 많을 것을 넣을 수록 서버에 부담이 간다.

			return "redirect:/index";
//			로그인 성공 시 index 페이지로 간다.
		}

		return "redirect:login?error=Login failed";
//		로그인 실패시 login 페이지로 가며 쿼리스트링으로 Login failed을 전달한다.
//		쿼리스트링은 다양한 방법으로 활용할 수 있다. 
	}

}
```

setAttribute로 세션을 부여했다.

```java
@Service
public class DefaultMemberService implements MemberService {

    @Autowired
    private MemberRepository repository;

    @Override
    public boolean isValid(String userName, String password) {
    
        Member member = repository.findByUserName(userName);

        if (member == null)
            return false;
        else if (!BCrypt.checkpw(password, member.getPwd()))
            return false;

        return true;
    }
}
```

위 코드는 서비스 단에서 userName과 password를 이용해 내 서비스에 등록된 사용자인지 알아내는 과정이다

```html
<h1 th:text="${param.error}"></h1>
```

쿼리스트링 error을 활용한 Thymeleaf 예제는 위 와 같다.   
파라미터(url의 쿼리스트링)을 통해 사용자 화면에 Login failed을 출력할 수 있다.

```html
<li><a th:if="${session.username}==null" href="/user/login">로그인</a></li>
<li><a th:if="${session.username}!=null" href="/user/logout">로그아웃</a></li>
```

session을 활용해 로그인과 로그아웃을 교차로 보여줄 수 있다.   
session username이 없다면(null 이라면)로그인을, session username이 있다면 로그아웃을 보여준다.

## returnURL

returnURL이란 사용자가 이전에 보던 페이지로 다시 돌아가기 위해 필요하다.

우리는 모두 다음과 같은 경험이 있을 것이다.

상품 내용을 보다가 장바구니에 담기를 눌렀는데 로그인이 되어있지 않다면 로그인 페이지로 넘어가고 로그인에 성공했을 때 내가 보고있던 상품 페이지로 돌아가는 경험이다.

이를 구현하는 방법은 다음과 같다.

```java
@Controller("adminHomeController")
@RequestMapping("admin")
public class HomeController {

	@GetMapping("index")
	public String index(HttpSession session) {
		
		if(session.getAttribute("uesrname") != null)
			return "redirect:/user/login?returnURL=/admin/index";
		
		return "admin/index";
	}
}
```

/admin/index 페이지를 요청할 때 무조건 로그인이 필요하다고 가정했다.   
session에서 username을 가져오며 username이 null이 아닐 경우(세션 username이 없는 경우)에 /user/login로 보내며 쿼리스트링에 returnURL=/admin/index를 같이 보낸다.

이후 /user/login이 매핑되어 있는 UserController를 다음과 같이 수정하면 된다.

```java
		if (isVaild) {
			session.setAttribute("username", uid);

			if (returnURL != null)
				return "redirect:" + returnURL;
//			쿼리스트링에 returnURL이 있다면 해당 URL로 간다

			return "redirect:/index";
//			로그인 성공했지만 returnURL이 없다면 index 페이지로 간다.
		}
```

하지만 이렇게 구현하면 모든 returnURL이 필요한 redirect요청에 ?returnURL=/admin/index를 붙여줘야 한다.   

returnURL은 같은 내용이 단순 반복이므로 다음글에서 Filter를 이용해 집중화, 공통화가 필요하다
