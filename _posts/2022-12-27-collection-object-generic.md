---
title: Collection, Object, Generic 
tags: java
---

Program.java
-------------

```java

package com.newlecture.app.util;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

import com.newlecture.app.util.GBoundedTypeParameters.SaltClass;
import com.newlecture.app.util.GBoundedTypeParameters.Student;

// Collection, Object, Generic을 공부했다면 Iterator, for-each, 스트림 API에 대해서도 공부해보자
public class Program {

	public static void main(String[] args) {
//		정수형 콜렉션
		IntList intList = new IntList();

		intList.add(3);
		intList.add(5);
		int size = intList.size();
		System.out.println(size); // 2

		intList.clear();
		size = intList.size();
		System.out.println(size); // 0

		intList.add(7);
		int num = intList.get(0);
		System.out.println(num); // 7

		num = intList.get(1); // java.lang.IndexOutOfBoundsException / 에러

//		----------------------------------------------------------------------

//		Object 콜렉션
		ObjectList objectList = new ObjectList();

		objectList.add("hello");
//		모든 데이터를 담을 수 있다
		objectList.add(5);
		int size2 = objectList.size();
		System.out.println(size2); // 2

		objectList.clear();
		size2 = objectList.size();
		System.out.println(size2); // 0

		objectList.add(7);
		int num2 = (Integer) objectList.get(0);
//		값을 꺼낼 때 마다 어떤 형식인지 알아내서 형변환을 해야 함 / 범용 자료형식(Object)의 문제점
//		Object형으로 반환 한다는 것은 참조를 반환한다는 것이다 / 따라서 참조형식을 형변환 해줬다
//		결국 형변환 → UnBoxing / int num2 = new Integer(7); → int num2 = 7;
		System.out.println(num2); // 7

//		----------------------------------------------------------------------

//		Generic
		GList<Integer> gList = new GList<>();
//		자료형에다가 형식을 넘겼다 / 형식을 여러개 넘길 수 있다 / 객체생성 시 형식을 생략해도 됨 / 앞 형식을 따라감
//		클래스 내부에서 지정하는 것이 아닌 외부에서 사용자에 의해 지정되는 것
		
//		int는 안 된다 / Object로 만들어진 것의 형식명이기 때문에 변환이 될 수 있어야 한다 / get()에서 return시 형변환이 일어남
//		Object로 형식변환이 가능한 Wrapper Class형식으로 해야한다

		gList.add(3);
//		정수만 담을 수 있다
		gList.add(5);
		gList.add(7);
		gList.add(7);
		gList.add(7);
		gList.add(7);
		gList.add(7);

		int num3 = gList.get(0);
//		참조형식을 형변환 해줄 필요가 없다 / get()의 return에서 형변환 함
		System.out.println(num3); // 3

		for (int i = 0; i < gList.size(); i++)
			System.out.println(gList.get(i));
//		가변길이 배열확인

//		----------------------------------------------------------------------
		
//		Generic Method
		
		GMethod.genericMethod(3); // The method genericMethod(int) is undefined for the type GMethod
//		GMethod 객체가 생성되기 전에 genericMethod에 접근할 수 있으나 유형을 지정할 방법이 없어 에러남
		
		GMethod.genericMethod2(3);
		System.out.println("<E> returnType : " + GMethod.genericMethod2(3.0).getClass().getName()); // <E> returnType : java.lang.Double
		
//		----------------------------------------------------------------------
		
//		Bounded Type Parameters in Generics
		
		GBoundedTypeParameters<Double> a1 = new GBoundedTypeParameters<Double>();
		 
		GBoundedTypeParameters<String> a2 = new GBoundedTypeParameters<String>(); // Bound mismatch
		
//		---
		
		SaltClass2<Student> a = new SaltClass2<Student>();
		
		SaltClass4<Student2> b = new SaltClass4<Student2>();
		
//		----------------------------------------------------------------------

//		Java Collection 프레임워크 (Set, List, Queue)
//		Collection 메소드 - add(e), clear(), size(), contains(o)
//		List - get(index) 메소드가 있다

//		Map - Collection이긴 한데 정의하는 함수의 형태가 다르다 / 데이터 구조가 다르기 때문이에 함수도 달라진 것
//		Map 메소드 - put(k,v), clear(), containsKey(k), get(k)

//		선형 데이터구조
//		데이터를 찾아야 할 때 처음부터 끝까지 다 찾아봐야 한다(Full Scan)

//		비선형 데이터구조 : Tree 모양
//		물리적으론 선형이지만 데이터를 구분지어 넣는다 / 도서관 책 배열을 만들 때 적합하다

//		선형 데이터구조 : 링크(참조, 포인터 등)로 연결된 데이터
//		값과 참조를 하나의 공간으로 만듦 / 값이 다음 값을 가르키고 있다
//		1 → 2 → 3 / 2가 지워지면 1 → 3 / 삽입, 삭제가 자유롭다

//		Java Collection 3대 계보
//		Set - HashSet / List - ArrayList / Map - HashMap

//		세 콜렉션의 차이

//		1.식별자
//			List는 값이 들어가면 별도의 식별자가 붙는다 / 식별자 - index / 가변길이 배열
//			Set은 값 자체가 Key이므로 식별자가 중복될리 없다 / 따라서 식별자가 없다 / 데이터 중복제거용
//			Map은 스스로가 값으로 식별자를 갖고 있다 / 식별자 - key / 속성과 값으로 구분된 데이터 집합의 객체
		List<Integer> list = new ArrayList<>();
//		참조형은 Interface로 함
		list.add(3);
		list.add(5);
		list.add(6);
		list.add(7);
		list.add(7);
		list.add(7);
		System.out.println(list.get(5)); // 7
		System.out.println(list.size()); // 6

		Set<Integer> set = new HashSet<>();
		set.add(3);
		set.add(5);
		set.add(6);
		set.add(7);
		set.add(7);
		set.add(7);
		System.out.println(set.add(7)); // false
//		이미 있는 7은 중복될 수 없다
		System.out.println(set.size()); // 4

		Map<String, Integer> map = new HashMap<>();
		map.put("id", 3);
//		Key - id / Value - 3
		map.put("hit", 12);
		System.out.println(map.get("id")); // 3
	}

}

```

