---
title: Encapsulation2
tags: java
---

code Link: [Encapsulation][id]

[id]: https://ksg0000.github.io/2022/12/19/Encapsulation.html

Encapsulation2
-------------

```java

package ex1;

public class Encapsulation {

	public static void main(String[] args) {

		Exam exam1 = new Exam(1,2,3);

		exam1.reset();

		Exam.inputExam(exam1);

		exam1.input();

		Exam.printExam(exam1);

		exam1.print();
		exam1.print('=');
		exam1.print('=', 10);
	}

}

```

Exam2
-------------

```java

package ex1;

import java.util.Scanner;

public class Exam {

	private int kor;
	private int eng;
	private int math;

	public Exam() {

//		kor = 0;
//		kor을 0으로 초기화 하고 아래서 this(0, 0, 0);로 또 한번 초기화 할수 없기 때문에 안된다
		this(0, 0, 0); // 기본값
		
		kor = 0;
//		0으로 초기화되는게 아닌 값 0을 넣어주는 것이다
	}

//	오버로드 생성자
	public Exam(int kor, int eng, int math) {
		this.kor = kor;
		this.eng = eng;
		this.math = math;
//		kor = kor;로 작성하면 변역기가 지역변수 kor = 지역변수 kor인줄 안다
//		this를 사용해 명확히 표기해줘야 한다 / this를 생략하지 못하는 경우도 있다
	}

	public void reset() {
		kor = 30;
		eng = 30;
		math = 30;
//		private로 인해 외부에서 데이터 구조를 변경할 수 없기 때문에 Exam안으로 들어와서 리셋 해주는 함수 구현
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

		print('-', 30);
		
//		적재량이 더 적은 함수 print()는 기본 함수라고 한다
//		기본값('-', 30) 함수가 자의적으로 정한 값 / 다른 함수들도 기본값이 있다 값 변수 0 / 참조(래퍼런스) null

	}

//	오버로드 함수
	public void print(char ch) {
//		같은 이름의 함수가 3개지만 다른 함수다 / 적재량이 더 많은 함수를 오버로드 함수라고 한다

		print(ch, 30);
		
//		ch만 입력한 경우
	}

	public void print(char ch, int length) {
//		같은 이름의 함수가 3개지만 다른 함수다 / 적재량이 더 많은 함수를 오버로드 함수라고 한다

		int total = total();
		double avg = avg();

		System.out.printf("kor:%d\n", kor);
		System.out.printf("eng:%d\n", eng);
		System.out.printf("math:%d\n", math);
		System.out.printf("total:%d\n", total);
		System.out.printf("avg:%d\n", avg);

		for (int i = 0; i < length; i++)
			System.out.printf("%c", ch);

		System.out.println();
	}

	private double avg() {
//		print()할 때만 평균, 합계를 구하지 않게 분리
		return total() / 3.0;
	}

	private int total() {
		return kor + eng + math;
	}

}

```
