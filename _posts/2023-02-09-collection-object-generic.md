---
title: Collection, Object, Generic 
tags: java
---

Program.java
-------------

```java

package com.newlecture.app.util;

public class Program {

	public static void main(String[] args) {
//		Collection, Object, Generic을 공부했다면 Iterator, for-each, 스트림 API에 대해서도 공부해보자
		
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
		
		num = intList.get(1);
		System.out.println(num); // java.lang.IndexOutOfBoundsException / 에러
		
//		----------------------------------------------------------------------------
		
//		Object 콜렉션
		ObjectList objectList = new ObjectList();
		
		objectList.add("hello");
		objectList.add(5);
		int size2 = objectList.size();
		System.out.println(size2); // 2
		
		objectList.clear();
		size2 = objectList.size();
		System.out.println(size2); // 0
		
		objectList.add(7);
		Object num2 = objectList.get(0);
		System.out.println(num2); // 7
		
		num2 = objectList.get(1);
		System.out.println(num2); // java.lang.IndexOutOfBoundsException / 에러
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
		if(current <= index)
			throw new IndexOutOfBoundsException();
//		사용자가 입력한 index가 current보다 크거나 같으면 예외처리
		
		return nums[index];
	}

}

```

Object.java
-------------

```java

package com.newlecture.app.util;
//Object - 최상위 추상 클래스 / Object형식 자체가 참조형식이다 / 모든 데이터를 목록으로 관리할 수 있는 자료형식

//Object - 참조변수는 공간을 갖고 있지 않다 / 객체(Class)는 참조가 되기때문에 Object형식으로 참조할 수 있다
//Object - 값은 공간에 담아야하므로 참조를 할 수 없다 / 값을 참조하려면 Wrapper Class로 Boxing이 필요하다

public class Object {

	public static void main(String[] args) {

//		Wrapper Class
//		byte, short, int, long, float, double, char, boolean
//		String은 값 형식이 아닌 참조형식이다

		int x = 3;
//		값 형식 / 3을 담고 있다

		Integer x2 = 3;
//		참조형식 / Auto Boxing이 일어나며 박스로 감싸진 3을 x2가 참조하고 있다
		Integer x3 = new Integer(3);
//		Manual Boxing
//		Boxing과 UnBoxing연산이 일어나므로 메모리 공간 차지 / 지양하자

		total(x, x2);
//		값 형식을 해당되는 Wrapper Class는 x2를 전달할 때 참조를 전달하는 것이 아닌 Auto UnBoxing하여 값을 전달한다
		int x4 = x2.intValue();
//		Manual UnBoxing
		total(x, x4);

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