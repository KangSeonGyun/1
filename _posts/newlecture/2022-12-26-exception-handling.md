---
title: Exception Handling
tags: java
---

예외란 오류중에 하나다.   
예외는 자바가 갖고있는 것이 아닌 라이브러리가 예외를 정의하고 만들어 뒀다.

오류 3가지   
 구문오류(치명적이지 않음) / 문법   
 논리오류(치명적임) / 학생 10명의 평균 키를 알기 위해 10명의 키를 다 더한 값에 11을 나눈 경우   
 예외(누군가만, 어떤 컴퓨터만, 어떤 상황에서만 예외적으로 발생하는 오류) / 인터넷이 끊긴 경우, 물리적인 장치문제, 입력 값 문제

Calculator.java
-------------

```java

//	성적관리 프로그램이 파일 입, 출력을 이용한다면 디스크에 직접 접근하는 것이 아닌 Disk Access API를 통해 접근한다
//	Disk Access API엔 권한, 파일존재여부, 파일용량을 확인할 수 있으나 예외가 발생할 수 있다
//	하지만 책임자인 성적관리 프로그램이 예외처리하지 않고 API가 대신 예외처리를 한다면 문제가 있다
//	결국 API는 책임자에게 예외를 보고하고 책임자는 예외를 처리해야 한다

//	예외를 처리한다는 것의 의미
//	1. 예외에 대한 안내 메시지를 사용자에게 안내
//	2. 예외가 치명적인지 아닌지에 따라 계속 프로그램을 이어갈지 말지 판단한다

//	책임자와 API 사이에 Library등이 낄 수 있으므로 보고는 return으로 하는 것이 아닌 새로운 보고체계(채널)을 만들어야 한다

//	보고방법
//	예외에 식별자(1, 2, 3)를 붙이고
//	식별자를 던진다(throw 1;) / 자바에서는 예외 클래스 객체를 던진다(throw new 권한없음예외();)
//	예외 클래스는 기능을 포함한 캡슐은 아니고 예외를 식별하기 위한 특수한 의미의 클래스

//	책임자는 던져진(throw) 예외를 잡아(catch) 예외처리를 한다

public class Calculator {
//	예외를 던질 수 있는 라이브러리

	private int x;
	private int y;

	public Calculator(int x, int y) {
		this.x = x;
		this.y = y;
	}

	public int add() {
		return x + y;
	}

	public static int add(int x, int y) throws 천을_넘는_예외, 음수가_되는_예외 {
//		더한 값이 1000이 넘으면 계산한 값을 반환하지 않는다 / 예외가 필요하다

		int result = x + y;

		if (result > 1000)
			throw new 천을_넘는_예외();
//			식별할 수 있는 예외 클래스를 객체로 만들어 던진거다
//			던질 때는 예외 객체를 던지는 것이므로 예외 클래스는 반드시 예외(Exception)를 상속받고 만들어 져야 한다
//			throw로 던져도 가정 먼저 받는 것은 자기 자신이므로 자신이 처리하지 않겠다면 throws를 이용해 위로 던진다

		if (result < 0)
			throw new 음수가_되는_예외();
//			throws로 여러개를 던질 수 있다

		return result;
	}

	public static int sub(int x, int y) throws 음수가_되는_예외 {

		int result = x - y;

		if (result < 0)
			throw new 음수가_되는_예외();

		return result;
	}

	public static int div(int x, int y) {
		return x / y;
	}

	public static int multi(int x, int y) {

		int result = x * y;

		if (result > 1000)
			throw new 천을_넘는_예외2();

		return result;
	}

}

```

Program.java
-------------

