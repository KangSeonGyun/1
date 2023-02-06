---
title: has a inheritance
tags: java
---

inheritance.java
-------------

```java

package ex1.has_a;

// 상속(inheritance)
// has_a 결합관계 제품과 부품의 관계가 된다 / 상속이란 단어에 수직적인 느낌이 있어서 상속으로 표현하지 않고 제품과 부품으로 표현한다.
// aggregation has a(부품을 합쳐서 사용) / composition has a(일체형)
// main과 main에서 사용된 캡슐이 있다 / main(제품) main에서 사용된 캡슐(의존객체, 부품)
// 부품과 부품에서 사용된 캡슐이 있다 / ExamConsole캡슐(제품)에서 사용된 Exam캡슐(부품) / main에겐 부품인 ExamConsole캡슐이 제품이 될 수 있다

// Dependency관계
// 매서드 내에서만 일시적으로 객체를 생성하여 사용되는 Scanner (의존객체)
// 클래스가 내에서가 아닌 함수(main)내에서만 사용되는 이용(부품)관계 inheritance <-> ExamConsole
// ExamConsole내에서 Exam을 사용도 하지만 구성도 하고있는 영구적인 관계 ExamConsole <-> Exam

public class inheritance {

	public static void main(String[] args) {

		ExamConsole console = new ExamConsole();
//		Dependency관계 / inheritance가 만들어지는데 꼭 필요한 의존객체로 ExamConsole사용하지만 구성하는 멤버로는 ExamConsole이 사용되고 있지 않다
		
		console.input();
		console.print();
	}

}

```

ExamConsole.java
-------------

```java

package ex1.has_a;

import java.util.Scanner;

public class ExamConsole {
//	콘솔로 일체화된 데이터 관리 클래스는 콘솔 외에 다른 플랫폼에서 재사용이 어렵다
//	Exam.class는 재사용에 용이하지만 ExamConsole.class는 input(), print()매서드 내 콘솔 사용으로 인해 콘솔 외 플랫폼에 재사용이 어렵다

	private Exam exam;
	private Exam[] exams;
//	캡슐(ExamConsole)이 다른 캡슐(Exam)을 이용하는 관계가 있다 / 이용하기 위해 부품으로 가지고 있으며 자기의 멤버로 갖고있다 has a 상속
	
	
//	private Exam exam = new Exam();
//	클래스 내에서는 연산자(사칙연산, 대입연산, NEW)를 사용하면 안된다. 매서드 내에서만 사용 가능하다
//	JDK버전이 올라가며 컴파일러의 기능으로 인해 두 방법 모두 가능해 졌으니 편한방법을 사용하자


	public ExamConsole() {
		exam = new Exam();
//		ExamConsole을 만들면 부품(Exam)도 같이 만들어짐 / ExamConsole과 Exam은 composition has a관계 / 결합도가 높아졌다
		exams = new Exam[3];
//		Exam객체가 만들어지지 않고 Exam을 참조하기 위한 참조변수 3개짜리 / aggregation has a관계 / 결합도가 낮아졌다 (더 좋다)
//		ExamConsole객체가 먼저 만들어지고 필요에 따라 Exam에서 객체가 참조변수에 대입된다 / 객체 생명주기가 일치하지 않다
	}
//	ctrl + space + enter 생성자 자동생성
	
	
	public void input() {
		Scanner scan = new Scanner(System.in);

		System.out.println("┌───────────┐");
		System.out.println("│  성적 입력	│");
		System.out.println("└───────────┘");

		int kor;
		do {
			System.out.print("국어 : ");


			kor = scan.nextInt();

			if (kor < 0 || 100 < kor)
				System.out.println("입력 가능 범위 : 0 ~ 100");

		} while (kor < 0 || 100 < kor);

		exam.setKor(kor);
//		Exam안 kor은 private로 인해 사용할 수 없다 / setter 사용

		int eng;
		do {
			System.out.print("영어 : ");

			eng = scan.nextInt();

			if (eng < 0 || 100 < eng)
				System.out.println("입력 가능 범위 : 0 ~ 100");

		} while (eng < 0 || 100 < eng);

		exam.setEng(eng);

		int math;
		do {
			System.out.print("수학 : ");

			math = scan.nextInt();

			if (math < 0 || 100 < math)
				System.out.println("입력 가능 범위 : 0 ~ 100");

		} while (math < 0 || 100 < math);

		exam.setMath(math);
	}

	public void print() {
		print('-', 30);
	}

	public void print(char ch) {
		print(ch, 30);
	}

	public void print(char ch, int length) {
		int total = exam.total();
		double avg = exam.avg();
//		total과 avg는 Exam.class에 있는 데이터로 계산하기 때문에 Exam.class에서 계산하는 게 바람직하다

		System.out.printf("kor:%d\n", exam.getKor());
		System.out.printf("eng:%d\n", exam.getEng());
		System.out.printf("math:%d\n", exam.getMath());
		System.out.printf("total:%d\n", total);
		System.out.printf("avg:%f\n", avg);

		for (int i = 0; i < length; i++)
			System.out.printf("%c", ch);

		System.out.println();
	}

}

```

Exam.java
-------------

```java

package ex1.has_a;
// ctal + shift + O 안쓰는 import 삭제

public class Exam {
	private int kor;
	private int eng;
	private int math;
//	데이터 구조는 노출하면 안됨
//	데이터 구성(변수 갯수), 속성(변수명)은 노출해도 된다

	public Exam() {
		this(0, 0, 0);
	}

	public Exam(int kor, int eng, int math) {
		this.kor = kor;
		this.eng = eng;
		this.math = math;
	}

	public void reset() {
		kor = 0;
		eng = 0;
		math = 0;
	}

	public double avg() {
		return (double) total() / 3.0;
	}

	public int total() {
		return kor + eng + math;
	}
	
//	캡슐화
//	setter, getter 자동생성
//	우클릭 > Source > Generate Getters and Setters...
	
//	setter, getter 사용 이유
//	데이터 속성(변수명, 변수 갯수)의 변화 때문이 아닌 데이터 구조의 변경 ex) exam.kor > exam.subject.kor / subject클래스로 한 단계 깊어진 경우
//	setter, getter를 사용 한다면 내부에서만 바뀔뿐 외부에서 데이터 구조의 변화를 느낄 수 없다
	
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
