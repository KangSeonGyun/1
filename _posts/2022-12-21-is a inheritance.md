---
title: is a inheritance
tags: java
---
참고 Link: [(is a)ExamConsole][id]

[id]: https://ksg0000.github.io/2022/12/20/(has-a)ExamConsole.html

참고 Link: [(is a)Exam][id]

[id]: https://ksg0000.github.io/2022/12/20/(has-a)Exam.html


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
