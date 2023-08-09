---
layout: default
title: 객체
parent: Javascript
nav_order: 1
---

# Object (객체)
{: .no_toc}
객체는 관련된 데이터와 함수로 이어 지는데 객체 안에 있을 때는 프로퍼티(속성)와 메소드라고 부른다.  
객체를 생성하는 것은 변수를 정의하고 초기화 하는 것으로 시작된다.  
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>



## 객체 정의
```js
function Person(name, alias, age, gender) {
   (this.name = {
      name,
      alias,
   }),
   (this.age = age);
   this.gender = gender;
}

const personA = new Person('김아무개', '김씨', 26, 'M')
const personA = new Person('박아무개', '박씨', 28, 'F')
```
person이라는 프로토타입 객체를 간단히 정의하고 객체를 생성했다.   

---


## 객체 데이터에 접근하는 방법
한 항목을 선택하기 위해 인덱스 숫자를 이용하는 대신에 각 멤버의 값과 연결된 key를 사용한다.
1. 점 표기법
   ```js
      personA.name
   ```

2. 괄호 표기법
   ```js
      personB['name']
   ```

---

## Prototype  

모든 객체는 상위 프로토타입 객체로부터 메소드와 속성을 상속 받을 수 있다.  
이를 프로토타입 체인이라 부르며 다른 객체에 정의된 메소드와 속성을 한 객체에서 사용할 수 있도록 한다.  
정확히 말하자면 상속되는 속성과 메소드들은 각 객체가 아니라 객체의 생성자의 prototype에 정의 되어 있다.  
프로토타입 체인에서 한객체의 메소드와 속성들이 복사되는 것이 아니며 체인을 타고 올라가며 접근하는 것이다.

---

### 객체 상속
두 방법모두 class에서는 사용하지 않는 방법

```js
function Developer(name, alias, age, gender, position) {
   Person.call(this, name, alias, age, gender);
   this.position = position;
}
```

```js
Developer.prototype = Object.create(Person.prototype);
console.log(Developer.prototype.constructor); // person의 값을 가진다.

Developer.prototype.constructor = Developer; // person의 생성자가 아닌 developer constructor 할당
console.log(Developer.prototype); // Developer cunstructer + person

const personC = new Developer('최아무개', '최씨', 31, 'M', 'front-end');

```
### 상속의 유형

1. 생성자 함수 내에서 인스턴스에 정의하는 유형
   - this.x = x 식으로 정의되어 있으므로 발견하기 쉽다.
   - 브라우저 내장 코드에서는 new 키워드를 통해 생성하고 접근할 수 있는 멤버이다.
   - const tera = new Person()으로 생성 tera.name으로 접근
2. 생성자에서 직접 정의하는 유형, 생성자에서만 사용 가능
   - 브라우저 내장 객체에서 흔히 사용하는 방식인데, 인스턴스가 아니라 생성자 함수에서 바로 호출되는 유형이다.
   - Object.key()와 같은 함수
3. 인스턴스와 자식 클래스에 상속하기 위해 생성자의 프로토타입에 정의하는 유형
   - 생성자의 프로토타입 속성에 정의되는 모든 멤버를 의미한다.
   - Person.prototype.x()

---

### Class에서의 상속
```js
  class Person {
   constructor(name, alias, age, gender) {
      this.name = {
         name,
         alias,
      };
      this.age = age;
      this.gender = gender;
   }

   greeting() {
      console.log(this.name.alias, this.age);
   }
  }

  class Developer extends Person {
    constructor(name, alias, age, gender, position) {
      super(name, alias, age, gender);
      this.position = position;
    }
  }
```

## 유지보수를 위한 팁

상속을 구현할 때 상속 레벨을 너무 깊게 하지말고 메소드와 속성들이 정확히 어디에 구현되어 있는지 인지해야한다.  
브라우저 내장 객체의 prototype는 수정이 가능하지만 정말로 필요한 경우를 제외하고는 건드리지 말아야 한다.  
서로 연관된 변수와 함수들을 하나로 묶어 다룰 필요가 있을 때 활용하는 것이 좋다.  
또한 한 곳에서 다른 곳으로 테이터 집합을 전달할 때에도 객체가 유용하다.


---

## 정리하면서
자바스크립트는 왜 prototype이란 개념을 가지게 된걸까?   
내 의문을 어렴풋이 이해하게 해준 블로그를 추가로 링크한다.   
[자바스크립트는 왜 프로토타입을 선택했을까?](https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42)