---
title: Polymorphism2
tags: java
---

Link: [Polymorphism][id]

[id]: https://ksg0000.github.io/2023/02/08/Polymorphism.html

Program.java
-------------

```java

package com.newlecture.web.인터페이스;

import java.io.FileInputStream;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Scanner;

public class Program {

	public static void main(String[] args)
			throws IOException, InstantiationException, IllegalAccessException, ClassNotFoundException,
			IllegalArgumentException, InvocationTargetException, NoSuchMethodException, SecurityException {
		A a = new A();
//		B는 A클래스 내부에서 결합하는 일체형 / 밖에선 B(부품)를 알 수 없다
		a.print();

//		--------------------------------------------------------------------------------------

		C c = new C();
//		interface X를 구현한 모든 객체를 결합할 수 있는 결합형 / 결합력이 낮아 졌다
		a.setX(c);
//		부품 C와 결합했다
		a.print2();

//		--------------------------------------------------------------------------------------

//		소스코드 변경하지 않고 기존 객체를 새로운 객체로 바꿔 낄 수 있는 방법

//		외부 설정파일의 대표적인 방법 2가지
//			1. xml 형태의 외부 설정파일
//			2. Class에 컴파일 해도 남겨지는 주석 어노테이션

//		기본적인 외부파일로 실습 / setting.txt를 수정하는 방법
//		setting.txt의 내용 - com.newlecture.web.인터페이스.C

		FileInputStream fis = new FileInputStream("src/main/java/com/newlecture/web/인터페이스/setting.txt");
		Scanner scan = new Scanner(fis);
		String className = scan.nextLine();
		scan.close();
		fis.close();
		System.out.println(className); // com.newlecture.web.인터페이스.C

		X x = new className(); // className cannot be resolved to a type / 에러
//		X x = new className();은 X x = new "com.newlecture.web.인터페이스.C"();와 같다 / 문자열 앞에 new는 불가능 하다

		Class clazz = Class.forName(className);
//		Class.forName - 문자열로 읽어온 것을 new하듯이 생성 / Class에 대한 정보를 얻을 수 있다 / 패키지 다 포함해야 한다 / 객체.getClass()와 같은 의미
//		C.class.get... 으로 C 클래스가 갖고있는 속성, 메소드, 생성자, 어노테이션, 프로그램 구조 등등을 알아낼 수 있다

		X x = (X) clazz.getDeclaredConstructor().newInstance();
//		새로운 객체로 바꿔 낄 수있게 자료형을 interface형 X로 한다
//		Class로 새로운 Instance를 만들었다 / Instance를 만드는 2가지 방법 C.class.newInstance(); or new C(); 
//		jdk버전이 1.8보다 낮다면 X x = (X) clazz.newInstance();
		a.setX(x);
		a.print2();

	}

}

```

A.java
-------------

```java

package com.newlecture.web.인터페이스;

public class A {
	private B b;
	private X x;
//	캡슐 단위로 코드를 분리했다
//	interface X가 정의한 기능을 구현한 객체는 무조건 참조변수 x로 참조 가능하다

	public A() {
		b = new B();
//		A와 B는 결합력이 강하다 / B를 대체할 수 없다 / 결합력이 강하다는 것은 소스코드를 통해서만 수정할 수 있다는 뜻
//		B가 없는 경우를 생각해야 한다 / B가 없다면 A를 구현할 수 없기에 B를 인터페이스로 구현해야 한다
//		결합력을 약하게 하려면 소스코드 없이도 수정 가능해야 한다

		x = new X(); // Cannot instantiate the type X / 에러
//		interface는 구현한 것이 아니고 실제로 사용할 수 있는 도구가 아니므로 interface 객체생성은 말이 안 된다
//		객체를 생성한다는 것은 X의 데이터 객체를 생성하는 건데 interface는 데이터를 정의하고 있지 않다
	}

	public void print() {
		int total = b.total();
		System.out.printf("total is %d\n", total);
	}

//	--------------------------------------------------------------------------------------

	public void setX(X x) {
		this.x = x;
	}

	public void print2() {
		int total = x.total();
		System.out.printf("total is %d\n", total);
	}

}

```

X.java
-------------

```java

package com.newlecture.web.인터페이스;

public interface X {
	int total();
}

```

B.java
-------------

```java

package com.newlecture.web.인터페이스;

public class B {
	public int total() {
		return 30;
	}
}

```

C.java
-------------

```java

package com.newlecture.web.인터페이스;

public class C implements X {
	public int total() {
		return 30;
	}
}

```
