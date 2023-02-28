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

//		num = intList.get(1);
		System.out.println(num); // java.lang.IndexOutOfBoundsException / 에러

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

//		num2 = (Integer) objectList.get(1);
		System.out.println(num2); // java.lang.IndexOutOfBoundsException / 에러

//		----------------------------------------------------------------------

//		Generic
		GList<Integer> gList = new GList<>();
//		자료형에다가 형식을 넘겼다 / 형식을 여러개 넘길 수 있다 / 객체생성 시 형식을 생략해도 됨 / 앞 형식을 따라감
//		int는 안 된다
//		Object로 만들어진 것의 형식명이기 때문에 변환이 될 수 있어야 한다 / get()에서 return시 형변환이 일어남
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

참고 Link: [JAVA - 제네릭(Generic)의 이해][id]

[id]: https://st-lab.tistory.com/153