---
title: Back단에서의 인증과 권한
tags: vue javascript
---

/admin으로 시작하는 url처럼 아무나 접근할 수 없는 url이 있다.
인증과 권한을 라이브러리 없이 만들어 볼 것이다.

### 세션 ID 발급

우선 세션 ID/쿠키를 이용해서 사용자가 인증되었던 적이 있는지 확인한다.
문서를 요청하고 받게되면 세션 ID가 생긴다. 다음부턴 이 세선 ID를 이용해 인증된 사용자인지 확인한다.
이 세션 ID는 일정시간(30분 or 변경가능)이 지나면 파기된다.
Spring을 통해 세션 ID를 부여하는 방법은 다음과 같다.

```java
@Controller("adminHomeController")
@RequestMapping("admin")
public class HomeController {

	@GetMapping("index")
	public String index(HttpServletRequest request) {
		request.getSession().setAttribute("test", "kkk");
		
		return "admin/index";
	}
}
```

첫 /admin/index를 요청하면 Response Headers에서 Set-Cookie를 확인할 수 있다.   
그후 다시 /admin/index를 요청하면 Request Headers에서 Cookie를 확인할 수 있다.

세션 ID 확인방법 : 브라우저 개발자 도구 → Network → 요청한 문서 클릭

### 권한 확인

세션 ID가 잘 발급 되었다면 다음과 같은 과정이 필요하다.

1. 로그인 여부를 확인하여 로그인이 되지 않았으면 로그인 페이지르 보낸다.

```java
		if(session.getAttribute("isAuth")==null)
			return "redirect:/uesr/login";
//		if(너 로그인 했니?)
//		아니라면 로그인 페이지(/uesr/login)로 보낸다.
```
2. 로그인 한 계정의 권한이 admin인지 확인해야 한다.


### 인증