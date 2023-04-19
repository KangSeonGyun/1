---
title: is a inheritance
tags: java
---

inheritance.java
-------------

```java

package ex1.is_a;

public class inheritance {

	public static void main(String[] args) {
//		누군가 배포한 binary실행파일(Exam.class)를 이용해 새로운 기능을 추가하여 (NewlecExam.java)사용
//		소스코드인(Exam.java)는 알 수 없음
//		만들고자 하는 것과 근접한 것을 is a 상속할 수록 생산성이 높아지고 비용 절감 효과가 있다
//		누군가 만든 Framework를 가져다 쓰기 때문에 흔하다는 단점이 있다
		
//		NewlecExam exam1 = new NewlecExam();
//		NewlecExam의 소스를 들여다 보지 않는 이상 틀로 만든 줄 모르고 NewlecExam이 만든걸로 이해한다 / .total(), .avg매서드도 NewlecExam만든걸로 이해한다


//		Exam과 NewlecExam 참조형식 차이 / 참조형식 참조변수 = new(생성자, 연산자) 객체형식();
		
// 	Exam exam = new NewlecExam(); 가능하다
//		NewlecExam객체는 Exam객체와 NewlecExam객체 두 가지 객체, 참조형식을 가지고 있다 / Exam형식으로 참조하면 NewlecExam객체의 일부인 Exam객체만 참조하는 것
		
//		NewlecExam exam = new Exam(); 에러난다
//		Exam객체는 Exam객체 한 가지 객체만 가지고 있다

		NewlecExam exam1 = new NewlecExam(1,1,1,1);
		System.out.println(exam1.total()); // 4가 나온다
//		하지만 Exam.java파일에 total()이 없고 NewlecExam.java파일에 오버라이드 total()만 있다면 실행 가능

		System.out.println(exam1.avg()); // 1.0
		
		Exam exam2 = new NewlecExam(1,1,1,1);
		System.out.println(exam2.total()); // 3이 나올 것 같지만 4가 나온다
//		자바는 참조형식(Exam)의 함수보다 객체 형식(NewlecExam)의 함수 호출을 우선시 한다 
//		하지만 Exam.java파일에 total()가 없고 NewlecExam.java파일에 오버라이드 total()만 있다면 실행 불가능 / Exam형식으로는 total()을 호출할 수 없기 때문
//		참조형식(Exam)이 갖고 있는 매서드에 한해서 호출 가능하지만 갖고 있지 않다면 호출 자체가 불가능 / 오버라이드 함수가 있다면 오버라이드 함수가 실행 됨

		System.out.println(exam2.avg()); // 1.0
		
	}

}

```

NewlecExam.java
-------------

```java

package ex1.is_a;

public class NewlecExam extends Exam {
//	exam(부모 상위클래스 기반클래스), NewlecExam(자식 하위클래스 확장클래스)

//	private Exam exam;
//	Exam은 부품이 아니라 틀이기 때문에 쓸 수 없다

	private int com;
//	소스코드를 알 수 없기에 Exam에 직접 com을 추가하지 못하고 is a 상속을 통해 Exam틀에 com만 추가

	public NewlecExam() {
//		kor = 1;
//		NewlecExam내에 kor이 정의되어 있지 않으므르 쓸 수 없다

//		this.serKor(kor);
//		불편하다 비효율적

//		this.com = 1;
		this(0, 0, 0, 0); 
//		생성자 호출하기 위해서는 갓 생성된 객체여야 한다 위에서 this.com을 사용하면 오류난다
	}

	public NewlecExam(int kor, int eng, int math, int com) {
		super(kor, eng, math);
//		super는 Exam객체 / super 사용되지 않은 갓 생성된 객체 / super() 부모 생성자 호출 > 오버로드 생성자 호출
		this.com = com;
//		부모객체는 부모의 기능으로 초기화 / NewlecExam이 생성한 com은 NewlecExam이 초기화
	}

//	Override / 고쳐쓰는 함수
//	ctrl + space 부모가 갖고 있는 함수 표시
//	부모에 있던 total 위에 자식 total을 덮어 쓴다 올라 탄다 / 부모에 있는 total을 못보게 함
//	total이 호출되면 먼저 NewlecExam(this)에서 total()이 있는지 확인 하고 없다면 Exam(super)에서 total()을 찾는다

	@Override
//	@Override가 있다면 해당 매서드는 무조건 @Override매서드 이기 때문에 오탈자를 잡아준다 / total2()면 새로운 매서드가 생성되는 것이 아닌 빨간줄이 그어진다
	public int total() {
		return super.total() + com;
//		그냥 kor + eng + math는 사용이 불가능하다 NewlecExam에 정의가 되어있지 않기 때문
//		getter를 사용한다 비효율적
//		super.를 생략하면 this.total()이므로 무한반복한다
	}


	public double avg() {
		return total() / 4.0;
	}

}

```

ExamConsole.java
-------------

```java

package ex1.is_a;

import java.util.Scanner;

public class ExamConsole {

	private Exam exam;

	public ExamConsole() {
		exam = new Exam();
	}
	
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

Exam.java
-------------

```java

package ex1.is_a;

public class Exam {
	private int kor;
	private int eng;
	private int math;

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