IntList.java
-------------

```java

package com.newlecture.app.util;
// Collection - 데이터를 수집하고 관리해주는 객체 / 가변길이 배열이 필요할 때 사용하는 객체

// 직접 사용하는 배열을 서비스 개념으로 쉽게 사용하게 해준다 / 배열을 직접 사용한다면 배열의 크기는 고정적이고 필요에 따라 늘릴 수 없다
// Collection을 사용한다면 배열을 사용할 필요가 없다 / 공간의 크기와 값이 어디에 저장이되는지 관심을 가질 필요가 없다

// Collection을 사용하는 이유
// 데이터 관리를 직접할 필요가 없다 / 배열의 크기가 부족할 때 배열을 새로만들고 값을 옮겨주는 서비스를 통해 배열의 크기를 가변적으로 바꿔준다
// 고정크기 배열의 사용하지 않은 공간은 메모리 공간만 차지한다

public class IntList {
//	정수형 콜렉션 / 정수만 담을 수 있다

	private int[] nums;
	private int current;

	public IntList() {
		nums = new int[3];
		current = 0;
	}

	public void add(int num) {
//		배열에 값 추가
		nums[current] = num;
		current++;
	}

	public void clear() {
//		배열에 있는 모든 값 지우기

//		배열에 있는 모든 값 지우는 방법 2가지
//		1
		for (int i = 0; i < current; i++)
			nums[i] = 0;

//		2
		nums = new int[3];
//		새로운 배열을 참조하면 기존 배열은 가비지 컬렉터가 수거해 간다

		current = 0;
//		current만 0으로 바꿔주면 배열에 있는 값을 지워주는 코드가 의미 없어진다
//		1번으로 배열을 지우지 않고 current가 0일 때 .add로 값을 넣으면 덮어쓰기 된다
//		2번도 위와 같은이유로 의미가 없다 / 
//		size(), get()도 current를 이용하므로 배열의 모든 값을 지우는 것이 의미없다
	}

	public int size() {
//		배열이 현재 갖고있는 데이터 개수를 알림
		return current;
	}

	public int get(int index) {
		if (current <= index)
			throw new IndexOutOfBoundsException();
//		사용자가 입력한 index가 current보다 크거나 같으면 예외처리

		return nums[index];
	}

}

```

