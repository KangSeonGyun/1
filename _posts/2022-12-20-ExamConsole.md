---
title: ExamConsole
tags: java
---
// 참고
Link: [inheritance][id]

[id]: https://ksg0000.github.io/2022/12/20/inheritance.html


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
