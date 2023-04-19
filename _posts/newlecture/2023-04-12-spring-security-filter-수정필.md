---
title: Spring Security, Filter
tags: spring
---

## Servlet Filter

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

## Spring Security 라이브러리 이용

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

```java
@Configuration
public class RlandSecurityConfig {

}
```

RlandSecurityConfig 클래스 생성 후 Bean 담기

```java
	@Bean
	public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
//		HttpSecurity는 import할 때 패키지명을 보면 알 수 있듯이 보안 설정을 위한 Builder다
//		Filter가 로그인이 안 되었으면 로그인 페이지로 보내며 권한도 확인해준다

		http
			.authorizeHttpRequests()
				.requestMatchers("/admin/**").hasAnyRole("ADMIN")
				.requestMatchers("/member/**").hasAnyRole("ADMIN", "MEMBER")
				.anyRequest().permitAll();
				
		return http.build();
	}
				
//		hasAnyAuthority()를 쓸 때 파라미터가 꼭 ROLE_로 시작해야 하는 규칙이 있다. 예) ROLE_ADMIN
//		hasAnyRole()은 과거 ROLE_을 꼭 붙여야하는 규칙을 생략해도 된다. 예) ADMIN

//		/admin/**은 ADMIN만
//		/member/** ADMIN과 MEMBER만
//		나머지는 전부 허용
```

/admin 혹은 /member에 들어가면 403 error, 세션 부여됨

//	빌더란?
//	Lombok에서도 빌더가 있다 all argu 필요
//	setter가 아닌 오버로드 생성자를 통해 만듦

```java
//	사용자 데이터 서비스
//	1. 인메모리 서비스 - 내가 정의한 사용자 리스트?
//	3. LDAP 서비스 - 총무부만 따로 사용?
	@Bean
	public UserDetailsService userDetailsService() {
		UserDetails newlec = User
				.builder()
				.username("newlec")
				.password("111")
				.roles("ADMIN", "MEMBER")
				.build();
		
		UserDetails dragon = User
				.builder()
				.username("dragon")
				.password("000")
				.roles("MEMBER")
				.build();
		
		return new InMemoryUserDetailsManager(newlec, dragon);
	}
```

이래도 403에러

```java
		http
			.authorizeHttpRequests()
				.requestMatchers("/admin/**").hasAnyRole("ADMIN")
				.requestMatchers("/member/**").hasAnyRole("ADMIN", "MEMBER")
				.anyRequest().permitAll()
			.and()
				.formLogin();
```

.and() .formLogin(); 이걸 해줘야 로그인 화면이 뜬다 사용자가 로그인 폼을 만들지 않으면 기본 폼을 제공한다.
이래도 newlec 111을 입력하면 500에러
There is no PasswordEncoder mapped for the id "null"
PasswordEncoder가 필요하다

//	양방향, 단방향 암호화 ?
//	원문을 알면 hash값을 알아 낼 수 있짐나 hash값으로 원문을 만들어 낼 순 없다.

```java
	@Bean
	public PasswordEncoder passwordEncoder() {
		
		return new BCryptPasswordEncoder();	
	}
```

예제에선 BCryptPasswordEncoder 사용
이래도 newlec 111로 로그인이 안된다.
왜냐하면 111을 암호화하면 $2a$10$H.HXCEd59CmUnp9luiKHwestJxuSIGRsJZNzCWfTfpJPgk6VGLm3O
password를 위와 같이 수정

엄청큰 파일을 암호화해도 같은 길이의 암호가 만들어진다. 단순 식별자 이기 때문

```java
		UserDetails newlec = User
				.builder()
				.username("newlec")
				.password("$2a$10$H.HXCEd59CmUnp9luiKHwestJxuSIGRsJZNzCWfTfpJPgk6VGLm3O")
				.roles("ADMIN", "MEMBER")
				.build();
```

newlec의 password를 암호화된 문자열로 수정
newlec 111을 입력하면 로그인 성공
왜냐하면 사용자가 입력한 111을 입력하면 $2a$10$H.HXCEd59CmUnp9luiKHwestJxuSIGRsJZNzCWfTfpJPgk6VGLm3O로 암호화 되며 password가 일치한 것이다.

```java
		UserDetails newlec = User
				.builder()
				.username("newlec")
				.password(passwordEncoder().encode("111"))
				.roles("ADMIN", "MEMBER")
				.build();
```

하지만 암호화된 문자열을 미리 알 수 없으므로 위와 같이 수정
이로써 사용자가 뭘 입력하는 지는 궁금하지않고 암호화된 문자열이 같으면 비밀번호가 일치한 것이다.

