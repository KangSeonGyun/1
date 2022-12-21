---
title: has a inheritance
tags: java
---
참고 Link: [(has a)ExamConsole][id]

[id]: https://ksg0000.github.io/2022/12/20/(has-a)ExamConsole.html

참고 Link: [(has a)ExamV][id]

[id]: https://ksg0000.github.io/2022/12/20/(has-a)Exam.html


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
