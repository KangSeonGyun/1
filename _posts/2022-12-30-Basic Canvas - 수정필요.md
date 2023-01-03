---
title: Basic Canvas
tags: html javascript
---

Html
-------------

```html

<!DOCTYPE html>

<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

    <script src="app.js">
        // 본문이 다 읽힌 다음에 script를 로드한다
        // 본문 보다 밑에 script코드를 작성한다

    </script>
</head>

<body>

    <canvas class="game-canvas" width="500" height="700">
        <!-- 캔버스 크기 조정 / css로 크기 조정하는 것은 확대 축소다 -->
        <!-- 캔버스는 멀티미디어 지원 -->
    </canvas>
    
</body>

</html>

```

Js
-------------

```js

// 브라우저 객체 12 - 30
// 브라우저의 가장자리 틀을 이용하는 객체 window / 브라우저 최상위 / 전역객체
// window.open();
// window를 열거나 닫거나 사이즈 변경은 잘 안쓴다 / 모바일 때문

// window.alert("aa");
// window.location.href = "https://www.newlecture.com";

window.addEventListener("load", function () {
    var canvas = this.document.querySelector(".game-canvas");
    // .의 의미 클래스명 / 태그명으로 찾거나 / #아이디로 찾는다
    // this 생략가능
    
    /** @type {CanvasRenderingContext2D} */

    var ctx = canvas.getContext("2d");
    
    ctx.fillStyle = "rgb(200,0,0)";
    ctx.fillRect(10, 10, 50, 50);
    ctx.strokeStyle = "green";
    ctx.strokeRect(20, 10, 160, 100);

    var img1 = this.document.querySelector("img");
    ctx.drawImage(img1, 100, 100)
    
})

```
