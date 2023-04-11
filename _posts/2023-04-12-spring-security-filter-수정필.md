---
title: Spring Security, Filter
tags: spring
---

#### Servlet Filter

Servlet Filter는 Servlet Container에서 HTTP 요청 및 응답에 대한 전처리 및 후처리 작업을 처리하는 클래스입니다. Servlet Filter는 일반적으로 웹 애플리케이션에서 반복적으로 수행되는 작업을 처리하거나, 보안, 로깅, 인증, 압축 등과 같은 기능을 추가하는 데 사용됩니다.   
Servlet Filter는 일반적으로 다른 서블릿 또는 JSP와 함께 사용되며, web.xml 또는 Java 코드에서 구성할 수 있습니다.

<img src="/assets/images/servlet-filter-default.jpg" title="참고 이미지" alt="이미지" />

**필터의 기본 구조**

AOP에서 등장하는 Proxy와 비슷하며. Filter는 서블릿계의 Proxy다.

라이브러리 없이 Filter를 구현해보려 한다.

```java
@Component
public class AuthFilter implements Filter {

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
			
		System.out.println("입구 필터가 실행되었습니다.");
	
	}
}
```

위와 같은 클래스를 선언하고 이전과 같이 서버를 켜보면 하얀 화면만 뜨면서 Spring Console엔 "입구 필터가 실행되었습니다." 한 줄이 찍힌 걸 볼 수 있다.   
브라우저 개발자 도구로 확인해보면 에러도 없고 문서도 잘 받아온걸 확인할 수 있다.

```java
@Component
public class AuthFilter implements Filter {

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {

		System.out.println("입구 필터가 실행되었습니다.");

		chain.doFilter(request, response);

		System.out.println("출구 필터가 실행되었습니다.");

	}
}
```

위와 같이 수정하면 페이지는 정상적으로 보이지만 Spring Console엔 "입구 필터가 실행되었습니다."와 "출구 필터가 실행되었습니다."가 엄청 많이 찍힌 걸 볼 수있다.   
이유는 image, css, html 등 모든게 Filter를 거쳐가기 때문이다.

ServletRequest : 요청 객체   
ServletResponse : 응답 객체   
FilterChain : 여러 개의 Filter를 체인 형태로 연결하여 하나의 요청(request)에 대해 여러 개의 Filter를 순차적으로 실행할 수 있도록 도와주며, FilterChain에 등록된 모든 Filter가 실행되었다면, 마지막으로 Servlet이 실행된다.

<img src="/assets/images/servlet-filter-chain.jpg" title="참고 이미지" alt="이미지" />

**필터 체인**

```java
@Component
public class AuthFilter implements Filter {

	private static final String[] authUrls = {
			"/admin/**", "/member/**"
	};
	
	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
			
		HttpServletRequest httpRequest = (HttpServletRequest) request;
		
		String uri = httpRequest.getRequestURI();
		
		String url = httpRequest.getRequestURL().toString();
		
		System.out.println("입구 필터가 실행되었습니다.");

		chain.doFilter(request, response);
		
		System.out.println("출구 필터가 실행되었습니다.");
	}
}
```
HttpServletRequest을 이용해 uri와 url도 가져올 수 있다.   

<details>
<summary>ServletRequest과 HttpServletRequest의 차이?</summary>
<div markdown="1">

ServletRequest과 HttpServletRequest는 모두 Java Servlet API에서 제공하는 인터페이스입니다. 그러나 HttpServletRequest는 ServletRequest를 상속한 자식 인터페이스입니다.

즉, HttpServletRequest는 ServletRequest 인터페이스에 추가적인 메서드와 기능을 제공합니다. HttpServletRequest는 HTTP 요청에 특화된 기능을 제공하는 반면, ServletRequest는 HTTP를 포함한 모든 요청에 대한 기능을 제공합니다.

HttpServletRequest는 HTTP 요청에 대한 다양한 정보와 기능을 제공하는 메서드들을 추가적으로 제공합니다. 예를 들어, getParameter(), getHeader(), getSession()과 같은 메서드는 HttpServletRequest에서만 제공됩니다.

또한, HttpServletRequest는 HTTP 요청에 대한 정보를 보다 쉽게 추출할 수 있도록 하기 위해 여러 메서드를 제공합니다. 예를 들어, getQueryString(), getRequestURI(), getContextPath()와 같은 메서드는 HTTP 요청의 URL 정보를 추출하는데 유용합니다.

따라서, ServletRequest는 모든 요청에 대한 인터페이스이며, HttpServletRequest는 HTTP 요청에 특화된 인터페이스입니다. 때문에 일반적으로 HTTP 프로토콜을 사용하는 웹 애플리케이션에서는 HttpServletRequest를 주로 사용합니다.

</div>
</details>

Filter에 대한 내용은 여기까지만 하고 라이브러리를 이용해 보자... 나중에 공부해보자.

#### Spring Security 라이브러리 이용

Filter는 xml(구 방식)로도 구현할 수 있지만 해당 예제는 Java로 구현했다.

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-test</artifactId>
	<scope>test</scope>
</dependency>
```
Spring Starters를 통해 간단히 추가할 수 있으며, pom.xml에 위와 같은 dependency가 추가 된다.

참고 : [자바캔](https://javacan.tistory.com/entry/58)
