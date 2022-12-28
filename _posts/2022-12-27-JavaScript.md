---
title: JavaScript
tags: javaScript
---

ES5를 배우고 ES6
ES6은 응용 

파이썬과 같이 JavaScript는 쉬운언어다
Java - int x = 3;
JS   - var x = 3;

Js 실습을 위한 Html
-------------

```html

<!DOCTYPE html>

<html>

<head>

<meta charset="UTF-8">
<title>Insert title here</title>

<script>
	//	컴파일러에서 오류를 찾을 수 없다 / 실행환경에 의한 오류 확인
	//	내려쓰기 or ; 으로 코드의 끝을 표시한다
	//	Wrapper 클래스가 기본형 int a;를 하면 int A = new int();로 만듦 / Wrapper객체 / primitive type의 값을 담는 객체를 생성하는 것이다.
	//	primitive 값은 Wrapper의 메소드를 통해 가져온다 / 결국 js는 값 변수 형식이없으며 모든걸 객체화한다
	//	Boxing - primitive type을 객체로 다른 메서드를 사용할 수 있습니다. String 기본형 변수를 자동으로 String 객체로 만듦
	
	var x // Type - undefined
	alert(x == undefined) // true / undefined가 하나의 타입이다
	x = 3; // Type - number

	var nums = new Array();
	nums[0] = 5;
	//	자바는 미리 공간을 정하지만 js는 필요에 따라 늘린다 correction(콜렉션)능력

	var nums = new Array();
	nums[3] = 5;
	console.log(nums); // [빈 ×3, 5]
	console.log(nums.length); // 4
	//	배열 3번칸에 5가 들어간 4칸짜리 배열이 만들어진다 0,1,2칸은 비어있음
	//	배열 중간 값이 비어있는 건 좋지 않다

	var nums = new Array(5);
	var nums = new Array(5, 10, 21, "hello");
	console.log(typeof nums[3]); // string
	//	5칸 짜리 배열이 만들어짐 / 여러 타입을 한 배열에 넣을 수 있음 / typeof로 확인

	var nums = new Array(1, 2);
	nums.push(4); // Stack(FILO)
	console.log(nums); // [1, 2, 4]
	nums.pop(); // Stack(FILO)
	console.log(nums); // [1, 2]

	var nums = new Array(1, 2);
	nums.push(5); // Buffer(FIFO)
	console.log(nums); // [1, 2, 5]
	nums.shift(); // Buffer(FIFO)
	console.log(nums); // [2, 5]
	nums.unshift(7); // 배열 맨 앞[0]에 7이 들어옴 / 나머진 뒤로 한 칸씩 밀린다
	console.log(nums); // [7, 2, 5]

	var nums = new Array(1, 2, 3, 4, 5);
	nums.splice(2); // 이어붙이기 위한 첫번째 장소 / 첫 번째 인자에 2를 넣으면 두 번째 칸까지 잘리기만 함
	console.log(nums); // [1, 2]
	var nums = new Array(1, 2, 3, 4, 5);
	nums.splice(2, 1); // 두 번째 칸에서 하나를 지운 것
	console.log(nums); // [1, 2, 4, 5]
	var nums = new Array(1, 2, 3, 4, 5);
	nums.splice(1, 0, 23); // 첫 번째 칸에서 지우지 않고 23을 삽입
	console.log(nums); // [1, 23, 2, 3, 4, 5]
	var nums = new Array(1, 2, 3, 4, 5);
	nums.splice(1, 0, 23, 24, 25); // 첫 번째 칸에서 지우지 않고 23, 24, 25를 삽입
	console.log(nums); // [1, 23, 24, 25, 2, 3, 4, 5]
	var nums = new Array(1, 2, 3, 4, 5);
	nums.splice(1, 2, 23); // 첫 번째 칸에서 두 개 지우고 23을 삽입
	console.log(nums); // [1, 23, 4, 5]

	//	Expand Object
	var exam = new Object();
	exam.kor = 30;
	exam.eng = 70;
	exam.math = 80;
	alert(exam.kor + exam.eng); // 100
	exam.Kor = 20;
	alert(exam.kor + exam.eng); // 100 / 대소문자 주의

	alert(exam["kor"]); // 30 / 차이?

	console.log(exam); // {kor: 30, eng: 70, math: 80, Kor: 20}
	delete exam.Kor; // 속성은 마음대로 생성 제거가 가능하다 / 객체는 garbage collector
	console.log(exam); // {kor: 30, eng: 70, math: 80}

	// 데이터 표기법 XLS CSV(컴마로 구분) JSON / javascript의 데이터 표기법 JSON
	// 원시 데이터
	// Boolean / var n = true;
	// Number / var n = 1;
	// String / var n = "s"; or var n = 's';
	
	// Array / var n = [];
	// Object / var n = {}; / var n = {"id":1 "title":"hi json"}

	var exam = "[3,5,4,2]";
	alert(exam[2]); // , / 인덱스 값 2를 찾는다 / 3과 5사이의 ,가 출력된다

	eval("var x = 3+5"); // eval은 문자열로 데이터가 왔을 때 객체화에 효율적이다 / caller의 권한으로 수행하는 위험한 함수
	alert(x); // 8

	var data = eval("([3,5,4,2])"); // 함수 정의 문자열로서의 eval 은 앞뒤를 "("와 ")"로 감싸야 한다
	alert(data[2]); // 4

	var data = JSON.parse("[3,5,4,2]"); // JSON 데이터를 객체화
	alert(data[2]); // 4
	console.log(typeof data) // Object

	JSON.stringify() // JSON 데이터 만들기

	var str1 = "hello";
	var str2 = "hello"; // string형
	var str3 = new String("hello"); // object형

	console.log(str1 == str2); // true
	console.log(str1 == str3); // true / java는 객체를 비교하기 때문에 false가 나온다
	console.log(str1 === str3); // false / javascript에서 객체를 비교하는 방법


	// 논리값 Truthy / Falsy / Nullish
	// if (0이 아니면 전부 true)
	var a = 'Cat' || 'Dog'; // OR연산 첫 번째 True 값이 변수에 들어간다
	console.log(a); // Cat
	var a = 'Cat' && 'Dog'; // AND연산 첫 번째 False 값이 변수에 들어간다 / 없으면 맨 마지막 값
	console.log(a); // Dog
	var a = null ?? 'default string'; // Null이 아닌 값을 찾는다
	console.log(a); // default string
	var a = 0 ?? 'default string';
	console.log(a); // 0
	
	var x = 3;
	var y = '3';
	// 컴파일러가 형변환 해줌
	console.log(x + y); // 33 / 덧셈은 숫자가 문자로 바뀜
	console.log(x * y); // 9 / 곱셈은 문자가 숫자가 바뀜
	
	var x = 3;
	var y = 'a';
	console.log(x + y); // 3a
	console.log(x * y); // NaN / 문자 a는 숫자가 아니라 바꿀 수 없다
	
	var z = 3 * 'a'; // NaN은 ==, ===로 비교할 수 없다
	
	if (z == NaN)
		console.log("오류"); // 실행 안됨
	
	if (isNaN(z))
		console.log("오류"); // 오류
	
	// for-in, for-of / for-of는 ES6에서 등장 / 이부분 정리 필요
	var ar = ["철수", "유리", "맹구", "짱구"];
	
	for(i in ar)
		console.log(ar[i]); // 인덱스가 필요하면 for-in 값이 필요하면 for-of ???
	
	for(var k in ar)
		console.log(ar[k]);
	
</script>

</head>

<body>

</body>

</html>

```
