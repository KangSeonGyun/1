---
title: Js Singleton
tags: javascript
--- 

```js

// 싱글톤(singleton) 패턴 이해를 위한 Js 복습

var ob = {
    publicMethod: function () {
        return 'hello Singleton Pattern!!!';
    },
    publicProp: 'single value'
};
// Object 객체인 ob생성 및 속성 추가

console.log(ob.publicMethod); // ƒ () { return 'hello Singleton Pattern!!!'; }
console.log(ob.publicMethod()); // hello Singleton Pattern!!!
console.log(ob.publicProp); // single value
// Object 객체인 ob의 속성을 출력했다

var example = ob;
// Object 객체인 ob의 속성들을 example에도 추가했다

console.log(example.publicMethod); // ƒ () { return 'hello Singleton Pattern!!!'; }
console.log(example.publicMethod()); // hello Singleton Pattern!!!
console.log(example.publicProp); // single value

// -------------------------------------------------------------------------------------

var o;

if (o)
    console.log(o); // 아무것도 출력되지 않는다
//  객체가 생성되었다면 true / 객체가 생성되지 않았다면 false를 반환한다

var o = {
    publicMethod: function () {
        return 'hello Singleton Pattern!!!';
    },
    publicProp: 'single value'
};

if (o)
    console.log(o); // {publicProp: 'single value', publicMethod: ƒ}

// -------------------------------------------------------------------------------------

// 클로저를 이용한 싱글톤(singleton) 패턴을 구현하는 예제

var Singleton = (function () {

    // 비공개 변수 instantiaed, 메서드 init 정의
    var instantiated;
    // 이미 객체가 생성됐는지 여부를 알려주는 내부 변수 / 싱글톤 객체가 할당되는 변수

    function init() {

        // 싱글톤 객체 정의
        return {
            // 공개 메서드 정의
            publicMethod: function () {
                return 'hello Singleton Pattern!!!';
            },
            // 공개 프로퍼티 정의
            publicProp: 'single value'
        }

    }

    // 공개 메서드인 getInstance() 를 정의한 객체.
    // 렉시컬 특성으로 인해 비공개 변수 instantiaed, 메서드 init에 접근 가능(클로저)
    return {
        getInstance: function () {
            if (!instantiated) {
                instantiated = init();
            }
            // 객체가 생성되지 않았을 때 딱 한 번 init()을 통해 초기화 해준다
            return instantiated;
        }
    }

})();
// })();
// }) 우선연산자
// (); function을 실행하여 Singleton에 return값 {getInstance: ƒ}를 대입한다 

var first = Singleton.getInstance();
// Singleton변수는 함수의 리턴된 값인 속성명(getInstance) 값(function)을 갖고있는 object가 된다
// 첫 싱글톤 객체 생성시 딱 한 번 초기화(init)된다
// 싱글톤 객체(publicMethod(), publicProp)가 할당된 instantiaed변수를 first변수에 대입한다

console.log(first.publicMethod()); // hello Singleton Pattern!!!

var second = Singleton.getInstance();
console.log(second.publicMethod()); // hello Singleton Pattern!!!

console.log(first === second); // true
// 변수의 렉시컬한 특성으로 인해 내부의 getInstance 함수에서 비공개 변수인 instantiaed 에 접근할 수 있다는 것과
// getInstane() 호출이 끝나더라도 instantiaed 값은 계속 유지되는 특성(클로저)을 이용해
// publicMethod(), publicProp 이 포함된 객체(instantiaed)를 유일하게 생성

// -------------------------------------------------------------------------------------

// 클래스를 이용한 싱글톤(singleton) 패턴을 구현하는 예제

class Singleton2 {
    static #instance;

    constructor() {
        if (Singleton2.#instance) return Singleton2.#instance;
        // 싱글톤 객체가 생성되었는지 아닌지 확인

        this.publicMethod = function () {
            return 'hello Singleton Pattern!!!';
        };
        this.publicProp = 'single value';
        Singleton2.#instance = this;
        // 싱글톤 객체가 생성되지 않았다면 싱글톤 객체 정의하고 #instance변수에 싱글톤 객체를 할당한다
    }
}

var a = new Singleton2();
var b = new Singleton2();

console.log(a === b); // true

console.log(a); // Singleton2 {publicProp: 'single value', publicMethod: ƒ}

```