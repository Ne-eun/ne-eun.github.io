---
layout: default
title: this
parent: Javascript
nav_order: 1
---

# This
다른 언어에서 this는 생성된 오브젝트 자기 자신을 가르킨다.   
자바스크립트에서의 this는 호출한 문맥에 따라 this가 변경된다.   
function으로 선언한 함수는 window객체에 등록이 되고 window에서 접근이 가능하다. (window는 생략 가능)   
const나 let으로 선언한 변수는 windwo객체에 등록되지 않는다.   
var로 선언햔 변수는 window객체에 등록된다.   

```js
console.log(this); //this는 window
function simpleFunc() {
	function simpleFunc2() {
		console.log(this); 
	}
	simpleFunc2()
}
window.simpleFunc(); // window에서 호출했기 떄문에 this는 window를 가르킨다. (window 생략 가능)
```

```js
class Counter {
  count = 0;
  increase() {
    console.log(this);
  };
}

const counter = new Counter();
counter.increase(); // counter에서 호출 했기 때문 increase안에 있는 this는 counter instance를 가르킨다. 
const caller = counter.increase
counter.increase() === caller() // true

class Bob {}
const bob = new Bob();
bob.run = counter.increase; 
bob.run(); // increase안의 this는 bob을 가르킨다.
// 호출의 주체가 bob안에 있는 run이기 때문이다.
```

