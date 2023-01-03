---
title: Basic Canvas2
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
    </script>
    
</head>

<body>

    <canvas class="game-canvas" width="500" height="700">
    </canvas>
    
</body>

</html>

```

Js
-------------

```js	

window.addEventListener("load", function () {
    var canvas = this.document.querySelector(".game-canvas");
    // .의 의미 클래스명 / 태그명으로 찾거나 / #아이디로 찾는다
    // this 생략가능

    window.onclick = function () {
        alert("window clicked"); // alert("window clicked")
    }

    canvas.onclick = function () {
        alert("canvas clicked"); // alert("canvas clicked") > alert("window clicked") / 버블링
        boy2.move(2);
        boy2.draw(ctx);
    }

    /** @type {CanvasRenderingContext2D} */
    var ctx = canvas.getContext("2d");

    // 01-02

    function Boy(x, y) {
        this.ix = 0;
        this.iy = 2;
        this.x = x || 100;
        this.y = y || 100;

        this.sw = 106;
        this.sh = 148.25;
        this.sx = this.sw * this.ix;
        this.sy = this.sh * this.iy;

    }

    Boy.prototype = {
        draw: function (ctx) {
            var img = new Image();
            img.src = "./images/boy.png";

            img.onload = function () {
                console.log(this.sw); // undefined
                console.log(this); // <img src="./images/boy.png">
                // img가 호출함으로써 this가 바뀜 / bind가 있다면 106과 Boy객체가 나온다

                ctx.drawImage(img, this.sx, this.sy, this.sw, this.sh,
                    this.x, this.y, this.sw, this.sh);
            }.bind(this);
        },
        move: function (dir) {
            switch (dir) {
                case 1: // 북
                    this.y -= 10;
                    break;
                case 2: // 동
                    this.x += 10;
                    break;
                case 3: // 남
                    this.y += 10;
                    break;
                case 4: // 서
                    this.x -= 10;
                    break;
            }
        }
    }

    var boy1 = new Boy(); // Boy선언 전에 객체생성하면 에러
    boy1.draw(ctx);
    var boy2 = new Boy();
    boy2.move(2);
    boy2.draw(ctx);
    // 브라우저 랜더링은 draw를 모았다가 한번에 처리함


    // -------------------------------------------------------------------------------------------------------

    var img = new Image();

    img.src = "./images/boy.png";

    img.onload = function () {
        // ctx.drawImage(img, 100, 100); 100 100 위치에서 다 보여주기
        // ctx.drawImage(img, 100, 100, 106, 148.25); 100 100 위치에서 이미지 전체를 106 148.25크기 줄이기
        // ctx.drawImage(img, 106, 296.5, 106, 148.25, 100, 100, 106, 148.25);
        // 106 296.5위치에서 106 148.25크기만큼 자르고 100, 100위치에 106 148.25크기로 출력

        var ix = 0;
        var iy = 3;

        var sw = 106;
        var sh = 148.25;
        var sx = sw * ix;
        var sy = sh * iy;

        ctx.drawImage(img, sx, sy, sw, sh,
            200, 100, sw, sh);
    }

})

// -------------------------------------------------------------------------------------------------------

function Box() {

}

Box.prototype = {
    test: function (x, y) {
        console.log(this);
        console.log(x);
        console.log(y);
    }
}

var box1 = new Box();
box1.test(); // Box {}

var f1 = box1.test; // ()가 빠졋다
f1(); // Window {}
// f1은 window.f1이다

var obj = { kor: 2 }
obj.onload = box1.test;
console.log(obj.onload); // ƒ () { console.log(this); }
obj.onload(); // {kor: 2, onload: ƒ}

var n1 = { id: 1, title: 'hello' }
obj.onload.call(n1); // {id: 1, title: 'hello'}
// call(), apply() 호출할 때 호출하는 주체에 상관없이 this를 바꿈

obj.onload.apply(n1, ['hi', 'okay']); // {id: 1, title: 'hello'} hi okay
// test function(x, y) 추가 및 console.log(x); console.log(y); 추가

// .bind(this); 함수를 호출한 뒤에 this를 바꿔줌?

```