```java
		http
			.authorizeHttpRequests()
				.requestMatchers("/admin/**").hasAnyRole("ADMIN")
				.requestMatchers("/member/**").hasAnyRole("ADMIN", "MEMBER")
				.anyRequest().permitAll()
			.and()
				.formLogin()
				.loginPage("/user/login");
```

스프링이 기본으로 제공해주는 로그인 페이지가 아닌 내가 만든 로그인 페이지 사용
이러고 newlec 111 로그인 시도하면 403에러

```java
		http
			.cors()
			.disable()
			.authorizeHttpRequests()
				.requestMatchers("/admin/**").hasAnyRole("ADMIN")
				.requestMatchers("/member/**").hasAnyRole("ADMIN", "MEMBER")
				.anyRequest().permitAll()
			.and()
				.formLogin()
				.loginPage("/user/login");
```

CORS 공격에 대한 대비가 없다?
same Origin인지 확인할 수 있는 Key가 없다. 혹은 cors 끄기
하지만 이래도 newlec 111 로그인 시도하면 403에러
왜냐하면 /user/login 로그인하면 /user/login로 Post요청이 간다.
스프링은 /login으로 post요청을 보낼 것 이지만 내가만든 post요청을 쓰려면 알려줘야 한다???? 스프링이 로그인 Post로직을 구현하고 있기 때문

```java
		http
			.cors()
			.disable()
			.authorizeHttpRequests()
				.requestMatchers("/admin/**").hasAnyRole("ADMIN")
				.requestMatchers("/member/**").hasAnyRole("ADMIN", "MEMBER")
				.anyRequest().permitAll()
			.and()
				.formLogin()
				.loginPage("/user/login")
				.loginProcessingUrl("/user/login");
```

loginProcessingUrl에 매핑된 /user/login으로 Post요청이오면 스프링이 구현하고 있는 로그인 로직을 사용한다는 의미다.
따라서 form action속성과 맞춰 주기만 하면 막 써도 괜찮다.
기존에 내가 만들어 놓은 PostMapping /user/login이 있다면 무시된다.

이래도 403에러 CSRF설정이 필요하다. 버전에 따라 cors만 설정해도 되는 경우가 있다
//	cors csrf 알아보자

```java
		http
			.cors().and()
			.csrf().disable()
			.authorizeHttpRequests()
				.requestMatchers("/admin/**").hasAnyRole("ADMIN")
				.requestMatchers("/member/**").hasAnyRole("ADMIN", "MEMBER")
				.anyRequest().permitAll()
			.and()
				.formLogin()
				.loginPage("/user/login")
				.loginProcessingUrl("/user/login");
```

만약 html에서 form태그 input의 name 속성이 스프링이 정한 username, password가 아니라 사용자가 임의로 정한 uid, pwd일 경우 로그인 되지 않는다

```java
		http
			.cors().and()
			.csrf().disable()
			.authorizeHttpRequests()
				.requestMatchers("/admin/**").hasAnyRole("ADMIN")
				.requestMatchers("/member/**").hasAnyRole("ADMIN", "MEMBER")
				.anyRequest().permitAll()
			.and()
				.formLogin()
				.loginPage("/user/login")
				.loginProcessingUrl("/user/login")
				.defaultSuccessUrl("/admin/index");
```

html을 고쳤다면 로그인이 된다 defaultSuccessUrl는 로그인 성공시 이동할 url이다.


## Logout

```java
	@RequestMapping("logout")
	public String logout(HttpSession session) {
		
		session.invalidate();
		
		return "redirect:/index";
	}
```
이 요청도 무시된다. Spring이 구현.

```java
		http
			.cors()
			.and()
				.csrf().disable()
				.authorizeHttpRequests()
					.requestMatchers("/admin/**").hasAnyRole("ADMIN")
					.requestMatchers("/member/**").hasAnyRole("ADMIN", "MEMBER")
					.anyRequest().permitAll()
			.and()
				.formLogin()
					.loginPage("/user/login")
					.loginProcessingUrl("/user/login")
					.defaultSuccessUrl("/admin/index")
			.and()
				.logout()
					.logoutUrl("/user/logout")
					.logoutSuccessUrl("/index");
```

## 사용자 데이터 서비스

```java
@Autowired
private DataSource dataSource;
```

