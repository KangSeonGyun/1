---
title: Spring Login
tags: spring 
---

해당 예제는 Passward 암호화 과정 및 Security를 생략했다.

### 각 층 구현

#### Controller

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
	public String login(String uid, String pwd) {
//		사용자에게 쿼리스트링을 통해 uid와 pwd를 입력받는다.

		Member member = memberservice.getByUesrName(uid);
//		여기서 멤버를 받아오면 Controller가 권한을 확인 한다는 것이다. 따라서 위와 같이 하면 안된다.
//		Controller는 입, 출력만 담당하고 권한을 확인하는 것은 Service(Business) Layer에서 한다.

		boolean isVaild = memberservice.isVaildMember(uid, pwd);
//		컨트롤러는 입출력만 해야 하므로 값을 그대로 넘긴다

		return "redirect:login";
	}

}
```

isVaild는 사용자가 입력한 uid(User ID), pwd(PassWard)가 정확하다면 true, 아니면 false이다.   
자세한 내용은 Service단과 Repository단을 확인해보자.

#### Service

```java
public interface MemberService {

	boolean isVaildMember(String uid, String pwd);

}
```

위 MemberService를 구현한 DefaultMemberService

```java
@Service
public class DefaultMemberService implements MemberService {

	@Autowired
	private MemberRepository repository;

	@Override
	public boolean isVaildMember(String uid, String pwd) {
		
		Member member = repository.findByUesrname(uid);
		
		if(member == null)
//			member가 null이면 .getPwd가 안된다. 따라서 null이 아닌지 확인 해야함.
			return false;
		else if(!member.getPwd().equals(pwd))
//			equals가 아닌 ==, != 으로 비교하면 안된다.
			return false;
		
		return true;
	}

}
```

사용자에게 입력받은 uid를 통해 DB에서 사용자 정보(Member)를 얻어온다.   
uid를 DB에서 찾을 수 없다면 사용자가 유효하지 않은 계정을 입력한 것이고 Member는 null이 된다.   
uid를 DB에서 찾을 수 있다면 사용자가 유효한 계정을 입력한 것이고 Member Entity의 속성(pwd)를 통해 사용자가 입력한 pwd와 비교할 수 있다.

Member가 null이거나 패스워드가 일치하지 않는다면 false를 반환하고 아니면 true를 반환한다.

#### Repository

```java
@Mapper
public interface MemberRepository {
	
	Member findByUesrname(String uid);
}
```

Mybatis Mapper사용

#### Mapper

```xml
<select id="findByUesrname" resultType="Member">
		SELECT * FROM member where username=#{uid}
</select>
```

사용자가 입력한 uid와 일치하는 username을 찾아 해당 회원의 모든 정보를 Member Entity에 담는다.   
username이 DB에서 사용자가 입력하는 계정 ID 컬럼명이다. 이유는 후술.

#### Entity Member

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Member {
	private Long id;
	private String userName;
	private String pwd;
	private String email;
	private int roleId;
}
```

사용자가 입력하는 계정을 식별자로 하는 것이 아닌 다른 식별자(PK)를 둬야 한다.   
사용자가 탈퇴해도 DB의 식별자(PK)는 남겨둬야 한다.   
계정을 식별자로 할 경우 해당 계정 ID를 다시는 사용하지 못하는 상황이 생긴다.   
따라서 사용자가 입력하는 계정 ID는 userName으로 했다.   

<details>
<summary>제목</summary>
<div markdown="1">

내용

</div>
</details>

---

<details>
<summary>제목</summary>

내용

</div>
</details>

---

<details>
<div markdown="1">

내용

</div>
</details>

---

<details>
<summary>제목</summary>

내용

</details>

---

<details>

내용

</details>