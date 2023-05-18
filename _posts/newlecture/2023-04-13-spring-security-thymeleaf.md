---
title: Thymeleaf에서의 Spring Security
tags: spring thymeleaf
---

## thymeleaf에서 spring security 사용

```xml
		<dependency>
			<groupId>org.thymeleaf.extras</groupId>
			<artifactId>thymeleaf-extras-springsecurity6</artifactId>
		</dependency>
```

pom.xml에서 dependency 추가. 맨뒤 숫자 6은 현재 사용 중인 spring 버전이다

```html
<html lang="en"
	xmlns:th="http://www.thymeleaf.org"
	xmlns:sec="http://www.thymeleaf.org/extras/spring-security6"
>
```

Html(view)에 위와 같이 xml namespace를 추가한다.

```html
<li sec:authorize="isAnonymous()"><a href="/user/login">로그인</a></li>
<li sec:authorize="isAuthenticated()"><a href="/user/logout">로그아웃</a></li>
```

로그인 여부에 따라 로그인, 로그아웃을 교차로 보여준다.

[https://docs.spring.io/spring-security/reference/servlet/authorization/expression-based.html](https://docs.spring.io/spring-security/reference/servlet/authorization/expression-based.html)

[https://www.baeldung.com/spring-security-thymeleaf](https://www.baeldung.com/spring-security-thymeleaf)

참고 : [자바캔](https://javacan.tistory.com/entry/58)