ObjectClass.java
-------------

```java

package com.newlecture.app.util;

// Object - 최상위 추상 클래스 / Object형식 자체가 참조형식이다 / 모든 데이터를 목록으로 관리할 수 있는 자료형식
// Object - 참조변수는 공간을 갖고 있지 않다 / 객체(Class)는 참조가 되기때문에 Object형식으로 참조할 수 있다
// 값은 공간에 담아야하므로 참조변수에 담을 수 없다 / 값을 참조하려면 Wrapper Class로 Boxing(공간 만들기)이 필요하다

// Wrapper Class
// byte, short, int, long, float, double, char, boolean
// String은 값 형식이 아닌 참조형식이다

public class ObjectClass {
//	클래스명을 Object로 하면 안 된다

	public static void main(String[] args) {

		int x = 3;
//		값 형식 / 3을 담고 있다

		Object obj = 3;
		Integer y = 3;
//		참조형식 / Auto Boxing이 일어나며 박스로 감싸진 3을 참조변수가 참조하고 있다
		Object obj2 = new Integer(3);
		Integer y2 = new Integer(3);
//		Manual Boxing
//		Boxing과 UnBoxing연산이 일어나므로 메모리 공간 차지 / 지양하자

		total(x, y);
//		값 형식을 해당되는 Wrapper Class는 y를 전달할 때 참조를 전달하는 것이 아닌 Auto UnBoxing하여 값을 전달한다
		int y3 = y.intValue();
//		Manual UnBoxing
		total(x, y3);

	}

	static int total(int x, int y) {
		System.out.println(x + y);
		return x + y;
	}

}

```

ObjectList.java
-------------

```java

package com.newlecture.app.util;

public class ObjectList {
//	Object 콜렉션 / 모든 데이터를 담을 수 있다

	private Object[] nums;
	private int current;

	public ObjectList() {
		nums = new Object[3];
		current = 0;
	}

	public void add(Object num) {
//		배열에 값 추가
		nums[current] = num;
		current++;
	}

	public void clear() {
//		배열에 있는 모든 값 지우기
		current = 0;
	}

	public int size() {
//		배열이 현재 갖고있는 데이터 개수를 알림
		return current;
	}

	public Object get(int index) {
		if (current <= index)
			throw new IndexOutOfBoundsException();
//		사용자가 입력한 index가 current보다 크거나 같으면 예외처리

		return nums[index];
	}

}

```

GList.java
-------------

```java

package com.newlecture.app.util;

public class GList<T> {
//	범용 콜렉션의 장점과 특화된 클래스의 장점을 모두 겸비한 Generic

	private Object[] data;
	private int current;
	private int capacity;
	private int amount;

	public GList() {
		capacity = 3;
		amount = 5;
		data = new Object[capacity];
//		자바는 객체생성 시 Object형이 기본이므로 data = new T[capacity];는 안 된다
		current = 0;
	}

	public void add(T num) {
//		공간이 부족하다면 공간을 늘려준다
		if (capacity <= current) {
//			current는 저장될 다음 위치를 가르키므로 capacity와 같으면 공간이 부족한 것이다
			capacity += amount;
			Object[] temp = new Object[capacity];

			for (int i = 0; i < current; i++)
				temp[i] = data[i];

			data = temp;
		}

//		배열에 값 추가
		data[current] = num;
		current++;
	}

	public void clear() {
//		배열에 있는 모든 값 지우기
		current = 0;
	}

	public int size() {
//		배열이 현재 갖고있는 데이터 개수를 알림
		return current;
	}

	public T get(int index) {
		if (current <= index)
			throw new IndexOutOfBoundsException();
//		사용자가 입력한 index가 current보다 크거나 같으면 예외처리

		return (T) data[index];
//		get메소드로 반환할 때 T형으로 변환해 준다
	}

}

```

GMethod.java
-------------

