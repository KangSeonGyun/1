---
title: Abstraction
tags: java
---

Abstraction.java
-------------

```java

// 프로젝트명 - OOPJavaPrj
package com;

public class Abstraction {
//	처음만들 땐 같은 내용도 여러번 복사해서 작성한다 / 그 뒤 집중화, 구조화, 캡슐화 등을 통해 리팩토링(고도화 작업)한다

//	코드 집중화
//	여러 프로그램에서 존재하는 같은 코드를 한곳으로 모아 같이 사용
//	한 번의 수정으로 모든 곳의 코드를 바꿀 수 있다

//	리팩토링 - 결과의 변경 없이 코드의 구조를 재조정함

//	추상화 : 코드 집중화(X) -> 서비스 집중화(캡슐 단위의 공통 기능을 집중화) / 뼈대로써 확장해 쓸 수 있게 한다 / 객체화 new Exam(); 되는 것을 막는다
//	추상 클래스 : 객체(캡슐)들의 공통 분모(공통 자료형, 공통 서비스) / 객체화(실체화) 될 수 없는 공통 분모 / 기울임체로 표현

//	추상 클래스를 만드는 이유
//	1. 개체들의 공통서비스 집중화
//		공통 서비스화로 얻을 수 있는 장점
//		I. 코드 집중화 효과
//			추상 클래스를 부모로한 모든 클래스들을 일괄적으로 관리할 수 있다

//		II. 일괄처리 / 추상 클래스 Shape와 개체들(Circle, Rect, Line)이 있을 때 아래 코드가 가능하다 / 추상 클래스 Shape는 실체화 되선 안된다
//			Shape[] shapes = new Shape[10];
//			Shape객체가 만들어지지 않고 Shape을 참조하기 위한 참조변수 10개

//			Shapes[0] = new Circle();
//			Shapes[1] = new Rect();

//			for(int ... )
//				Shapes[i].move();

//	2. 향후 제품을 위한 프레임 워크
//		Exam 클래스와 Exam에서 기능을 추가해 확장한 클래스들 (NewlecExam, YBMExam, ...Exam)
//		Exam이 공통 분모기도 하지만 그 자체로 완전한 개체로써 사용이 된다면 추상화는 할 필요가 없다
//		Exam이 객체화 해서 프로그램을 만드는 것이 아닌 (NewlecExam, YBMExam, ...Exam)의 공통 분모로만 사용이 된다면 추상화(뼈대화)를 해야 한다

	public static void main(String[] args) {
//		Exam exam = new Exam(1, 1, 1);
//		abstract class Exam / 에러 - Cannot instantiate the type Exam / Exam형식은 실체화 될 수 없다

		NewlecExam exam = new NewlecExam();
//		exam을 부모로 확장한 NewlecExam는 객체화 할 수 있다

//		-----------------------------------------------------------------------------------------------------------------------------

//		추상 메소드

//		구현(코드) 자체가 공통은 아니지만 서비스 목록이 공통이다 / 추상 클래스에 구현을 둘 수 없다 / 공통 서비스, 공통 자료형으로 의미 있다

//		Shape[] shapes = new Shape[10];

//		Shapes[0] = new Circle();
//		Shapes[1] = new Rect();

//		public void paint() {
//			for(int ... )
//			Shapes[i].paint();
//		}

//		추상 클래스 Shape에 (Circle, Rect, Line)의 공통 기능 move()가 있다 / (Circle, Rect, Line)에 각각 공통되지 않는 기능 paint()가 있다
//		구현이 다 다른 paint()는 공통 기능 될 수 없다 / 따라서 Shape에서 일괄처리가 불가능하다
//		공통 기능이 아닌 paint()는 오류난다 / shape 기능 범주 내에 paint()가 없기 때문 / 따라서 서비스에 대한 골격(형식)만 추상 클래스 Shape에 공통으로 둔다
//		paint()메소드의 정의(뼈대)만 추상 클래스 Shape에 공통으로 둔다 / paint()의 기능 구현은 (Circle, Rect, Line)에 각각 Override한다
//		결국 paint()는 Shape형식으로 호출했지만 실제 객체(Circle, Rect)의 paint()가 호출된다

//		향후 제품을 위한 프레임 워크를 만들때도 추상 메소드를 만드나 일괄처리 목적이 아니다
//		(NewlecExam, YBMExam, ...Exam)는 (Circle, Rect, Line)처럼 동시에 사용되지 않는다 / Newlec에선 NewlecExam을 YBM에선 YBMExam을 사용한다
//		구현이 달라도 (NewlecExam, YBMExam, ...Exam)에 일괄 서비스로 total(), avg()가 들어간다면 공통 자료형에 들어가므로 추상 메소드를 구현해야 한다

//		추상 메소드로 공통 자료형을 만들어 두는 이유
//		Exam을 사용하는 ExamConsole을 만들어 둘 때 (NewlecExam, YBMExam, ...Exam)은 태어나기 전이다
//		ExamConsole이 Exam형식을 통해 공통 서비스(total(), avg())를 사용할 수 있을 때 ExamConsole에 print()메소드를 만들어 둘 수 있다
//		ExamConsole이 앞으로 태어날 어떠한 ...Exam 데이터 형식을 서비스 하려면 Exam에 공통 기능(total(), avg())을 뼈대만이라도 올려놔야 한다

//		-----------------------------------------------------------------------------------------------------------------------------

//		추상 메소드를 이용한 대표적인 GoF 디자인 패턴 팩토리 메소드(Factory Method)

		ExamConsole console = new NewlecExamConsole();
//		console.inputList();
//		3개의 배열을 만들어 3개를 한번에 입력받으면 inputList가 맞다
		console.input();
//		하나씩 받으므로 메소드명은 input
		console.print();

	}

}

```

