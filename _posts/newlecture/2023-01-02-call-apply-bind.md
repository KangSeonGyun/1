---
title: Js call apply bind
tags: javascript
---

```js

function Box() {

}

Box.prototype = {
    test: function () {
        console.log(this);
    },
    test2: function (x, y) {
        console.log(this);
        console.log(x);
        console.log(y);
    }
}

var box = new Box();
box.test(); // Box {}

var f1 = box.test;
// ()가 빠졋다 / 실행한 리턴값이 아닌 함수 자제를 넣어줌
f1(); // Window {}
// f1은 인스턴스 매서드가 된다 / f1은 window.f1이다 /호출자가 바꼈다

var obj = { kor: 2 }
// kor: 2가 없다면 Uncaught TypeError: Cannot set properties of undefined (setting 'onload')
obj.onload = box.test;
console.log(obj.onload); // ƒ () { console.log(this); }
obj.onload(); // {kor: 2, onload: ƒ}

var n1 = { id: 1, title: 'hello' }
obj.onload.call(n1); // {id: 1, title: 'hello'}
// call(), apply() 호출할 때 호출하는 주체에 상관없이 this를 바꿈
// call()과 apply()는 거의 동일하지만, call()은 인수 목록을, 반면에 apply()는 인수 배열 하나를 받는다

obj.onload = box.test2;
obj.onload.apply(n1, ['hi', 'okay']); // {id: 1, title: 'hello'} hi okay

console.log(this) // Window {}
// window.f1()으로 실행되면 Window {} 
console.log(f1.bind(this)) // ƒ () { console.log(this); }
// 함수 실행 시 .bind(this)로 인해 box.test가 실행된다

// bind() 메소드가 호출되면 새로운 함수를 생성합니다.
// 받게되는 첫 인자의 value로는 this 키워드를 설정하고, 이어지는 인자들은 바인드된 함수의 인수에 제공됩니다.

```