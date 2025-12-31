---
layout: default
title: 객체
parent: Javascript
nav_order: 1
---

# Object (객체)
{: .no_toc}

객체는 관련된 데이터와 함수의 집합입니다.  
객체 내부의 데이터를 **프로퍼티(속성)**, 함수를 **메서드**라고 부릅니다.

<details open markdown="block">
  <summary>Table of contents</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## 1. 객체 정의와 생성
생성자 함수를 사용하여 객체의 구조를 정의하고 인스턴스를 생성할 수 있습니다.

```javascript
function Person(name, alias, age, gender) {
  this.name = {
    name,
    alias,
  };
  this.age = age;
  this.gender = gender;
}

const personA = new Person('김아무개', '김씨', 26, 'M');
const personB = new Person('박아무개', '박씨', 28, 'F');
```

### 데이터 접근 방법
1. **점 표기법**: `personA.name`
2. **괄호 표기법**: `personA['name']` (키값이 동적일 때 유용)

---

## 2. Prototype (프로토타입)
모든 객체는 상위 프로토타입 객체로부터 메서드와 속성을 상속받을 수 있습니다.  
이를 **프로토타입 체인**이라고 합니다.

- 속성과 메서드는 실제 인스턴스가 아닌, 생성자의 `prototype` 속성에 정의됩니다.
- 프로토타입 체인을 타고 올라가며 필요한 자원을 탐색하므로, 메모리를 효율적으로 사용할 수 있습니다.

### 객체 상속 (ES5 방식)
```javascript
function Developer(name, alias, age, gender, position) {
  // 상위 생성자 호출
  Person.call(this, name, alias, age, gender);
  this.position = position;
}

// 프로토타입 연결
Developer.prototype = Object.create(Person.prototype);
Developer.prototype.constructor = Developer;

const personC = new Developer('최아무개', '최씨', 31, 'M', 'Front-end');
```

---

## 3. Class 기반 상속 (ES6+)
최신 자바스크립트에서는 `class` 문법을 통해 더욱 직관적으로 상속을 구현할 수 있습니다.

```javascript
class Person {
  constructor(name, alias, age, gender) {
    this.name = { name, alias };
    this.age = age;
    this.gender = gender;
  }

  greeting() {
    console.log(`${this.name.alias}님, 안녕하세요! 나이는 ${this.age}세입니다.`);
  }
}

class Developer extends Person {
  constructor(name, alias, age, gender, position) {
    super(name, alias, age, gender); // 부모 생성자 호출
    this.position = position;
  }
}
```

---

## 4. 유지보수를 위한 팁
- **상속 깊이**: 상속 레벨이 너무 깊으면 구조파악이 어려워지므로 상속 레벨은 최대 2단계로 유지하는 것이 좋습니다.
- **내장 프로토타입 수정 금지**: `Array.prototype` 같은 내장 객체는 가급적 수정하지 않는 것이 안전합니다.
- **캡슐화**: 연관된 데이터와 함수를 객체로 묶어 관리하면 테이터 전달과 코드 관리가 용이해집니다.

---

### 생각 더하기
프로토타입에 대해 막연한 생각에 조금은 가까워지게 도와준 블로그 링크를 공유합니다.  
[자바스크립트는 왜 프로토타입을 선택했을까?](https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42)