Exam.java
-------------

```java

// 프로젝트명 - LibPrj
package com;

public abstract class Exam {
//	abstract 메소드를 가질 수 있는 class는 abstract 클래스 뿐이다
	int kor;
	int eng;
	int math;

	public Exam() {
		this(0, 0, 0);
	}

	public Exam(int kor, int eng, int math) {
		this.kor = kor;
		this.eng = eng;
		this.math = math;
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

//	추상 메소드

//	abstract public int total() {
//		return kor + eng + math;
//	}
//	구현 자체가 공통 분모로써 기능 구현이 아닌 공통 서비스로 목록화해야 한다 / 구현 자체가 의미 없다

	public abstract int total();
//	abstract - total()는 자식이 반드시 구현해야 한다

	protected int onTotal() {
		return kor + eng + math;
	}
//	private - 자식이 책임질 수 없다 / Override할 수 없다
//	public - 서비스 함수가 된다
//	protected - 자식만 볼 수 있고 외부 서비스로 노출 되지 않는다

	public abstract float avg();

}

```

NewlecExam.java
-------------

```java

// 프로젝트명 - OOPJavaPrj
package com;

// 다른 Java Project폴더인 LibPrj에 Exam.java가 있다 / Exam.java가 있는 LibPrj는 라이브러리로 가정 / NewlecExam은 Exam을 가져다 씀
// 현실성을 주기 위해 Project를 나눴다 / 한 쪽에선 사용되고 한 쪽에선 사용하는 두 개의 프로젝트가 서로 변화를 줘야한다

// reuse할 때 처럼 Exam을 바꿀 때 마다 배포(.jar)하고 Build Path - Libraries에 추가하면 매우 번거롭다
// 따라서 공부할 목적으로 Build Path - Projects에서 현재 내가 가지고 있는 Project 중 사용할 Project를 참고할 수 있다
// Project를 배포해서 Libraries에 추가한 것처럼 쓸 수 있다 / LibPrj에 있는걸 다 쓸 수 있다
// package을 com으로 통일해줬다 / package명이 같은 LibPrj에 있는 Exam을 자연스럽게 사용한다
// Project가 다르거나 jar파일이 다르거나 배포된 내용이 달라도 package명이 같으면 같은 패키지에 있다고 본다 / 따라서 import를 쓸 필요가 없다

public class NewlecExam extends Exam {
//	extends - is a 상속

	private int com;

	public NewlecExam() {
		this(0, 0, 0, 0);
	}

	public NewlecExam(int kor, int eng, int math, int com) {
		super(kor, eng, math);
		this.com = com;
	}

	public NewlecExam(int com) {
		super();
		this.com = com;
	}

	public int getCom() {
		return com;
	}

	public void setCom(int com) {
		this.com = com;
	}

//	추상 메소드

	@Override
	public int total() {
//		int total = getKor() + getKor() + getKor() + com;
//		avg()와 다르게 total()은 super.total() + com을 쓸 수 있었다
//		그렇다고 Exam.java에 있는 total()에 abstract을 없애고 기능을 구현하면 total()을 다시 구현해야하는 강제성이 사라진다
		int total = onTotal() + com;
//		super.total()을 대체할 수 있는 절충안
		return total;
	}

	@Override
	public float avg() {
		return total() / 4.0f;
	}

}

```

