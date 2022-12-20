---
title: encapsulationV2
tags: java
---
// 참고
Link: [ExamV2][id]

[id]: https://ksg0000.github.io/2022/12/20/ExamV2.html


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
		Exam exam1 = new Exam(1,2,3);
//		Exam exam1 = new Exam 여기 까지가 (객체 실체 인스턴스) 생성 / ()constructor로 구분할 수 있다
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
		exam1.print('=');
		exam1.print('=', 10);
	}

}

```
