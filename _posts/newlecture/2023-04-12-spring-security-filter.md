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

## Spring Security 라이브러리

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

		http
			.authorizeHttpRequests()
				.requestMatchers("/admin/**").hasAnyRole("ADMIN")
				.requestMatchers("/member/**").hasAnyRole("ADMIN", "MEMBER")
				.anyRequest().permitAll();
				
		return http.build();
	}
```

HttpSecurity는 import할 때 패키지명을 보면 알 수 있듯이 보안 설정을 위한 Builder다.

hasAnyAuthority()를 쓸 때 파라미터가 꼭 ROLE_로 시작해야 하는 규칙이 있다. 예) ROLE_ADMIN   
hasAnyRole()은 과거 ROLE_을 꼭 붙여야하는 규칙을 생략해도 된다. 예) ADMIN

url이 admin으로 시작한다면 ADMIN 권한을 가진 유저만 접근할 수있다.   
url이 member로 시작한다면 ADMIN 혹은 MEMBER 권한을 가진 유저만 접근할 수있다.   
admin과 member로 시작하지 않는 나머지는 url은 모든 유저 허용

주소창에서 /admin 혹은 /member에 들어가면 403 error, 세션 부여됨

## 사용자 데이터 서비스

위에서는 접근권한만 설정해줬지 사용자 정보를 가져오진 않았다.

사용자 데이터 서비스를 구현하는 방법은 인메모리 서비스, JDBC 서비스, LDAP 서비스, Custom User 서비스 등.. 이 있다.

### 인메모리 서비스

우선 인메모리 서비스다. 쉽게 생각하면 내가 정의한 사용자 리스트이다.   
아래와 같이 newlec, dragon 등 사용자를 내가 만들어 주면 된다.

```java
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

### JDBC 서비스

application.properties에 DB정보(datasource)가 정의되어 있다면 Bean에 담겨있다.   
즉 Autowired로 간편하게 사용할 수 있다.

```java
@Autowired
private DataSource dataSource;
```

```java
	@Bean
	public UserDetailsService jdbcUserDetailsService() {
		
		JdbcUserDetailsManager manager = new JdbcUserDetailsManager(dataSource);
		
		manager.setUsersByUsernameQuery("select username, pwd password, 1 enabled from member where username=?");
//		로그인 시도한 사람의 계정, 패스워드, enabled(계정 활성화, 비활성화 여부)만 가져와야한다.
//		만약 DB Table에서 활성화, 비활성화 여부를 저장하지 않는다면 고정값 1로 할 수 있다.
		
		manager.setAuthoritiesByUsernameQuery("SELECT username, 'ROLE_ADMIN' authority from member where username=?");
//		member Table에 권한 정보가 없다고 가정하고 고정값 ROLE_ADMIN를 넣었다.

		return manager;
	}
```

### Custom User Service

만약 로그인 한 사람의 계정, 패스워드 뿐만아니라 이메일 등 다른 정보도 가져와야 할 필요가 있다면 Custom User Service를 만들어야 한다.   
사용자 정보를 담을 그릇(UserDetails)와 사용자 정보를 담는 서비스(UserDetailsService)를 구현하면 된다.   

바로 다음에서도 언급할 것이지만 미리 말하면 여기서 @Bean을 선언하면 RlandUserDetailsService에서 @Service를 달면 안된다. 같은 Bean을 두 번 담는 결과가 된다.

```java
	@Bean
	public UserDetailsService rlandUserDetailsService() {
		
		return new RlandUserDetailsService();
	}
```

## 알랜드 만의 UserDetails 만들기

우선 그릇부터 만들 것이다. UserDetails를 구현하는 RlandUserDetails를 구현할 것이다.   
이 방법의 장점은 필요한 사용자 정보를 내가 원하는 만큼 가져올 수 있다는 것.

```java
@Data
@ToString
public class RlandUserDetails implements UserDetails {

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

다음과 같이 User를 상속받아도 된다

```java
public class RlandUserDetails extends User implements UserDetails {

}
```

## 알랜드 만의 UserDetailsService 만들기

위에서 언급했듯이 여기서 @Service를 하면 같은 Bean을 두 번 담았다는 에러가 난다.

```java
//@Service
public class RlandUserDetailsService implements UserDetailsService {

	@Autowired
	private MemberRepository repository;
	
	@Override
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		
		Member member = repository.findByUesrname(username);
//		DB에서 username으로 회원 정보를 가져온다.
		
