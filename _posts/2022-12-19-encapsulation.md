---
title: encapsulation
tags: java
---
// 참고
Link: [Exam][id]

[id]: https://ksg0000.github.io/2022/12/19/Exam.html


```java

package ex1;

public class encapsulation {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
//		캡슐화
//		데이터 구조와 함수를 하나의 영역에 함께
//		함수와 함수가 사용된 영역을 같이 둠

//		객체 개체 짜장면, 내가 시킨 짜장면
//		자동차는 타입, 형식명 / 이름은 참조
//		자바에서 객체를 만들면 이름에 상관없이 hcode가 생성된다
		
//		객체지향은 우리가 머리속에 있는걸 그대로 표현하자
		
//		성적 객체를 준비한다.
//		성적 뉴렉성적 = new 성적();
		Exam exam1 = new Exam();
		
//		성적을 입력 받는다.
//		뉴렉성적.입력();
		inputExam(exam1);
		exam1.input(); // 실 세계의 표현방식으로 프로그래밍
		
//		Exam.print(exam); 이렇게 작성하면 안됨
		
//		성적을 출력한다.
//		뉴렉성적.출력();
		printExam(exam1); // 객체지향 스럽지 않은 함수
		exam1.print(); // 객체지향스러운 함수
//		객체 O 실체 O 인스턴스 O 통해 호출되는 함수들은 exam1을 넘겨받는다. 위와 같은 의미의 새로운 표기법
		
	}

	private static void printExam(Exam exam1) {
//		인스턴스 함수가 아니라는 의미로 static을 붙인다. this를 사용할 수 없다
		int kor = exam1.kor;
		
		System.out.printf("kor:%d\n", kor);
		System.out.printf("eng:%d\n", exam1.eng);
		System.out.printf("math:%d\n", exam1.math);
		
	}

	private static void inputExam(Exam exam1) {
		
	}

}

```
