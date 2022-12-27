---
title: ElevatorController
tags: java
---

ElevatorController
-------------

```java

package com.elevator.service;

public class ElevatorController {

	public static void main(String[] args) {
//		엘리베이터는 4명이 탑승이 가능하며, 총 3개 층으로 이루어져있음.
//		탑승객은 목표 층을 입력하여 해당 층이 되면, 자동으로 내리게 구현.
//		엘리베이터는 무조건 올라갔다가 내려가는 형태로 이동.

		ElevatorService es = new ElevatorService();

		System.out.println("---- Welcome Elevator ----");

		es.run();

	}

}

```

ElevatorService
-------------

```java

package com.elevator.service;

import java.util.Scanner;

public class ElevatorService {

	int[][] elevator;
	int curFloor;
	Boolean up;

	public ElevatorService() {
		elevator = new int[3][4];

		for (int y = 0; y < 3; y++)
			for (int x = 0; x < 4; x++)
				elevator[y][x] = 9;

		curFloor = 0;
	}

	public void run() {

		Scanner sc = new Scanner(System.in);

		label: while (true) {

			System.out.println("메뉴를 선택하세요.");
			System.out.println("1.탑승  2.이동  3.탑승현황  4.종료");

			Boolean empty = elevator[curFloor][0] != 9 && elevator[curFloor][1] != 9 && elevator[curFloor][2] != 9
					&& elevator[curFloor][3] != 9;

			switch (sc.nextInt()) {
			case 1:
				if (empty)
					System.out.println("탑승할 공간이 없습니다.");
				else
					take();
				break;
			case 2:
				move();
				break;
			case 3:
				status();
				break;
			case 4:
				System.out.println("엘리베이터 운행을 종료합니다.");
				break label;
			default:
			}

		}
	}

	private void status() {
		int count = 0;

		for (int i = 0; i < 4; i++)
			if (elevator[curFloor][i] != 9)
				count += 1;

		System.out.println("---- 탑승 현황 ----");
		System.out.printf("현재 탑승 인원은 %d명 입니다.\n", count);
		System.out.printf("현재 층수는 %d층 입니다.\n", curFloor + 1);
	}

	private void move() {
		int pastFloor = curFloor;

		if (curFloor == 0)
			up = true;
		else if (curFloor == 2)
			up = false;

		if (up)
			curFloor += 1;
		else
			curFloor -= 1;

		for (int x = 0; x < 4; x++) {
			elevator[curFloor][x] = elevator[pastFloor][x];
			elevator[pastFloor][x] = 9;
			if (elevator[curFloor][x] == curFloor)
				elevator[curFloor][x] = 9;
		}

	}

	private void take() {
		Scanner sc = new Scanner(System.in);

		while (true) {
			System.out.println("층 수를 선택해 주세요.");
			System.out.printf("1.1층\t2.2층\t3.3층\n");

			int floor = sc.nextInt() - 1;

			if (floor < 0 || floor > 2)
				System.out.println("다시 입력해주세요.");
			else if (floor == curFloor)
				System.out.println("목적지로 현재 층은 안됩니다.");
			else {
				while (true) {
					System.out.println("어디에 탑승하시겠습니까?");
					for (int x = 0; x < 4; x++)
						if (elevator[curFloor][x] == 9)
							System.out.printf("[%d]", x + 1);
						else
							System.out.printf("[  ]");
					System.out.println();

					int position = sc.nextInt() - 1;

					if (elevator[curFloor][position] != 9)
						System.out.println("탑승할 공간이 없습니다. 빈 공간을 선택해 주세요.");
					else {
						elevator[curFloor][position] = floor;
						break;
					}
				}
				break;
			}

		}

	}

}

```