		List<GrantedAuthority> authorities = new ArrayList<>();
		authorities.add(new SimpleGrantedAuthority("ROLE_ADMIN"));
//		임시로 권한 리스트엔 ROLE_ADMIN 고정 값을 넣었다.
		
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

## Spring Security 사용 예제

가장 간단한 인메모리 서비스에 이어지는 예제이다.   
인메모리 서비스를 이용해 단순 사용자만 만들면 403에러가 난다.   
.and() .formLogin(); 을 해줘야 로그인 화면이 뜬다. 사용자가 로그인 폼을 만들지 않으면 기본 폼을 제공한다.

```java
		http
			.authorizeHttpRequests()
				.requestMatchers("/admin/**").hasAnyRole("ADMIN")
				.requestMatchers("/member/**").hasAnyRole("ADMIN", "MEMBER")
				.anyRequest().permitAll()
			.and()
				.formLogin();
```

이래도 ID:newlec PW:111을 입력하면 500에러가 난다. There is no PasswordEncoder mapped for the id "null"   

PasswordEncoder가 필요하다. PasswordEncoder는 필수이다. 나는 BCryptPasswordEncoder 사용했다.   
원문을 알면 hash값을 알아 낼 수 있지만 hash값으로 원문을 만들어 낼 순 없다.

```java
	@Bean
	public PasswordEncoder passwordEncoder() {
		
		return new BCryptPasswordEncoder();	
	}
```

이래도 ID:newlec PW:111로 로그인이 안된다.   
왜냐하면 111을 암호화했을 때 나온 $2a$10$H.HXCEd59CmUnp9luiKHwestJxuSIGRsJZNzCWfTfpJPgk6VGLm3O을 newlec의 password로 수정해야 한다.   

해쉬값은 다를 수 있다.   
엄청큰 파일을 암호화해도 같은 길이의 암호가 만들어진다. 단순 식별자 이기 때문

```java
		UserDetails newlec = User
				.builder()
				.username("newlec")
				.password("$2a$10$H.HXCEd59CmUnp9luiKHwestJxuSIGRsJZNzCWfTfpJPgk6VGLm3O")
				.roles("ADMIN", "MEMBER")
				.build();
```

이젠 ID:newlec PW:111로 로그인을 입력하면 로그인이 된다.   
왜냐하면 사용자가 입력한 111을 입력하면 암호화 되며 password가 일치한 것이다.   
하지만 암호화된 문자열을 미리 알 수 없으므로 아래와 같이 수정한다.   
이로써 사용자가 뭘 입력하는 지는 궁금하지않고 암호화된 문자열이 같으면 비밀번호가 일치한 것이다.

```java
		UserDetails newlec = User
				.builder()
				.username("newlec")
				.password(passwordEncoder().encode("111"))
				.roles("ADMIN", "MEMBER")
				.build();
```

이제 스프링이 기본으로 제공해주는 로그인 페이지가 아닌 내가 만든 로그인 페이지 사용하고 싶을 수 있다.   
.loginPage("/user/login");을 붙이면 로그인을 해야할 상황이 왔을 때 /user/login으로 이동한다.   

잘 작동될줄 알았지만 ID:newlec PW:111 로그인 시도하면 403에러가 난다.

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

Same Origin인지 확인할 수 있는 Key가 없다. 즉, CORS 공격에 대한 대비가 없기 때문이다.   
cors를 비활성화 했다.

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

하지만 이래도 ID:newlec PW:111 로그인 시도하면 403에러난다.   

왜냐하면 /user/login 페이지에서 로그인하면 /user/login로 Post요청이 갈 것이다.   
Spring Security를 사용하려면 /login으로 post요청을 보내야 한다.   
이게 싫고 내가만든 post요청을 쓰려면 loginProcessingUrl통해 알려줘야 한다.   

loginProcessingUrl에 매핑된 /user/login으로 Post요청이오면 스프링이 구현하고 있는 로그인 로직을 사용한다는 의미다.   
따라서 form action속성과 맞춰 주기만 하면 막 써도 괜찮다.   
기존에 내가 만들어 놓은 PostMapping /user/login이 있다면 무시된다.

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

이래도 403에러. CSRF설정이 필요하다. Spring 버전에 따라 cors만 설정해도 되는 경우가 있다.

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

만약 html에서 form태그 input의 name 속성이 스프링이 정한 username, password가 아니라 사용자가 임의로 정한 uid, pwd일 경우 로그인 되지 않는다.   
html을 고쳤다면 로그인이 된다 defaultSuccessUrl는 로그인 성공시 이동할 url이다.

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

## Logout

Spring Security를 사용하면서 아래와 같이 logout을 처리하는 로직을 구현하면 무시된다.

```java
	@RequestMapping("logout")
	public String logout(HttpSession session) {
		
		session.invalidate();
		
		return "redirect:/index";
	}
```

Spring Security를 사용해 logout을 구현하는 방법.   
.logout() 으로 구현 가능하다.   
logoutUrl은 /user/logout 요청이 올 경우 스프링의 로그아웃 로직이 실행된다.   
logoutSuccessUrl은 로그아웃에 성공할 시 이동할 url이다.

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

## 메뉴 등록 요청을 보낼 때 username을 얻는 방법

어떤 회원이 메뉴 등록 요청, 글 작성 등.. 요청을 보낼 때 누가 요청을 했는지 알아야 할 필요가 있다.   
아래는 Spring에서 username을 얻는 방법들이다.

1

```java
	@PostMapping("reg")
	public String reg(String title) {
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
//		getPrincipal() 이건 유저 정보를 준다. username, password(보호됨), enabled, 권한 등을 알 수 있다
//		UserDetails로 형 변환만 해주면 된다.

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

5 Custom User 정보 얻기

```java
	@PostMapping("reg")
	public String reg(
			String title,
			Principal principal) {

		RlandUserDetails user = (RlandUserDetails) authentication.getPrincipal();
		System.out.println(user.getUsername());
		
		return "redirect:list";
	}
```

참고 : [자바캔](https://javacan.tistory.com/entry/58)
