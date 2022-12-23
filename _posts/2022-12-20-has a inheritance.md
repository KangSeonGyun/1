---
title: has a inheritance
tags: java
---

has a inheritance
-------------

```java

package ex1.has_a;

// 상속(inheritance)
// has_a 결합관계 제품과 부품의 관계가 된다 / 상속이란 단어에 수직적인 느낌이 있어서 상속으로 표현하지 않고 제품과 부품으로 표현한다.
// main과 main에서 사용된 캡슐이 있다 / main(제품) main에서 사용된 캡슐(의존객체, 부품)
// 부품과 부품에서 사용된 캡슐이 있다 / 부품(제품) 부품에서 사용된 캡슐(부품)
// program <- association has a 관계 -> ExamConsole <-구성(composition has a) 관계를 가지고 있다-> Exam
// composition이란 일체형 관계
// association부품을 합쳐서 사용해야함
public class inheritance {

	public static void main(String[] args) {

		ExamConsole console = new ExamConsole();
//		composition has a관계
//		exam을 바꿔낄 수 없다
//		결합도가 높아졌다
		
//		Exam exam = new Exam(1, 2, 3);
//		console.setExam(exam);
//		association has a관계
//		exam을 바꿔낄 수 있다 / 더 좋다
		
		console.input();
		console.print();
	}

}

```

ExamConsole
-------------

```java

package ex1.has_a;

import java.util.Scanner;

public class ExamConsole {

	private Exam exam;
//	private Exam exam = new Exam();
//	별로 안좋음 번역기가 해준거임
	public ExamConsole() {
//	ctrl + space + enter 생성자 자동생성
		exam = new Exam();
	}

//	has a를 가진거다
	public void input() {
		Scanner scan = new Scanner(System.in);

		System.out.println("┌───────────┐");
		System.out.println("│  성적 입력	│");
		System.out.println("└───────────┘");

		int kor;
		do {
			System.out.print("국어 : ");
//			Exam안 kor은 private로 인해 사용할 수 없다.

			kor = scan.nextInt();

			if (kor < 0 || 100 < kor)
				System.out.println("입력 가능 범위 : 0 ~ 100");

		} while (kor < 0 || 100 < kor);

		exam.setKor(kor); // setter / this 생략 가능

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

Exam
-------------

```java

package ex1.has_a;
// ctal + shift + O 안쓰는 import 삭제

public class Exam {
	private int kor;
	private int eng;
	private int math;
//	데이터 구조 int kor, int , int 는 노출하면 안됨
//	데이터 구성, 속성은 노출해도 된다 ex) 변수명

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
	
//	setter, getter 자동생성
//	우클릭 > Source > Generate Getters and Setters...
	
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
