---
layout: default
title: 변수
parent: Javascript
nav_order: 1
---

# Variable (변수)
javascript의 변수는 기본적으로 호이스팅(hoisting)이 된다.  
var는 호이스팅을 허용하는 반면 const/let은 TDZ때문에 허용되지않는다.

- 선언 (Declaration): 스코프와 변수 객체가 생성되고 스코프가 변수 객체를 참조한다.
- 초기화(Initalization): 변수 객체가 가질 값을 위해 메모리에 공간을 할당한다. 이때 초기화 되는 값은 undefined이다.
- 할당 (Assignment): 해당 변수가 가리키는 주소의 공간에 데이터를 저장한다.

---

## Hoisting (호이스팅)
변수의 선언과 초기화를 분리한 후, 선언만 코드의 최상단으로 옮기는 것으로  
변수를 정의하는 코드보다 사용하는 코드가 앞서 등잘 할 수 있다.  
인터프리터가 코드를 해석할 때 변수 및 함수의 선언처리와 실제 코드 실행을 두단계로 나눠서 처리 하기때문에 발생한다.  
var로 선언한 변수의 경우 호이스팅 시 (undefined)로 변수를 초기화 한다.  
반면 let과 const로 선언한 변수의 경우 호이스팅 시 변수를 초기화 하지 않는다.
```js
/* --- 호이스팅 --- */
  console.log("hoisting", myName); // undefined
  var myName = "ne-eun";

  // console.log(name2); // error: Uncaught ReferenceError: Cannot access 'name' before initialization
  let name2 = "Evan";

  /* --- var 스코프 --- */
  function test() {
    var a = "123";
  }
  var b = "ass";
  // console.log("scope", a); // error: a is not defined
  if (true) {
    var b = "123";
    console.log("scope", b); // 123
  }
  console.log("scope", b); //123

  /* --- 블록 호이스팅 --- */
  let name = "ne-eun park";
  if (name === "ne-eun park") {
    // console.log(name); // error: Uncaught ReferenceError: Cannot access 'name' before initialization
    let name = "terabyte";
    console.log(name); // terabyte
  }
```

## Temporal Dead Zone (임시로 죽어있는 공간)
var의 경우(v8엔진)kVar모드로 변수 객체를 생성한 후 바로 AllocateTo메소드를 통해 메모리에 공간을 할당한다.  
그러나 const와 let의 경우 kLet,kConst모드로 생성한 변수 객체들은 AllocateTo메소드가 바로 호출되지않고,  
소스 코드상에서 해당코드의 위치를 의미하는 position값만 정해준다.  
이 타이밍에 lef이나 const로 생성된 변수들이 TDZ(Temporal Dead Zone)구간에 들어간다.   
(선언은 되었지만 초기화가 되지않아 공간이 메모리에 할당되지 않음)

---

## 1. var
함수 레벨 스코프를 사용  
var 키워드 생략 가능  
호이스팅 현상을 사용할 수 있음
변수의 중복 선언 가능

## 2. const / let
const는 상수를 선언할 때 let은 변수를 선언할 때 사용되는 키워드이다.  
키워드의 생략이 불가능 하다.
변수의 중복 선언이 불가능 하다.  
블록 레벨 스코프를 사용  
블록 내부에서 호이스팅 현상이 발생한다.
