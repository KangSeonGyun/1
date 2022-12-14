---
title: ArrayTest
tags: java
---

```java

package ex1;

import java.io.PrintStream;
import java.util.Random;

public class ArrayTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		int[] ar1 = new int[5];

		ar1 = new int[7];

		int[][] ar2 = new int[3][5]; // 5칸짜리 배열이 3줄

		ar2 = new int[2][6];

		ar2[0] = new int[9]; // 된다

		int[][] ar3 = new int[3][]; // 된다

		// 자바는 기본이 톱니형 배열이다. 줄마다 칸 수가 일정하지 않음
		// c는 톱니형이 기본이 아니다.

//		int[] nums = new int[] {20,5,7,98,45,7,45,62,12,47};
		int[] nums = { 20, 5, 7, 98, 45, 7, 45, 62, 12, 47 }; // 같은 의미 최신버전

		for (int i = 0; i < 10; i++) {
			System.out.print(nums[i]);
			if (i != 9)
				System.out.print(", ");
		}
		System.out.println();

		// nums[]에서 [0]의 값과 [2]의 값 바꿔주기

		int a;
		a = nums[0];
		nums[0] = nums[2];
		nums[2] = a;

		for (int i = 0; i < 10; i++) {
			System.out.print(nums[i]);
			if (i != 9)
				System.out.print(", ");
		}
		System.out.println();

		// nums[]에서 [랜덤]의 값과 [랜덤]의 값 바꿔주기

		Random rand = new Random();

		int s = rand.nextInt(10); // 0~9사이 랜덤
		int d = rand.nextInt(10);
		int temp;

		temp = nums[s];
		nums[s] = nums[d];
		nums[d] = temp;

		for (int i = 0; i < 10; i++) {
			System.out.print(nums[i]);
			if (i != 9)
				System.out.print(", ");
		}
		System.out.println();

	}

}

```