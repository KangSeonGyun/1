---
title: ArraySwap
tags: java
---

```java

package ex1;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.PrintStream;
import java.util.Random;
import java.util.Scanner;

public class ArraySawp {

	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub

// 		1. nums라는 이름으로 정수 15개를 저장할 수 있는 배열 객체를 생성한다.

		int[] nums = new int[15];

//		2. res/data.txt 파일에 저장된 값들을 nums 배열에 로드한다.

		FileInputStream fis = new FileInputStream("res/data.txt");
		Scanner scanf = new Scanner(fis);

		for (int i = 0; i < 15; i++)
			nums[i] = scanf.nextInt();

		scanf.close();
		fis.close();

		System.out.println("로드 완료");

//		3. 0~14 범위의 랜덤값 2개를 얻어서  그 위치의 값을 서로 바꾼다. 그것을 50번 반복한다.

		Random rand = new Random();

		int a, b, temp;

		for (int i = 0; i < 50; i++) {
			a = rand.nextInt(15);
			b = rand.nextInt(15);
			temp = nums[a];
			nums[a] = nums[b];
			nums[b] = temp;
		}
		System.out.println("번호 섞기 완료");

// 		4. res/data-out.txt 파일로 배열의 값들을 저장

		FileOutputStream fos = new FileOutputStream("res/data-out.txt");
		/*
		 * 파일을 저장하려면 버퍼가 필요하다 파일아웃풋스트림 객체를 생성해준다 이름은 fos로 한다. fos가 무슨타입인지 알려주기 위해서
		 * FileOutputStream 타입명을 써줬다
		 * 
		 * fos 참조타입(참조변수 객체명), new 생성자(연산자) 사용해 인스턴스(개체) 생성
		 * 
		 * FileOutputStream을 이용해 이름 fis 객체생성
		 * 
		 * FileOutputStream 객체를 생성해서 이름은 fos로 지었다.
		 */
		PrintStream out = new PrintStream(fos);
//		객체를 이용한 개체 - 응용객체
// 		한 문자씩 내보내지 않고 문자열로 내보대기 위해 PrintStream을 이용

		for (int i = 0; i < 15; i++) {
			out.printf("%d", nums[i]);
			if (i != 14)
				out.print(' ');
		}

		out.close();
		fos.close();

		System.out.println("저장 완료");

		for (int i = 0; i < 14; i++)
			for (int j = 0; j < 14 - i; j++)
				if (nums[j] > nums[j + 1]) {
					temp = nums[j];
					nums[j] = nums[j + 1];
					nums[j + 1] = temp;
				}

		for (int i = 0; i < 15; i++) {
			System.out.printf("%d ", nums[i]);
		}

	}

}

```