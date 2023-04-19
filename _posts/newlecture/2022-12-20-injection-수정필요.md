---
title: Injection
tags: java
---



A제품에 B부품(Dependency)이 있을 경우 A와 B를 조립(Injection)하는 것을 DI라 한다.   
조립(Injection)의 방법은 Setter Injection(조립형)과 Construction Injection(조립형)이 있다.

**Setter Injection**

```java
B b = new B();
A a = new A();

a.set(b);
```
**Construction Injection**

```java
B b = new B();
A a = new A(b);
```


```java

package ex1;

public class Injection {

	public static void main(String[] args) {
// DI(Dependency Injection) 의존성 주입 / 부품 결합으로 이해하자
// 의존관계란? / A가 B를 의존한다 > 의존대상 B가 변하면, 그것이 A에 영향을 미친다.
// B의 기능이 추가 또는 변경되거나 형식이 바뀌면 그 영향이 A에 미친다.

//		DI에서 Injection의 두 가지 방식

//		1. Setter Injection

//		A a = new A();
//		B b = new B();
//		a.setB(b);

//		2. Construction Injection

//		has a와 is a 두 가지상속이 있다

//		has a
//		로봇을 만들 때 부품(머리 몸통 팔 다리)를 가져와서 만듦

//		is a
//		몸통을 만들 때 몸통 틀(Framework)을 가져와 수정을 통해 만듦 / 로봇을 만들 때 로봇 틀(Framework)을 가져와 수정을 통해 만듦
	}

}

```