```java

package com.newlecture.app.util;

public class GMethod<E> {

	static E genericMethod(E o) { // Cannot make a static reference to the non-static type E
//	클래스와 같은 E 타입이더라도 static 메소드는 객체가 생성되기 이전 시점에 메모리에 먼저 올라가기 때문에 E 유형을 클래스로부터 얻어올 방법이 없다.
//	따라서 제네릭이 사용되는 메소드를 정적메소드로 두고 싶은 경우 제네릭 클래스와 별도로 독립적인 제네릭이 사용되어야 한다
		return o;
	}

//	----------------------------------------------------------------------------------------------

//	[접근 제어자] <제네릭타입> [반환타입] [메소드명]([제네릭타입] [파라미터]) {
//	텍스트
//	}
//	클래스와는 다르게 반환타입 이전에 <> 제네릭 타입을 선언한다.
//	파라미터 타입에 따라 제네릭타입이 결정

	public static <E> E genericMethod2(E o) {
//	해당 메소드의 E타입은 제네릭 클래스의 E타입과 다른 독립적인 타입이다
		return o;
	}

}

```

GBoundedTypeParameters.java
-------------

```java

package com.newlecture.app.util;

//	Bounded Type Parameters in Generics

//	참조타임을 특정 범위 내로 좁혀서 제한
//	extends 와 super, 그리고 ?(물음표)다

//	?는 와일드 카드라고 해서 쉽게 말해 알 수 없는 타입
//	한마디로 어떤 타입이든 상관 없다는 의미다 당신이 String을 받던 어떤 타입을 리턴 받던 신경쓰지 않는다
//	이는 보통 데이터가 아닌 '기능'의 사용에만 관심이 있는 경우에 <?>로 사용할 수 있다
//	또한 특정 타입에 대해 참조할 필요가 없을 경우에는 와일드 카드가 유용하게 작용합니다

//	<K extends T>	// T와 T의 자손 타입만 가능 (K는 들어오는 타입으로 지정 됨)
//	<K super T>	// T와 T의 부모(조상) 타입만 가능 (K는 들어오는 타입으로 지정 됨)

//	<? extends T>	// T와 T의 자손 타입만 가능
//	<? super T>	// T와 T의 부모(조상) 타입만 가능
//	<?>		// 모든 타입 가능. <? extends Object>랑 같은 의미

//	----------------------------------------------------------------------------

//	이 때 주의해야 할 게 있다. <K extends T>와 <? extends T>는 비슷한 구조지만 차이점이 있다
//	'유형 경계를 지정'하는 것은 같으나 경계가 지정되고 K는 특정 타입으로 지정이 되지만, ?는 타입이 지정되지 않는다는 의미다
//	정확히는 와일드 카드를 쓸 경우 타입을 참조할 수가 없습니다

//	유형을 지정하는경우에는 자바의 최상위 객체인 Object를 제외하고는 모두 상계(상한선)이 제한되지만
//	와일드카드에서는 상한 하한 모두에서 쓸 수 있다. 그렇다보니 대개는 와일드카드는 super에서 많이 쓰인다

//	<K extends Number>
//	K는 Number와 이를 상속하는 Integer, Short, Double, Long 등의
//	타입이	지정될 수 있으며, 객체 혹은 메소드를 호출 할 경우 K는
//	지정된 타입으로 변환이 된다.

//	<? extends T> // T와 T의 자손 타입만 가능
//	T를  Number로 지정한다면 ?는 Number와 이를 상속하는 Integer, Short, Double, Long 등의
//	타입이 지정될 수 있으며, 객체 혹은 메소드를 호출 할 경우 지정 되는 타입이 없어 타입 참조를 할 수는 없다

//	----------------------------------------------------------------------------

public class GBoundedTypeParameters<K extends Number> {
}
//	대표적인 예로는 제네릭 클래스에서 수를 표현하는 클래스만 받고 싶은 경우가 있다
//	Integer, Long, Byte, Double, Float, Short 같은 래퍼 클래스들은 Number 클래스를 상속

class SaltClass<E extends Comparable<? super E>> {
}
//	super는 해당 객체가 업캐스팅(Up Casting)이 될 필요가 있을 때 사용한다
//	사과와 딸기는 종류가 다르지만, 둘 다 '과일'로 보고 자료를 조작해야 할 수도 있다
//	조금 더 현실성 있는 예제라면 제네릭 타입에 대한 객체비교가 있다
//	특히 PriorityQueue(우선순위 큐), TreeSet, TreeMap 같이 값을 정렬하는 클래스
//	특정 제네릭에 대한 자기 참조 비교를 하고싶을 경우 대부분 공통적으로 위와 같은 형식을 취한다

//	--------------------------------------------------------------------------

//	E extends Comparable 부터 한 번 분석해보자

//	extends는 앞서 말했듯 extends 뒤에오는 타입이 최상위 타입이 되고, 해당 타입과 그에 대한 하위 타입이라고 했다
//	그럼 역으로 생각해보면 이렇다. E 객체는 반드시 Comparable을 구현해야한다는 의미

class SaltClass2<E extends Comparable<E>> {
//	SaltClass2의 E는 Student가 되어야 한다
//	Comparable<Student>의 하위 타입이어야 하므로 거꾸로 말해 Comparable을 구현해야한다는 의미인 것이다
}

class Student implements Comparable<Student> {
	@Override
	public int compareTo(Student o) {
		return 0;
	};
}

//	--------------------------------------------------------------------------

class SaltClass3<E> implements Comparable<E> {
//	위 SaltClass2와는 다른 의미
//	E타입에 대해 비교를 구현하겠다는 의미로, E 타입에 대해 Override하여 compareTo를 구현해야 한다
	@Override
	public int compareTo(E o) {
		return 0;
	}
}

//	--------------------------------------------------------------------------

//	그러면 왜 Comparable<E> 가 아닌 <? super E> 일까?

class SaltClass4<E extends Comparable<? super E>> {
}

class Person {
}

class Student2 extends Person implements Comparable<Person> {
//	학생보다 더 큰 범주의 클래스인 사람(Person)클래스를 둘 수도 있다
//	Student2가 Person을 상속받고 있다
	@Override
	public int compareTo(Person o) {
		return 0;
	};
}
//	Comparable 구현부인 comparTo에서 Person 타입으로 업캐스팅(Up-Casting) 했다

//	만약 class SaltClass4 <E extends Comparable<E>>라면?
//	SaltClass4<Student2> b 객체가 타입 파라미터로 Student2를 주지만, Comparable에서는 그보다 상위 타입인 Person으로 비교한다
//	따라서 Comparable<E>의 E인 Student보다 상위 타입 객체이기 때문에 제대로 정렬이 안되거나 에러가 날 수 있다 / cast하지 못했다는 에러등
//	E 객체의 상위 타입, 즉 <? super E> 을 해줌으로써 위와같은 불상사를 방지할 수가 있는 것이다
//	즉, <E extends Comparable<? super E>> 는 쉽게 말하자면 E 타입 또는 E 타입의 슈퍼클래스가 Comparable을 의무적으로 구현해야 하며
//	super클래스타입으로 Up Casting이 발생하더라도 안정성을 보장받을 수 있다.

//--------------------------------------------------------------------------

//	재귀적 타입 바운드

class Water {
}

class Oil {
}
//	예로들어 물이라는 클래스가 있고, 기름이라는 클래스가 있다고 가정
//	일반적으로(우리가 아는 상식선에선) 두 클래스를 서로 비교하지는 않겠죠

interface Liquid {
}
//	근데, 만약 사용자가 좀 더 큰 추상적 계층으로 "액체"라고 다음과 같이 묶었다고 해봅시다

//	그리고, 액체를 구현하는 두 클래스는 다음과 같이 될 겁니다
class Water implements Liquid {
}

class Oil implements Liquid {
}

//	더 나아가, 각 클래스는 compareTo를 쓰기위해 각각 Comparable까지 구현했다고 해봅시다
class Water implements Liquid, Comparable<Water> {
	@Override
	public int compareTo(Water o) {
		return 0;
	}
}

class Oil implements Liquid, Comparable<Oil> {
	@Override
	public int compareTo(Oil o) {
		return 0;
	}
}

//	그러면 두 개가 공통적인 Liquid 인터페이스를 상속받고,각 클래스는'동일 한 타입에 대해서' 비교 가능하다는 것 까지는 가능하겠죠.
//	예로들어 water1.compareTo(water2); 이렇게 말이죠.

//	하지만 만약에 액체라는 공통된 속성에 대해 이를 구현하는 클래스들이 Comparable을 각각 구현하는 것을 좀 줄이고 하나의 속성으로 대체를 할 수 있으려면 어떻게 해야할까요?
//	쉽게 말해서 어차피 공통된 속성에 대해 비교를 할 건데, 굳이 다른 클래스들에서 두 번 반복해서 작성해줄 필요가 있냐라는 것이죠.

//	해법은 간단합니다.저 Liquid라는 인터페이스를 하나의 제네릭 클래스로 만들어서 하나의 클래스에 대한 Comparable<T> 을 만들어 주는 것이죠

//	그럼 다음과 같이 만들 수 있습니다
class Liquid<T> implements Comparable<T> {
	@Override
	public int compareTo(T o) {
		return 0;
	}
}

class Water extends Liquid2<Water> {
}

class Oil extends Liquid2<Oil> {
}

//	그러면 Liquid에서의 Comparable을 구현한 compareTo는 각각의 클래스에서 Liquid 이고, 이 클래스를 Water와 Oil이 각각 상속받고 있으니,
//	두 클래스 모두 Comparable의 compareTo를 사용 할 수 있습니다.

//	다만, 여기에서도 한계점이 있습니다. 바로 서로 다른 자손클래스끼리는 비교할 수 없다는 것이죠.
//	쉽게말해 water.comparTo(oil)이 안된다는 것입니다.만약에 물, 기름 상관없이 모든 액체클래스를 상속받는 클래스들이 서로 비교가 가능하게 하려면 어떻게 해야할까요?

//	이 질문의 답을 하기 전에 한 가지 먼저 다시 짚고 넘어가죠.<T extends(Type)>은 무엇이었을까요?
//	제너릭을 제한, 또는 경계를 정해주는 것입니다. 제 포스팅에서 예시로 들었듯<K extends Number> 을 하면 Number 및 그의 자손 타입들까지만 한정한다는 것이었죠.

//	다시 한 번 보죠.
class Liquid<T> implements Comparable<T> {
	@Override
	public int compareTo(T o) {
		return 0;
	}
}

//	이 것은 Liquid 클래스에 타입을 인자로 넘기면 해당 타입(T) 대한 타입 비교가 될 수 있습니다.
//	이는 단일 유형이기 때문에 우리는 더 확장하여 exteds 와 super을 이용한 경계 설정을 할 수 있었습니다.

//	제 포스팅 처럼 T extends Number 라고 한다면, Number 클래스 및 그 하위 클래스 타입들 까지 허용을 한다는 의미였죠.
//	그럼 이렇게 볼 수 있습니다.
//	"T에게 Number 및 하위 클래스 타입이라고 알려준다." 또는 "T의 상한경계(가장 높은 타입) Number다"

//	그럼 본 문제로 돌아와 액체를 상속하는 모든 클래스에 대해 비교가 가능하게 하려면 어떻게 해야할까요?
//	T에게 자기 자신 및 그 하위 클래스들까지 포함하라고 알려주어야 하지 않을까요?

//	그러면 T extend ( ??? ) 이 물음표에 무언가가 와야겠죠?
//	근데, 우리가 앞서 Liquid 클래스는 Liquid<T> 로 된 제네릭 클래스입니다.

//	그러면 물음표에 무엇이 올까요?
//	그대로 오면 되는 것이죠.

//	즉, 아래 클래스에서
class Liquid<T> implements Comparable<T> {
	@Override
	public int compareTo(T o) {
		return 0;
	}
}

//	상한 경계 클래스는 자신인 Liquid<T>이므로, 다음과 같은 클래스로 바뀌게 됩니다.
class Liquid<T extends Liquid<T>> implements Comparable<T> {
	@Override
	public int compareTo(T o) {
		return 0;
	}
}

//	T 타입은 클래스 자신을 포함한 그 하위 클래스 타입들이 올 수 있다는 의미가 되는 것이죠
//	그렇게 된다면, Liquid를 상속하는 모든 클래스들에 대해 T는 모두 자기 유형을 넣을 수 있게 되는 것입니다
//	(이는 꼭 class뿐만 아니라 제네릭 메소드에서도 마찬가지로 활용이 가능합니다.)

```

참고 Link: [JAVA - 제네릭(Generic)의 이해][id]

[id]: https://st-lab.tistory.com/153