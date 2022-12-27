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
	//	값 변수 형식이없다 / wrapper 클래스가 기본형
	//	컴파일러에서 오류를 찾을 수 없다 / 실행환경에 의한 오류 확인
	//	var x = 3; > 객체화한다 > var x = new Number(3);
	//	내려쓰기 or ; 으로 코드의 끝을 표시한다

	var x // Type - undefined
	alert(x == undefined) // undefined가 하나의 타입이다
	x = 3; // Type - number

	var nums = new Array();
	nums[0] = 5;
	//	자바는 미리 공간을 정하지만 js는 필요에 따라 늘린다 correction(콜렉션)능력

	var nums = new Array();
	nums[3] = 5;
	console.log(nums);
	console.log(nums.length);
	//	배열 3번칸에 5가 들어간 4칸짜리 배열이 만들어진다 0,1,2칸은 비어있음
	//	배열 중간 값이 비어있는 건 좋지 않다

	var nums = new Array(5);
	var nums = new Array(5, 10, 21, "hello");
	console.log(typeof nums[3]);
	//	5칸 짜리 배열이 만들어짐 / 여러 타입을 한 배열에 넣을 수 있음 / typeof로 확인

	var nums = new Array(1, 2);
	nums.push(4); // Stack(FILO)
	console.log(nums);
	nums.pop(); // Stack(FILO)
	console.log(nums);

	var nums = new Array(1, 2);
	nums.push(5); // Buffer(FIFO)
	console.log(nums);
	nums.shift(); // Buffer(FIFO)
	console.log(nums);
	nums.unshift(7); // 배열 맨 앞[0]에 7이 들어옴 / 나머진 뒤로 한 칸씩 밀린다
	console.log(nums);
	
	var nums = new Array(1, 2,3,4,5);
	nums.splice(2);
	console.log(nums);
	
</script>

</head>

<body>

</body>

</html>

```
