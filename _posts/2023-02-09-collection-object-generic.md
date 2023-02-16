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
		
		IntList list = new IntList();
		
		list.add(3);
		list.add(5);
		int size = list.size();
		System.out.println(size); // 2
		
		list.clear();
		size = list.size();
		System.out.println(size); // 0
		
		list.add(7);
		int num = list.get(0);
		System.out.println(num); // 7
		
		num = list.get(1);
		System.out.println(num); // java.lang.IndexOutOfBoundsException / 에러
		
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
//	정수형 콜렉션

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

		current = 0;
//		current만 0으로 바꿔주면 배열에 있는 값을 지워주는 코드가 의미 없어진다
//		1번으로 배열을 지우지 않고 current가 0일 때 .add로 값을 넣으면 덮어쓰기 된다
//		2번 위와 같은이유로 의미가 없다 / 새로운 배열을 참조하면 기존 배열은 가비지 컬렉터가 수거해 간다
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