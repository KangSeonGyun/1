---
title: Structure
tags: java
---

MainMemory
-------------

```java

package ex1;

public class MainMemory {

	public static void main(String[] args) {

//		메인메모리 RAM은 Data, Text로 구분
//		Data는 Heap, Stack / 선으로 구분되어 있는게 아니라 식당의 입석과 예약석과 비슷하다 예약석이 많아지면 입석을 줄이고 입석이 많아지면 예약석을 줄임
//		Text는 Instruction이 움직이며 한줄한줄 실행
		int total = 0;
		int kor = 2;
		int[] kors = new int[3];

//		메인메모리 Data - Stack 미리 선언되어야 할 예약어는 args total kor kors 4개
//		변수명, 배열의 이름은 Stack에 있다 / 배열은 Heap에 있다 배열의 이름이 사라지면 Heap에 남아있는 값은 가바지 값이 된다
	
	}

}

```

Structure
-------------

```java

package ex1;

public class Structure {

	static class Exam {
		int kor;
		int eng;
		int math;
	}
//	Exam 데이터 구조 정의

//	Exam exam 선언(이름 붙이기) exam 객체 만듦
//	객체란? 어떤 프로그래밍 언어가 내용(값 등등..)을 사용할 수 있게 해주는 메모리상에 대상이 되는 것

//	exam = new Exam() 참조

//	class 사용하는 이유 2가지 - 함수를 묶기위해 사용(구조화), 캡슐화 ?

	public static void main(String[] args) {
		Exam exam = new Exam(); // 선언과 참조 exam[kor][eng][math]
//		exam은 Stack에 만들어짐
//		new Exam()는 Heap에 만들어짐 변수선언 안됨 / .kor, .eng, .math은 Heap안에 있는 Exam()안에 있다

		exam.kor = 10;
		exam.eng = 20;
		exam.math = 30;

		printExam(exam);

		Exam[] exams = new Exam[3];
//		exams 이름의 배열이 만들어짐 / 이름(exams)은 Stack에 배열은 Heap에 만들어짐
//		new는 실행될 때 동적에 할당 Heap
//		Exam이라는 참조만 3개 준비 kor eng math 없다
//		Exam 객체가 아닌 배열이 만들어짐 or Exam형식의 변수가 3개만들어짐 Heap에 만들어 진다
//		()가 없으면 객체를 생성한 게 아님. 배열만 생성한 것. int[] arr = new int[5]; 처럼 생각
//		Exam[]형 배열형식이 3칸 있다

//		exams(Stack) > exams[[0][1][2]](Heap) / [0][1][2]생성 Heap

//		exams[0].kor = 30; // Error - NullPointerException / .kor이 뭔지 알 수 없다

		exams[0] = new Exam(); // 객체 만들어줌
//		[[kor][eng][math]]생성 (Heap)
//		exams(Stack) > exams[0](Heap) > exams[[[kor][eng][math]][1][2]](Heap)

		exams[0].kor = 10;
		exams[0].eng = 10;
		exams[0].math = 10;

		exams[1] = new Exam();
		exams[1].kor = 20;
		exams[1].eng = 20;
		exams[1].math = 20;

		exams[2] = new Exam();
		exams[2].kor = 30;
		exams[2].eng = 30;
		exams[2].math = 30;

		printExams(exams);

	}

	private static void printExam(Exam exam) {
		System.out.printf("kor:%d\n", exam.kor);
		System.out.printf("eng:%d\n", exam.eng);
		System.out.printf("math:%d\n", exam.math);
	}

	private static void printExams(Exam[] exams) {
		for (int i = 0; i < 3; i++) {
			Exam exam = exams[i];
			printExam(exam);
//			System.out.printf("kor:%d\n", exam.kor);
//			System.out.printf("eng:%d\n", exams[i].eng);
//			System.out.printf("math:%d\n", exams[i].math);
		}
	}

//	구조화의 문제점 2가지
//	코드를 나누는 기준이 명확하지 않다
//	함수의 고립도를 무너트린다

}

```
