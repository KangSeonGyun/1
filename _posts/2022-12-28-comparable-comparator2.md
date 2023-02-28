---
title: Comparable, Comparator 2
tags: java
---

Link: [Comparable, Comparator][id]

[id]: https://ksg0000.github.io/2022/12/28/comparable-comparator.html

Anonymous.java
-------------

```java

// 익명 객체 설명

public class Anonymous {
	public static void main(String[] args) {

		Rectangle a = new Rectangle();

//		익명 객체 1 / 클래스 이름으로 정의되지 않는 객체를 바로 익명 객체라 한다
		Rectangle anonymous1 = new Rectangle() {

			@Override
			int get() {
				return width;
			}
		};

		System.out.println(a.get()); // 20
		System.out.println(anonymous1.get()); // 10
		System.out.println(anonymous2.get()); // 6000
	}

//	익명 객체 2
	static Rectangle anonymous2 = new Rectangle() {
//	구현부에서 분명히 변수를 선언하기도 하고, Rectangle 클래스의 메소드 get()을 재정의(Override)를 했다
//	즉 annoymous1, 2는 Rectangle을 상속받은 하나의 새로운 Class라는 것이다 / 하지만 분명 새로운 Class인데 이름이 정의되지 않고 있다
//	이는 거꾸로 말하면, 이름이 정의되지 않기 때문에 특정 타입이 존재하는 것이 아니기 때문에 반드시 익명 객체의 경우는 상속할 대상이 있어야 한다는 것이다
//	이 때, 상속이라 함은 class의 extends 뿐만 아니라 interface의 implements 또한 마찬가지다.

//	클래스를 상속(구현)할 때, 이름만 다르게 하면 몇 개던 여러개를 생성할 수 있듯이, 익명 객체를 가리키는 변수명만 달리하면 몇 개든 자유롭게 생성할 수 있다

		int depth = 30;

		@Override
		int get() {
			return width * height * depth;
		}
	};
}

//------------------------------------------------

class Rectangle {

	int width = 10;
	int height = 20;

	int get() {
		return height;
	}
}

```

Anonymous2.java
-------------

```java

// Comparator 익명 객체

import java.util.Comparator;

public class Anonymous2 {
	public static void main(String[] args) {

		Student a = new Student(17, 2);
		Student b = new Student(18, 1);
		Student c = new Student(15, 3);

		int isBig = comp2.compare(b, c); // comp2 익명객체를 통해 b와 c객체를 비교한다.

		if (isBig > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		} else if (isBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		} else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}

//		-----------------------------------------------------------------

//		익명 객체 구현방법 1
		Comparator<Student> comp1 = new Comparator<Student>() {
			@Override
			public int compare(Student o1, Student o2) {
				return o1.classNumber - o2.classNumber;
			}
		};
	}

//	익명 객체 구현방법 2
	public static Comparator<Student> comp2 = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			return o1.classNumber - o2.classNumber;
		}
	};
}

//	----------------------------------------------------------------------

// 외부에서 익명 객체로 Comparator가 생성되기 때문에 클래스에서 Comparator을 구현 할 필요가 없어진다
class Student {

	int age; // 나이
	int classNumber; // 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

}

```

Anonymous3.java
-------------

```java

// Comparable 익명 객체

public class Anonymous3 {
	public static void main(String[] args) {

		Student a = new Student(17, 2);
		Student b = new Student(18, 1);

		int classBig = comp.compareTo(b); // Stduent b 객체는 comp의 30이랑 비교되는 것이다.
//		a.compareTo(b) 처럼 서로 다른 객체에 대한 비교가 불가능하다.

	}

//	학급 대소 비교 익명 객체
	public static Comparable<Student> comp = new Comparable<Student>() {
//	Comparable을 익명객체로 생성 할 필요도 없고 오히려 복잡해진다.
//	Comparable에서 자기 자신은 익명 객체가 될 것이다. Student객체가 아니라는 것이다
//	즉, 익명의 객체와 Student가 비교하는 것이지, Student와 Student가 비교되는 것이 아니라는 것이다
//	자기 동일한 타입의 자신의 객체와 어떤 객체를 비교하고자 하면 Comparable을 익명객체로 선언한다면 동일한 타입 비교는 불가능하다는 것이다.
		int a = 30;

		@Override
		public int compareTo(Student o) {
			return a - o.classNumber;
		}
	};
}

// --------------------------------------------------------------------

// Student 클래스 생략

```

참고 Link: [JAVA - Comparable 과 Comparator의 이해][id2]

[id2]: https://st-lab.tistory.com/243