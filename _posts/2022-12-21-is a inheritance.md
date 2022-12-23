---
title: is a inheritance
tags: java
---

is a inheritance
-------------

```java

package ex1.is_a;

public class inheritance {

	public static void main(String[] args) {

//		누군가 배포한 binary실행파일(Exam.class)를 이용해 새로운 기능을 추가하여 (NewlecExam.java)사용
//		소스코드인(Exam.java)는 알 수 없음
		
		NewlecExam exam = new NewlecExam(1,1,1,1);
		System.out.println(exam.total());
		System.out.println(exam.avg());
//		부품으로 썻다면 exam. 찍으면 뭐가 나온다
		
//		ExamConsole console = new ExamConsole();
		
//		console.input();
//		console.print();
	}

}

```

NewlecExam
-------------

```java

package ex1.is_a;

public class NewlecExam extends Exam {

//	exam(부모 상위클래스 기반클래스), NewlecExam(자식 하위클래스 확장클래스)
//	private Exam exam;
//	쓸 수 없다 exam은 틀이다
	private int com;

	public NewlecExam() {
//		kor = 1;
//		쓸 수 없다
//		super();
//		순서 바꾸면 안됨 어제와 같은 이유
//		this.com = 1;
//		내 건 내가 초기화
//		setKor(1);

		this(0, 0, 0, 0);
//		집중화
	}

	public NewlecExam(int kor, int eng, int math, int com) {
		super(kor, eng, math);
		this.com = com;
	}

//	ctrl + space 부모가 갖고 있는 함수 표시

//	Override / 고쳐쓰는 함수
//	부모에 있던 total 위에 자식 total을 덮어 쓴다 올라 탄다 / 부모에 있는 total을 못보게 함
	public int total() {
		return super.total() + com;
	}
//	재활용 할 수 있었다

	public double avg() {
		return total() / 4.0;
	}
//		
}

```

ExamConsole
-------------

```java

package ex1.is_a;

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

package ex1.is_a;
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
