---
title: encapsulation
tags: java
---

encapsulation
-------------

```java

package ex1;

public class encapsulation {

	public static void main(String[] args) {

//		캡슐화
//		구조화된 객체(Exam)를 사용하는 함수는 객체(Exam)의 데이터 구조 변경에 취약해진다 
//		따라서 구조화된 객체(Exam)와 구조화된 객체(Exam)를 사용하는 함수를 하나의 영역(Exam.java)에 함께 둔다
//		구조화된 객체(Exam)를 변경했을 때 오류가 날 수 있는 부분을 한정할 수 있다
//		역할, 행위를 가지고 있는 게 캡슐이다

//		자동차는 타입, 형식명 / 이름은 참조
//		자바에서 객체를 만들면 이름에 상관없이 hcode가 생성된다

//		객체지향은 우리가 머리속에 있는걸 그대로 실현하는 것

//		성적 객체를 준비한다.
		Exam exam1 = new Exam();
//		Exam exam1 = new Exam 여기 까지가 객체 생성 / ()constructor로 구분할 수 있다
//		() constructor 생성자란? 앞에 갓 생성된 객체가 있어야 한다 / 초기화 기능을 갖고 있다 / Exam.java 참고
//		초기화란 객체가 생성 되자 마자 무조건 제일 먼저 실행되어야 하고 생성될 때 한번만 실행 되어야 한다.

//		은닉성
//		exam1.kor = 30;
//		exam1.eng = 30;
//		exam1.math = 30;
//		Exam.java파일의 private으로 인해 Exam캡슐이 아닌 다른 곳에서는 데이터 구조를 사용할 수 없다

		exam1.init();
//		초기화가 아니라 리셋이다
//		init후 init을 할 수 있다 input후 init을 할 수 있기 때문에 정확한 의미를 따지자면 리셋이다.

//		성적을 입력 받는다.
		Exam.inputExam(exam1);
//		자바는 클래스 영역 내에 함수를 작성해야 하며, 다른 클래스에 있는 함수를 호출할 때는 반드시 .을 통해 외부 클래스를 밝혀 줘야 한다.
//		캡슐화를 깨지않기위해 구조화된 객체(Exam)와 구조화된 객체(Exam)를 사용하는 함수(inputExam)를 하나의 영역(Exam.java)에 함께 둔다
//		exam1을 매개변수로 받는다. / exam1을 인스턴스로 전달한다
//		다른언어에선 함수 Java에선 static 매서드(고전적인 함수)라고 한다 / 모든 값은 파라미터를 통해 넘겨 받는다
//		캡슐화를 깨트리면 encapsulation.java파일 내에 함수 작성도 가능하다

		exam1.input();
//		실 세계의 표현방식으로 프로그래밍 / 위와 같은 의미의 새로운 표기법 / 더 편하다고 느껴진다 / exam1야.입력해();
//		주체가 되는 exam1을 먼저 거론하는 표현하고 exam1을 이용한 입력(input) 출력(print)을 써준다.
//		파라미터(매개변수)로 전달받는게 아니라 (객체. 실체. 인스턴스.)을/를 통해 호출하게 되면 (객체 실체 인스턴스)이/가 저절로 전달된다(인스턴스 함수)
//		다른언어에선 매서드 Java에선 인스턴스 매서드라고 한다 / 객체를 통해서 호출되며 객체가 저절로 전달되는 함수
//		구조화된 객체(Exam)를 형식으로 가지는 exam1을 기본으로 전달받는 .input 인스턴스 매서드는 Exam.java파일에 작성해야만 한다

//		두 함수의 차이
//		매서드 구현 시 static의 여부 / 매서드 사용 시 매개변수의 여부

//		성적을 출력한다.
		Exam.printExam(exam1); // 객체지향스럽지 않은 함수

		exam1.print(); // 객체지향스러운 함수
	}

}

```

Exam
-------------

```java

package ex1;

import java.util.Scanner;

public class Exam {

	private int kor;
	private int eng;
	private int math;

//	은닉성
//	예를들어 다른 캡슐에 있는 데이터 구조를 직접 파라미터(매개변수)로 받거나 지역적으로 new를 통해 속성을 쓴다면 다른 캡슐에 있는 데이터 구조의 변화(이름, 형식)에 따라 Exam캡슐이 영향을 받을 수 있다 / 캡슐이 깨진다
//	따라서 다른 캡슐에서 관련 기능의 함수를 만들고 Exam에선 함수를 호출해서 사용한다
//	캡슐화는 어느 언어에서도 가능하다
//	C++, JAVA는 객체지향을 지원해서 private를 통해 캡슐을 깨지 못하게 막을 수 있다
//	C언어의 경우 다른 캡슐에서 Exam의 데이터 구조를 사용한다 해도 막을 방법이 없다
//	데이터 구조의 경우 private 거의 필수 / 다른 곳에서 서비스해야 하는 함수는 public

	public Exam() {
//		생성자()는 이름이 없다. Exam은 이름이 아니고 Exam형식의 객체만 호출할 수 있는 의미 / 한정사 / 함수가 아니다
//		Exam형식의 객체가 만들어 져야만 호출 할 수 있다
		kor = 30;
		eng = 30;
		math = 30;
	}

	public void input() {
		Scanner scan = new Scanner(System.in);

		System.out.println("┌───────────┐");
		System.out.println("│  성적 입력	│");
		System.out.println("└───────────┘");

		do {
			System.out.print("국어 : ");
			kor = scan.nextInt();

			if (kor < 0 || 100 < kor)
				System.out.println("입력 가능 범위 : 0 ~ 100");

		} while (kor < 0 || 100 < kor);

	}

	public void print() {
//		주로 쓰는 함수 / 인스턴스 함수는 this를 사용할 수 있다 / this를 생략해도 된다 / this를 안 써주는게 더 좋다

		int kor = 10;

		System.out.printf("kor:%d\n", kor);
//		지역변수에서 kor이 있는지 찾고 없으면 this에서 kor을 찾는다 / 전역변수 Exam에서 절대 찾지 않는다.
//		우선순위에 따라 지역변수 kor 10이 먼저 인식된다

		System.out.printf("eng:%d\n", eng);
		System.out.printf("math:%d\n", math);

	}

	static void printExam(Exam exam1) {
//		인스턴스 함수가 아니라는 의미로 static을 붙인다. this를 사용할 수 없다
//		다른 클래스에서 사용해야 하기 때문에 private 삭제

		int kor = exam1.kor;

		System.out.printf("kor:%d\n", kor);
		System.out.printf("eng:%d\n", exam1.eng);
		System.out.printf("math:%d\n", exam1.math);

	}

	static void inputExam(Exam exam1) {

	}

	public void init() {
		kor = 30;
		eng = 30;
		math = 30;
//		private로 인해 외부에서 데이터 구조를 변경할 수 없기 때문에 Exam안으로 들어와서 리셋 해주는 함수 구현
	}

}

```