ExamConsole.java
-------------

```java

package com;

import java.util.Scanner;

public abstract class ExamConsole {
//	private ExamList list; / 구조적인 프로그래밍 후 수정

//	추상 메소드를 이용한 대표적인 GoF 디자인 패턴 팩토리 메소드(Factory Method)
//	팩토리 메소드 - 객체를 생성하는 부분을 자식에게 위임해서 자식이 객체 생성을 책임지게하는 방식

	Exam exam = makeExam(); // 임시
//	new Exam()을 할 수 없다 / Exam을 추상화 했기 때문
//	결국 만들어야 할 Exam객체는 앞으로 태어날 Exam형식의 Class를 객체로 생성해야 한다 (NewlecExam, YBMExam, ...Exam)

//	ExamConsole을 재사용 하는 2가지 방법
//	1. NewlecExam을 만들 때 ExamConsole.java 소스코드를 직접 가져와 new Exam()을 new NewlecExam()으로 수정한다

//	2. ExamConsole을 바이너리로 만들어 두고 new Exam()를 수정하게 만든다 / 소스코드 없이 재사용 가능 / 바림직한 방법
//	ExamConsole은 앞으로 태어날 NewlecExam을 사용할 것이다
//	NewlecExam만드는 쪽에서 new Exam()를 수정하게(책임지게) 만든다 / new Exam()를 책임지게 하려면 추상 메소드를 활용한다
//	ExamConsole은 추상 클래스가 되고 NewlecExam만드는 쪽에선 ExamConsole을 상속하고 있는 NewlecExamConsole도 만들게 된다

	public ExamConsole() {
//		list = new ExamList();
	}

	public void input() {
		Scanner scan = new Scanner(System.in);

		System.out.println("┌───────────┐");
		System.out.println("│ 성적 입력	│");
		System.out.println("└───────────┘");

		int kor;
		do {
			System.out.print("국어 : ");

			kor = scan.nextInt();

			if (kor < 0 || 100 < kor)
				System.out.println("입력 가능 범위 : 0 ~ 100");

		} while (kor < 0 || 100 < kor);

		int eng;
		do {
			System.out.print("영어 : ");

			eng = scan.nextInt();

			if (eng < 0 || 100 < eng)
				System.out.println("입력 가능 범위 : 0 ~ 100");

		} while (eng < 0 || 100 < eng);

		int math;
		do {
			System.out.print("수학 : ");

			math = scan.nextInt();

			if (math < 0 || 100 < math)
				System.out.println("입력 가능 범위 : 0 ~ 100");

		} while (math < 0 || 100 < math);

		exam.setKor(kor);
		exam.setEng(eng);
		exam.setMath(math);
//		생성자를 통해 값을 세팅할 수 없다 / 어떤 ...Exam이든 공통 분모로 Exam을 갖고 있으므로 setter를 통해 세팅

//		list.add(exam);

		onInput(exam);
//		onInput을 사용하지 않고 com을 입력받지 않았을 때도 total() / 4.0f을 한다
//		Exam에선 total(), avg()를 정의한 적 없고 NewlecExam에서만 total(), avg()을 정의했기 때문
//		Abstraction.java에서 ExamConsole console = new NewlecExamConsole(); 또한 객체형식을 우선시한 Override함수가 실행된다

//		해결방법
//		1. NewlecExamConsole에서 ExamConsole의 input(), print()를 Override한다
//		   하지만 이러면 ExamConsole을 재사용하는 것이 아닌 새로만드는 것과 같다
		
//		2. 이벤트 메소드를 활용한다
	}

	public void print() {
		print('-', 30);
	}

	public void print(char ch) {
		print(ch, 30);
	}

	public void print(char ch, int length) {
//		Exam exam = list.get(i);

		int total = exam.total();
		float avg = exam.avg();

		System.out.printf("kor:%d\n", exam.getKor());
		System.out.printf("eng:%d\n", exam.getEng());
		System.out.printf("math:%d\n", exam.getMath());
		onPrint(exam);

		System.out.printf("total:%d\n", total);
		System.out.printf("avg:%f\n", avg);

		for (int i = 0; i < length; i++)
			System.out.printf("%c", ch);

		System.out.println();
	}

	protected abstract Exam makeExam();

//	---------------------------------------------------------------------------------------------
	
//	이벤트 메소드 / 추상 메소드
//	kor, eng, math는 ExamConsole이 책임진다 / NewlecExamConsole은 확장되는 부분만 책임진다
	protected abstract void onInput(Exam exam);
//	makeExam()으로 만들어진 한 객체에 kor, eng, math, com을 담아야 하기 때문에 참조변수 exam을 파라미터로 넘겨줬다
//	추상 메소드는 미리 만들어 뒀기에 NewlecExam형식이 아닌 Exam형식으로 넘겨준다(?) / 소스코드 없이 재사용
//	출력할 내용은 참조변수명 exam이 갖고 있는 데이터다 / NewlecExamConsole에선 부모가 갖고 있는 exam을 이용해 출력해야 한다

	protected abstract void onPrint(Exam exam);

}

```

