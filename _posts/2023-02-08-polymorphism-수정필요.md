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
		
		Banner[] banners = new Banner[2];

		banners[0] = new ICTBanner();
		banners[1] = new BB();
		
		for(int i= 0; i<2; i++)
			Lib.printIntro(banners[i]);

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
			System.out.println("  Lamda 익명 교육센터  ");
		});
//		----------------------------------------------------------------------------------------------

		List list = new ArrayList();
		list.add("3");
		list.add("2");
		list.add("6");
		list.sort(null);

		System.out.println(list); // [2, 3, 6]

		list.add("15");
		list.sort(null);
//		문자는 첫 번째 글자로 비교되기 때문에 잘못된 결과가 나온다
//		기준열을 제공하지 않고 정렬
		System.out.println(list); // [15, 2, 3, 6]

//		----------------------------------------------------------------------------------------------

		List list2 = new ArrayList();
		list2.add(3);
		list2.add(2);
		list2.add(6);
		list2.sort(null);

		System.out.println(list2); // [2, 3, 6]

		list2.add(15);
		list2.sort(null);
//		숫자는 잘 정렬된다
		System.out.println(list2); // [2, 3, 6, 15]

//		----------------------------------------------------------------------------------------------

		List list3 = new ArrayList();
		list3.add(new Exam(1, 3, 2));
		list3.add(new Exam(10, 20, 30));
		list3.add(new Exam(60, 50, 40));
		list3.add(new Exam(3, 4, 5));

//		list3.sort(new Comparator<T>() {
//			@Override
//			public int compare(T o1, T o2) {
//				// TODO Auto-generated method stub
//				return 0;
//			}
//		});

//		Comparator 람다식
//		list3.sort(null);로 비교한다면 어떤걸 기준으로 정렬해야 할지 모른다
		list3.sort((x, y) -> ((Exam) x).getKor() - ((Exam) y).getKor());
//		Kor점수료 정렬한다
//		양수, 0, 음수 3가지 경우의 수로 비교한다

		System.out.println(list3); // [2, 3, 6]
	}
}

```

Banner.java
-------------

```java

package com.newlecture.web.poly;

public interface Banner {
//	메소드 인터페이스(?)
	
//	interface - 부품의 분리를 생각할 때 부품을 구현하는 클래스가 만족해야할 최소한의 기능, 규칙, 규약을 정의하는 도구
//	interface형식으로 자료형을 만들어 뒀다
//	추상클래스 Banner가 아닌 약속으로써의 Banner가 필요하다
//	추상클래스 였다면 Banner의 자식으로 ICTBanner가 있다
//	추상화와 달리 집중화 목적이 아니다 / 코드 분리 목적
//	interface는 Banner에 들어온 Class가 print()를 구현하고 있어야 한다는 약속이다 / Class가 아니다
//	기능면의 인터페이스
	
//	interface는 두 가지다
//	1. 부품으로 분리되는 형태
//		여러개의 메소드를 갖고 있다
//		하나의 부품이 갖고 있어야 할 모든 목록을 다 구현한다
	
//	2. 코드의 일부분만 분리
//		메소드에서 일부분의 내용만 분리
	
//	추상 클래스와 인터페이스의 차이
//	추상 클래스는 이미 구현된 추상 클래스와 추상 클래스를 상속받은 객체 중 골라서 사용한다
	
//	인터페이스는 틀만 정리된 interface로 서비스를 만들어 놓고 추후에 interface를 구현한 부품을 서비스에 끼워서 사용한다
	
	

	void print();
//	public private은 구현한 것에대한 정의(??)다
//	따라서 안 들어간다 / 약속에 대한 명세를 쓰는 것이다

//	int x;
//	안 된다
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

Exam.java
-------------

```java

package com.newlecture.web.poly;

public class Exam {
	int kor;
	int eng;
	int math;

	public Exam() {
		this(0, 0, 0);
	}

	public Exam(int kor, int eng, int math) {
		this.kor = kor;
		this.eng = eng;
		this.math = math;
	}

	public void setKor(int kor) {
		this.kor = kor;
	}

	public void setEng(int eng) {
		this.eng = eng;
	}

	public void setMath(int math) {
		this.math = math;
	}

	public int getKor() {
		return kor;
	}

	public int getEng() {
		return eng;
	}

	public int getMath() {
		return math;
	}
	
}

```