---
title: SubwayController
tags: java
---

SubwayController
-------------

```java

package com.test.service;

public class SubwayController {

	public static void main(String[] args) {
		// 지하철 만들기
		// 역은 총 5개로 순환열차이다.(합정, 홍대입구, 신촌, 이대, 아현)
		// 차량은 총 4량, 각 차량당 탑승가능인원 4명
		// 탑승 시 목적지 지정. 목적지도착 시 자동하차
		// 이동시 1개역씩 이동

		SubwayService ss = new SubwayService();

		System.out.println("---- Welcome Subway ----");
		ss.run();
	}

}

```

SubwayService
-------------

```java

package com.test.service;

import java.util.Scanner;

public class SubwayService {
	private Seat[] car;

	public SubwayService() {
		car = new Seat[4];

		for (int i = 0; i < 4; i++)
			car[i] = new Seat();
	}

	public void run() {
		Boolean loop = true;
		String[] station = { "합정", "홍대입구", "신촌", "이대", "아현" };
		int curStation = 0;

		while (loop) {
			Scanner scan = new Scanner(System.in);

			System.out.println("=================================");
			System.out.printf("현재역은 %s입니다.\n", station[curStation]);
			System.out.println("=================================");
			System.out.println("메뉴를 선택하세요.");
			System.out.println("1.탑승 2.상세보기 3.이동 9.종료");
			switch (scan.nextLine()) {
			case "1":
				join(station, curStation);
				break;
			case "2":
				status();
				break;
			case "3":
				if (curStation < 4) {
					curStation++;
					move(station, curStation);
					break;
				}
			case "9":
				System.out.println("열차운행을 종료합니다.");
				loop = false;
				break;
			default:
				System.out.println("잘못입력하였습니다.");
			}
		}
	}

	private void join(String[] station, int curStation) {
		Scanner scan = new Scanner(System.in);

		System.out.println("---- 탑승가능 현황 ----");

		Boolean[] empty = new Boolean[4];

		for (int i = 0; i < 4; i++) {
			System.out.printf("%d호차 : ", i + 1);
			empty[i] = car[i].getSeat1() == "" || car[i].getSeat2() == "" || car[i].getSeat3() == ""
					|| car[i].getSeat4() == "";
			if (empty[i]) {
				System.out.println("가능");

			} else
				System.out.println("불가능");
		}

		int num, dest;

		System.out.println("어느 열차에 탑승하시겠습니까?");
		System.out.println("1. 1호차 2. 2호차 3. 3호차 4. 4호차");

		num = scan.nextInt() - 1;

		if (empty[num]) {
			System.out.println("목적지를 선택해 주세요.");
			for (int i = curStation + 1; i < 5; i++)
				System.out.printf("%d.%s ", i, station[i]);

			System.out.println();

			dest = scan.nextInt();

			if (this.car[num].getSeat1() == "")
				car[num].setSeat1(station[dest]);
			else if (car[num].getSeat2() == "")
				car[num].setSeat2(station[dest]);
			else if (car[num].getSeat3() == "")
				car[num].setSeat3(station[dest]);
			else if (car[num].getSeat4() == "")
				car[num].setSeat4(station[dest]);
		} else
			System.out.println("열차 안이 꽉 찼습니다.");
	}

	private void status() {
		System.out.println("---- 열차 현황 ----");
		for (int i = 0; i < 4; i++) {
			System.out.printf("%d호차 : ", i + 1);
			System.out.print(car[i].getSeat1());
			System.out.print(car[i].getSeat2());
			System.out.print(car[i].getSeat3());
			System.out.println(car[i].getSeat4());
		}

	}

	private void move(String[] station, int curStation) {
		for (int i = 0; i < 4; i++) {
			if (car[i].getSeat1() == station[curStation])
				car[i].setSeat1("");

			if (car[i].getSeat2() == station[curStation])
				car[i].setSeat2("");

			if (car[i].getSeat3() == station[curStation])
				car[i].setSeat3("");

			if (car[i].getSeat3() == station[curStation])
				car[i].setSeat4("");
		}

	}
}

```

Seat
-------------

```java

package com.test.service;

public class Seat {
	private String seat1;
	private String seat2;
	private String seat3;
	private String seat4;

	public Seat() {
		this("", "", "", "");

	}

	public Seat(String seat1, String seat2, String seat3, String seat4) {
		this.seat1 = seat1;
		this.seat2 = seat2;
		this.seat3 = seat3;
		this.seat4 = seat4;
	}

	public String getSeat1() {
		return seat1;
	}

	public void setSeat1(String seat1) {
		this.seat1 = seat1;
	}

	public String getSeat2() {
		return seat2;
	}

	public void setSeat2(String seat2) {
		this.seat2 = seat2;
	}

	public String getSeat3() {
		return seat3;
	}

	public void setSeat3(String seat3) {
		this.seat3 = seat3;
	}

	public String getSeat4() {
		return seat4;
	}

	public void setSeat4(String seat4) {
		this.seat4 = seat4;
	}

}

```