NewlecExamConsole.java
-------------

```java

package com;

import java.util.Scanner;

public class NewlecExamConsole extends ExamConsole {

	@Override
	protected Exam makeExam() {
		return new NewlecExam();
//		NewlecExam객체를 생성해 반환한다 / 반환 타입이 Exam이지만 NewlecExam의 부모이므로 가능하다
	}

	@Override
	protected void onInput(Exam exam) {
		NewlecExam newlecExam = (NewlecExam) exam;
//		Exam exam = new NewlecExam();
//		exam이 참조하고 있는 객체는 makeExam()으로 만들어진 NewlecExam이다
//		참조 되는 자료형식이 Exam이기 때문에 Exam범위 내에서만 메소드를 볼 수 있다 / 형변환이 필요하다

		Scanner scan = new Scanner(System.in);
		int com;
		do {
			System.out.print("컴퓨터 : ");

			com = scan.nextInt();

			if (com < 0 || 100 < com)
				System.out.println("입력 가능 범위 : 0 ~ 100");

		} while (com < 0 || 100 < com);

		newlecExam.setCom(com);
	}

	@Override
	protected void onPrint(Exam exam) {
		NewlecExam newlecExam = (NewlecExam) exam;

		int com = newlecExam.getCom();

		System.out.printf("com:%d\n", com);
	}

}

```