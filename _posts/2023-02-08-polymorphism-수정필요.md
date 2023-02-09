---
title: Polymorphism
tags: java
---

Program.java
-------------

```java

package com.newlecture.web.poly;

import java.util.ArrayList;
import java.util.List;

public class Program {

	public static void main(String[] args) {
//		다형성
//		하나의 프로그램을 여러 곳에서 쓸 때 달라져야할 부분이 있다
//		코드의 일부분을 쉽게 바꿔낄수있는 방법이 필요하다

		Banner banner;
//		interface를 이용한 변수선언
//		참조형으로 Banner를 참조할 수 있다
//		Banner는 약속 수반한 자료형이다
		
		banner = new ICTBanner();
//		Banner interface를 이용해 분리되어 있던 ICTBanner객체를 사용할 수 있게됐다

		Lib.printIntro(banner);
		
//		ICTBanner객체를 함수의 인자로 넘겨줬다  / 어떤 것을 print할지는 ICTBanner가 결정

//		----------------------------------------------------------------------------------------------

//		메소드 내부 클래스
//		새로운 클래스 파일 생성이 의미 없어보였다

		class AAA implements Banner {
//		interface의 약속을 구현할 수 있는 방법이 class뿐이여서 class를 꼭 새로운 파일로 만들어야한다는 문제가 있었다
//		ICTBanner class를 만들고 new로 객체로 만들어야 하는 불편합이 있다
//		자바 1.5부터 함수 안에서 클래스 정의가 가능해졌다 / interface구현목적으로만 사용하자
			@Override
			public void print() {
				System.out.println("   AAA 교육센터   ");

			}

		}

		Banner banner2 = new AAA();

		Lib.printIntro(banner2);

//		----------------------------------------------------------------------------------------------

//		익명 클래스
//		클래스 생성이 의미 없어보였다

		Banner banner3 = new Banner() {
//			인터페이스 구현블록
			@Override
			public void print() {
				System.out.println("   익명 교육센터   ");
			}
		};
//		대입연산 때문에 ;을 붙여줬다

		Lib.printIntro(banner3);

//		----------------------------------------------------------------------------------------------

//		객체 생성도 의미가 없어보였다
		Lib.printIntro(new Banner() {

			@Override
			public void print() {
				System.out.println("   BBB 교육센터   ");

			}
		});
//		인자가 너무 비대하다 / 인자로는 람다함수를 사용해 주는게 좋다

//		람다식을 활용한 익명함수
//		자바에선 메소드 인터페이스에서만 사용
//		하나의 함수만 약속된 interface만 구현 가능하다
//		함수가 2개 이상이라면 람다가 어떤함수를 정의했는지 알 수 없다
		Lib.printIntro(() -> {
//			함수로 구현했기 때문에 {}를 써줬다
			System.out.println("  lambda 익명 교육센터  ");
		});
		
	}
}

```

Lib.java
-------------

```java

package com.newlecture.web.poly;

public class Lib {
//	Lib이 set 느낌???

	static void printIntro(Banner banner) {
//		interface를 통해 ICTBanner형식으로 한정하는 것이 아닌 Banner형식으로 받을 수 있게 됐다(?) / 재사용 가능
		System.out.println("┌───────────────────────┐");
		banner.print();
//		구현되지 않은 print()매서드를 사용했다 / 이후 ICTBanner에서 구현
//		System.out.println("뉴렉처 교육 프로그램"); 대신 banner.print();으로 특정 교육센터 이름이 들어간 부분을 분리했다
		System.out.println("└───────────────────────┘");
	}
//	Lib.java는 jar파일로 재사용했다고 생각해라 / 라이브러리처럼 생각 / 해당 실습에선 재사용을 생략했다
//	jar파일로 재사용했다면 static대신 public사용 가능

}

```

ICTBanner.java
-------------

```java

package com.newlecture.web.poly;

public class ICTBanner implements Banner {
//	class상속 / 부모 클래스를 물려받는다
//	interface상속(구현) / 물려받는게 없다
//	틀만 정리된 interface를 사용한 서비스를 받고 싶을 때 interface를 구현해 끼운다 / 필요에 따라 결합
	
//	하나의 클래스에서 여러개의 인터페이스를 구현할 수 있다
	
//	interface 약속 구현이 중요한 것이지 꼭 class로 구현하지 않아도 된다
	@Override
	public void print() {
		System.out.println("   ICT 교육센터   ");
	}

}

```