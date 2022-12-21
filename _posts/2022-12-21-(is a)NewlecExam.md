---
title: (is a)NewlecExam
tags: java
---
// 참고
Link: [is a inheritance][id]

[id]: https://ksg0000.github.io/2022/12/21/is-a-inheritance.html

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
