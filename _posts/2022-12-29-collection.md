---
title: Collection 
tags: java
---

Link: [Generic 2][id]

[id]: https://ksg0000.github.io/2022/12/28/generic2.html

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

public class Program {

	public static void main(String[] args) {
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

참고 Link: [Java - 자바 컬렉션 프레임워크][id2]

[id2]: https://st-lab.tistory.com/142