---
title: BasicFunction
tags: java
---

```java

package ex1.func;

public class BasicFunction {

	static int f1(int x) {
		return 3 + x * 23;
	}
//	f1 앞 int의 의미는 반환값의 형식이 int라는 의미

	static int add(int a, int b) {
		return a + b;
	}

	static void printSum(int sum) {
		System.out.println(sum);
	}
//	void 들어온 값 a를 아무것도 하지않고 소비해버림 return을 쓸 수 없죠.

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int result = 3 + 4 * 23 - 23 + 345 - 23;

		System.out.println(result);

		result = f1(4) - 23 + 345 - 23 + add(5, 9);
//		f1() 빨간 물결밑줄이 뜨면 정의가 되어있지 않다는

		System.out.println(result);

		printSum(add(4, 5));
//		add함수가 먼저 실행 됨 > printSum(9)

	}

}

```
