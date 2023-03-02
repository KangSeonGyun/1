---
title: Comparable, Comparator
tags: java
---

Program.java
-------------

```java

// primitive 타입의 변수(byte, int, double 등등..)의 경우 부등호를 갖고 쉽게 두 변수를 비교할 수 있었다
// 객체는 사용자가 기준을 정해주지 않는 이상 어떤 객체가 더 높은 우선순위를 갖는지 판단 할 수가 없다
// 이러한 문제점을 해결하기 위해 Comparable 또는 Comparator가 쓰인다

// Comparable과 Comparator은 모두 인터페이스(interface) / 객체를 비교할 수 있도록 만든다
// Comparable은 compareTo(T o) / Comparator는 compare(T o1, T o2)을 반드시 구현해야 한다

// compareTo는 파라미터가 한 개 / compare은 파라미터가 두 개인 이유 / 비교 대상이 다르다
// Comparable은 자기 자신과 매개변수 객체를 비교 / Comparator는 두 매개변수 객체를 비교

public class Program {

	public static void main(String[] args) {

//		Comparable

		Student a = new Student(17, 2);
		Student b = new Student(18, 1);

		int isBig = a.compareTo(b); // a자기자신과 b객체를 비교한다

		if (isBig > 0) {
			System.out.println("a객체가 b객체보다 큽니다.");
		} else if (isBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		} else {
			System.out.println("a객체가 b객체보다 작습니다.");
		}

//		----------------------------------------------------------------------

//		Comparator

		Student2 aa = new Student2(17, 2);
		Student2 bb = new Student2(18, 1);
		Student2 cc = new Student2(15, 3);

		int isBig2 = aa.compare(bb, cc); // aa객체와는 상관 없이 b와 c객체를 비교한다
//		Comparator를 통해 compare 메소드를 사용려면 결국에는 compare메소드를 활용하기 위한 객체가 필요하게 된다
//		compare 메소드를 호출하기 위한 대상은 aa든 bb든 cc든 상관이 없다. 이 말을 조금 돌려서 생각해보면, 일관성이 떨어진다는 것이다
		Student2 comp = new Student2(0, 0);
		isBig2 = comp.compare(bb, cc);
//		비교만을 위해 Student 객체(comp)를 하나 더 생성해주는 방법도 있다
//		하지만 위 처럼 하면 age와 classNumber 변수는 굳이 쓸모가 없음에도 생성이 되어버린다는 단점이 있다
//		Comparator 기능만 따로 두고싶다면 익명 객체(클래스)를 활용해야 한다
//		Comparable, Comparator 2편으로...

		if (isBig2 > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		} else if (isBig2 == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		} else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}

	}

}

```

Student.java
-------------

```java

// Comparable

public class Student implements Comparable<Student> {

	private int age;
	private int classNumber;

	public Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

	@Override
	public int compareTo(Student o) {
//		compareTo 메소드를 보면 int값을 반환하도록 되어있다 / 즉 '값'을 비교해서 정수를 반환

		if (this.age > o.age) {
//		자기자신의 age가 o의 age보다 크다면 양수
			return 1;
		}

		else if (this.age == o.age) {
//		자기 자신의 age와 o의 age가 같다면 0
			return 0;
		}

		else {
//		자기 자신의 age가 o의 age보다 작다면 음수
			return -1;
		}

	}

//	---------------------------------------------------------------

	@Override
	public int compareTo(Student o) {
		return this.age - o.age;	
	}
//	두 값의 차를 반환해버리면 번거로운 조건식 없이 한방에 3개의 조건을 만족할 수 있다

//	단 주의해야 할 점이 있다
//	우리가 편리하게 두 수의 대소비교를 두 수의 차를 통해 음수, 0, 양수로 구분하여 구했지만, 여기에는 치명적인 단점이 있다
//	바로 뺄셈 과정에서 자료형의 범위를 넘어버리는 경우가 발생할 수 있기 때문이다 / 만약 해당 범위 밖을 넘게 되면 반대편의 값으로 넘어가게 된다

//	int 자료형은 32비트(4바이트) 자료형이며 표현 범위가 -2의 31승 ~ 2의 31승-1 으로, 이를 풀어쓰면 -2,147,483,648 ~ 2,147,483,647 이다
//	-2,147,483,648 - 1 = -2,147,483,649 일 것이다. 하지만, int 자료형에서 표현할 수 없는 수로 2,147,483,647으로 int형의 최댓값으로 반환한다
//	이렇게 주어진 범위의 하한선을 넘어버리는 것을 'Underflow' 라고 한다
//	반대로 2,147,483,647 + 1 = 2,147,483,648 일 것이다. 하지만 마찬가지로 int 자료형에서 표현할 수 없는 수로 -2,147,483,648 이 되어 int 형의 최솟값으로 반환된다
//	이렇게 주어진 범위의 상한선을 넘어버리는 것을 'Overflow' 라고 한다

//	즉 대소비교에 있어 이러한 Overflow 혹은, Underflow가 발생할 여지가 있는지를 반드시 확인하고 사용해야 한다
//	특히 primitive 값에 대해 위와 같은 예외를 만약 확인하기 어렵다면 <, >, == 으로 대소비교를 해주는 것이 안전하며 일반적으로 권장되는 방식이다

}

```

Student2.java
-------------

```java

// Comparator

import java.util.Comparator;
// import 필요

class Student2 implements Comparator<Student2> {

	private int age;
	private int classNumber;

	Student2(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

	@Override
	public int compare(Student2 o1, Student2 o2) {

		if (o1.classNumber > o2.classNumber) {
//			o1의 학급이 o2의 학급보다 크다면 양수
			return 1;
		}

		else if (o1.classNumber == o2.classNumber) {
//			o1의 학급이 o2의 학급과 같다면 0
			return 0;
		}

		else {
//			o1의 학급이 o2의 학급보다 작다면 음수
			return -1;
		}
	}
}

```

참고 Link: [JAVA - Comparable 과 Comparator의 이해][id]

[id]: https://st-lab.tistory.com/243