---
layout: default
title: this
parent: Javascript
nav_order: 1
---

# this 키워드

자바스크립트에서 `this`는 함수가 **호출되는 방식**에 따라 가리키는 대상이 동적으로 결정됩니다.

## 1. 전역 문맥 (Global Context)
전역 실행 컨텍스트에서 `this`는 전역 객체를 가리킵니다.
- 브라우저 환경: `window`
- Node.js 환경: `global`

```javascript
console.log(this); // window (브라우저 기준)

function simpleFunc() {
  console.log(this); // window
}
simpleFunc(); // window.simpleFunc()와 동일
```

---

## 2. 메서드 호출 (Method Call)
함수가 객체의 메서드로 호출될 때, `this`는 해당 메서드를 호출한 **객체**를 가리킵니다.

```javascript
class Counter {
  count = 0;
  increase() {
    console.log(this);
  };
}

const counter = new Counter();
counter.increase(); // this는 counter 인스턴스를 가리킴
```

---

## 3. 호출 주체에 따른 변화
동일한 함수라도 누구에 의해 호출되느냐에 따라 `this`가 달라집니다.

```javascript
class Bob {}
const bob = new Bob();

// counter의 메서드를 bob의 속성에 할당
bob.run = counter.increase; 

bob.run(); // this는 이제 bob을 가리킴
// 호출 주체(Caller)가 bob이기 때문입니다.
```

## 4. 선언 방식에 따른 차이 (`var` vs `let/const`)
- **`var`**로 선언한 전역 변수는 전역 객체(`window`)의 속성이 됩니다.
- **`let`, `const`**로 선언한 전역 변수는 전역 객체의 속성이 되지 않습니다.

```javascript
var myVar = "I am window's";
let myLet = "I am hidden";

console.log(window.myVar); // "I am window's"
console.log(window.myLet); // undefined
```
