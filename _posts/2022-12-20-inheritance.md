---
title: inheritance
tags: java
---
참고 Link: [ExamConsole][id]

[id]: https://ksg0000.github.io/2022/12/20/ExamConsole.html

참고 Link: [ExamV3][id]

[id]: https://ksg0000.github.io/2022/12/20/ExamV3.html


```java

package ex1.has_a;

// 상속(inheritance)
// 의존(dependency) 제품과 부품의 관계가 된다
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
