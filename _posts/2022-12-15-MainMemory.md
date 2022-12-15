---
title: MainMemory
tags: java
---

```java

package ex1;

public class MainMemory {

	public static void main(String[] args) {

//		메인메모리 RAM은 Data, Text로 구분
//		Data는 Heap, Stack 선으로 구분되어 있는게 아니라 식당의 입석과 예약석과 비슷하다 예약석이 많아지면 입석을 줄이고 입석이 많아지면 예약석을 줄임
//		Text는 Instruction이 움직이며 한줄한줄 실행
		int total = 0;
		int kor = 2;
		int[] kors = new int[3];

//		메인메모리 Data - Stack 미리 선언되어야 할 예약어는 args total kor kors 4개
//		변수명 배열의 이름은 Stack에 있다 배열은 Heap에 있다 배열의 이름이 사라지면 Heap에 남아있는 값은 가바지 값이 된다
		
		kors[1] = 3;

		total = total + 3 + 4;

	}
	static int total(int kor, int eng, int math) {
		
	}

}

```