```java
//	2. JDBC 서비스
	@Bean
	public UserDetailsService jdbcUserDetailsService() {
		
		JdbcUserDetailsManager manager = new JdbcUserDetailsManager(dataSource);
//		application.properties에 DB정보(datasource)가 정의되어 콩자루에 담겨있다.
		
		manager.setUsersByUsernameQuery("select username, pwd password, 1 enabled from member where username=?");
//		로그인 시도한 사람의 계정, 패스워드, enabled(활성화, 비활성화 여부)만 가져와야한다.
//		우리 DB Table은 활성화, 비활성화 여부를 저장하지 않아 고정값 1로 했다.
		
		manager.setAuthoritiesByUsernameQuery("SELECT username, 'ROLE_ADMIN' authority from member where username=?");
//		member Table에 권한 정보가 없다고 가정하고 고정값 ROLE_ADMIN를 넣었다.
		

		return manager;
	}
```

## 메뉴 등록 요청을 보낼 때 username을 얻는 방법

세션을 이용하는 것은 올바르지 않다?

1

```java
	@PostMapping("reg")
	public String reg(String title) {
//		1.
		Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
		String currentPrincipalName = authentication.getName();
		System.out.println(currentPrincipalName);
		
		return "redirect:list";
	}
```

2

```java
	@PostMapping("reg")
	public String reg(
			String title,
			Authentication authentication) {
//		2.
		String userName = authentication.getName();
		
		System.out.println(authentication.getPrincipal());
		System.out.println(userName);
		
		return "redirect:list";
	}
```

3

```java
	@PostMapping("reg")
	public String reg(
			String title,
			Authentication authentication) {
		
		UserDetails user = (UserDetails) authentication.getPrincipal();
//		getPrincipal() 이건 유저 정보(UserDetails)를 준다. username password(보호됨), enabled, 권한 등을 알 수 있다
//		형 변환만 해주면 된다.

		System.out.println(user.getUsername());
		
		System.out.println(user.getPassword());
//		이건 null이 뜬다
		
		return "redirect:list";
	}
```

4

```java

	@PostMapping("reg")
	public String reg(
			String title,
			Principal principal) {
		
		System.out.println(principal.getName());
		
		return "redirect:list";
	}
```

## 알랜드 만의 UserDetails 만들기

```java
public class RlandUserDetails extends User implements UserDetails {

}
```

User를 상속받아도 된다

```java
@Data
@ToString
public class RlandUserDetails implements UserDetails {
//	Rland만의 UserDetails 그릇

	private Long id;
	private String email;
	private String username;
	private String password;
	private List<GrantedAuthority> authorities;
	
	@Override
	public Collection<? extends GrantedAuthority> getAuthorities() {
		// TODO Auto-generated method stub
		return authorities;
	}

	@Override
	public String getPassword() {
		// TODO Auto-generated method stub
		return password;
	}

	@Override
	public String getUsername() {
		// TODO Auto-generated method stub
		return username;
	}

	@Override
	public boolean isAccountNonExpired() {
		// TODO Auto-generated method stub
		return true;
	}

	@Override
	public boolean isAccountNonLocked() {
		// TODO Auto-generated method stub
		return true;
	}

	@Override
	public boolean isCredentialsNonExpired() {
		// TODO Auto-generated method stub
		return true;
	}

	@Override
	public boolean isEnabled() {
		// TODO Auto-generated method stub
		return true;
	}

}
```

```java
//@Service
public class RlandUserDetailsService implements UserDetailsService {

	@Autowired
	private MemberRepository repository;
	
	
	@Override
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		
//		RlandUserDetails 그릇에 담을 데이터 준비
		Member member = repository.findByUesrname(username);
		List<GrantedAuthority> authorities = new ArrayList<>();
		authorities.add(new SimpleGrantedAuthority("ROLE_ADMIN"));
		
//		데이터가 준비 되었으면 이제 RlandUserDetails 그릇 객체를 만들어서 데이터 반환해주면 끝
		RlandUserDetails user = new RlandUserDetails();
		user.setId(member.getId());
		user.setUsername(member.getUserName());
		user.setPassword(member.getPwd());
		user.setEmail(member.getEmail());
		user.setAuthorities(authorities);
		
		return user;
	}

}
```

## 다시 사용자 데이터 서비스

```java
//	3. Custom User Service
	@Bean
	public UserDetailsService rlandUserDetailsService() {
		
		return new RlandUserDetailsService();
	}
//	여기서 Bean을 선언하면 RlandUserDetailsService를 @Service를 달면 안된다.
//	다른사람이 만든 객체를 담을 땐 위와 같이 하면 된다
```

## 다시 메뉴 등록 요청을 보낼 때 username을 얻는 방법

```java
	@PostMapping("reg")
	public String reg(
			String title,
			Principal principal) {
		
//		방법 5 커스텀 사용자 정보 얻기
		RlandUserDetails user = (RlandUserDetails) authentication.getPrincipal();
		System.out.println(user.getUsername());
		
		return "redirect:list";
	}
```


참고 : [자바캔](https://javacan.tistory.com/entry/58)