```java

public class Program {

	public static void main(String[] arge) {
//		calc.add(3, 4);
//		인스턴스 메소드 사용 / 하지만 이렇게 만든다면 인스턴스로 만들 이유가 없다
//		인스턴스라고 하는 것은 멤버에 값을 넘겨주고 멤버를 객체화 한 뒤 덧셈만 해달라고 해야 한다
		Calculator calc = new Calculator(3, 4);
		calc.add();

		Calculator.add(3, 4); // 예외를 throws 혹은 catch를 하지 않아 에러난다
//		static 메소드 사용
//		static 메소드란 객체가 갖고 있는 값을 사용하지 않고 넘겨진 것을 가지고 덧셈을 한다
//		계산기를 만들 때는 static 메소드를 사용해 만드는 게 적합하다

//		-----------------------------------------------------------

		int result = 0;

		result = Calculator.add(3, -4); // 예외를 throws 혹은 catch를 하지 않아 에러난다
//		main함수마저 예외를 throws한다면 책임질 사람이 없으므로 default행동이 진행된다
//		main함수가 던지면 자바 런타임환경이 받게되고 자바 런타임환경은 예외가 치명적인 예외인지 아닌지 모른다
//		따라서 예외가 발생한 이후는 전혀 실행되지 않고 프로그램이 끝난다

//		예외를 처리하려면 Calculator.add가 예외를 던질 수 있다는 걸 알고 있어야 한다
//		자바 함수를 검색해 해당 함수가 어떤 것을 throws 하는지 찾아보면 예외가 발생할 수 있다는 걸 알 수 있다

		try {
			result = Calculator.add(3, -4);
//			예외 발생
		} catch (천을_넘는_예외 | 음수가_되는_예외 e) {
//			프로그램이 끝나지 않고 계속 이어진다
//			2개의 예외를 처리하는 방법이 같을 경우 or 연산으로 묶어 단일하게 처리한다
			System.out.println("계산 결과가 1000을 넘었습니다"); // 계산 결과가 1000을 넘었습니다
			System.out.println(e.getMessage()); // 계산 결과가 음수입니다
		}

//		-----------------------------------------------------------

		try {
//			예외가 발생할 수 있는 것들은 한 곳에 몰아놔야 한다 / 하지만 catch하는 여러 방법들을 기록하기 위해 분리
			result = Calculator.add(3, 10004);
			result = Calculator.add(3, -4);
		} catch (천을_넘는_예외 e) {
//			각각 예외를 특화시켜서 처리하는 방법
			System.out.println(e.getMessage()); // 계산 결과가 1000을 넘었습니다
		} catch (음수가_되는_예외 e) {
			System.out.println(e.getMessage());
			// 실행 되지 않는다 / 천을_넘는_예외를 처리했기 때문에 다른 예외가 발생해도 해당 코드까지 오지 않는다
		}

//		-----------------------------------------------------------

		try {
			result = Calculator.add(3, 4);
			result = Calculator.sub(3, 5);
		} catch (천을_넘는_예외 e) {
			System.out.println(e.getMessage());
//			실행 되지 않는다
		} catch (Exception e) {
			System.out.println("계산 결과가 범위 밖 입니다"); // 계산 결과가 범위 밖 입니다
//			모든 예외를 갖고 있는 부모(Exception) / switch문의 default와 같다
//			모든 예외를 다 받을 수 있으므로 다른 catch절보다 위에 있으면 안 된다
		} finally {
//			예외가 발생하거나 발생하지 않아도 마지막에 꼭 들리는 곳
			System.out.println("다시 입력해 주세요"); // 계산 결과가 범위 밖 입니다
		}

//		-----------------------------------------------------------

//		Checked 예외
//		위와 같이 꼭 잡아야 하는 예외(천을_넘는_예외, 음수가_되는_예외)

//		UnChecked 예외
//		던지거나 잡지 않아도 되는 예외(천을_넘는_예외2, 음수가_되는_예외2), 잡을 수도 있다
//		UnChecked 예외 클래스 구현 시 RuntimeException을 상속받으면 된다

//		UnChecked 예외의 예시
		result = Calculator.div(3, 0); // java.lang.ArithmeticException
//		분모를 0으로 나누면 무한대가 된다 / 하지만 이런 예외를 Calculator.div()에서 처리한 적이 없다
//		모든 것들을 Checked로 하면 코드가 복잡해 진다 / 사용자가 알아서 피할 수 있는 입력값들에 대한 처리는 UnChecked예외로 한다

		result = Calculator.multi(3, 400); // 천을_넘는_예외2

	}

}

```

천을_넘는_예외.java
-------------

```java

public class 천을_넘는_예외 extends Exception {
	@Override
	public String getMessage() {
		return "계산 결과가 1000을 넘었습니다";
	}
}

```

음수가_되는_예외.java
-------------

```java

public class 음수가_되는_예외 extends Exception {
	@Override
	public String getMessage() {
		return "계산 결과가 음수입니다";
	}
}

```

천을_넘는_예외2.java
-------------

```java

public class 천을_넘는_예외2 extends RuntimeException {

}

```

## 자바 8.0 부터 추가된 try-with-resources

try-with-resources가 없을 때의 코드

```java
public class Program {
    public static void main(String[] args) {

        FileOutputStream fos = null;
        PrintStream out = null;
        try {
            fos = new FileOutputStream("res/data.txt");
            out = new PrintStream(fos);

            out.println("Hello");

            out.close();
            fos.close();
        } catch (FileNotFoundException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } finally {
            try {
                out.close();
                fos.close();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }

    }

}
```

try-with-resources

close가 추가된 try문이다.
예외가 발생하지 않아도 close한다.

```java
public class Program2 {
    public static void main(String[] args) {

        try (
                FileOutputStream fos = new FileOutputStream("res/data.txt");
                PrintStream out = new PrintStream(fos);
        ) {

            out.println("Hello");
            
        } catch (FileNotFoundException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }

    }

}
```