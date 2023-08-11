---
layout: default
title: 기본
parent: Typescript
nav_order: 1
---

# Base (기본적인 내용)

{: .no_toc}

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## interface와 type의 차이

interface는 구현제의 규격사항을 정의한다.  
type은 데이터의 타입을 정의할 때 사용한다.  
해당 class/function은 인터페이스에 정의된 내용을 모두 구현해야한다.  
요즘의 타입스크립트는 둘의 차이가 거의 없고 모두 구현체를 구현할 수 있다.

---

## 인터페이스의 확장

```ts
interface Position2D {
  x: number;
  y: number;
}

interface Position3D extends Position2D {
  z: number;
}

const position: Position3D = {
  x: 1,
  y: 2,
  z: 3,
};
```

## type의 확장

```ts
type Position2D = {
  x: number;
  y: number;
};

type Position3D = Position2D & {
  z: number;
};

const position: Position3D = {
  x: 1,
  y: 2,
  z: 3,
};
```

---

## key를 사용한 타입

타입을 자바스크립트의 object처럼 key를 사용하여 value를 받아 올 수 있다.

```ts
type Animal = {
  name: string;
  age: number;
  gender: "male" | "female";
};

type Name = Animal["name"]; // string
type errorName = Animal.gender // error: 점표기법은 사용할 수 없음
const text: Name = "hello";
```

---

## 유니온과 인터섹션

```ts
type a = string & number; // and
type B = string | number; // or
```

---

## 자바스크립트 타입과 다른점

### any

모든 타입을 수용한다.
~~사용금지~~

---

### unknown

어떤 타입이 올지 알 수 없을 때 any대신 사용하고, 타입을 검사해 정제할 수 있다.

```ts
let a: unknown = 30;
let b = a === 123;
let c = a + 3; // 에러
if (typeof a === "number") {
  let d = a + 10; // 에러나지않음
}
```

---

### Object

어떤 필드가 있는지는 관심없고 오로지 객체인지 확인할때 사용한다.

```ts
const a: Object = {
  x: 1,
  y: 2,
};
const b: typeof a = {}; // 에러가 발생하지 않음
```

---

### void

함수를 실행하고 리턴값이 없을 때 사용하는 타입

```ts
function print(): void {
  console.log("hello");
  return;
}
```

---

### never

error가 발생하거나, 무한 루프를 도는 함수일경우 아무런것도 리턴하지 않을 때 사용하는 타입

---

### Readonly

```ts
const a: readonly number[] = [1, 2, 3];
const b: ReadonlyArray<number> = [3, 4, 5];
a.pop(); // error
b.pop(); // error
```

읽기 전용 데이터를 생성할 수 있다.

---

### Tuple

```ts
type A = [string, number];
const b: A = [3, 4]; //error
const c: A = ["1", 2];
```

array의 서브 타입으로 길이와 타입을 정의할 수 있다.

---

### enum

시작하는 값이 number이면 순차적으로 1씩 늘어나고, string으로 지정할 경우 모두 type을 지정해 주어야 한다.  
다른 언어에서는 유용하게 사용하지만, typesciprt에서는 union타입으로 대부분 대체가 가능하다.  
가능 하면 union타입으로 사용하고, 다른 디바이스에 전송해야하는 json경우에서만 주로 사용한다.

```ts
type DaysOfWeek = "Monday" | "Tuesday" | "Wednesday";
enum Days {
  Monday,
  Tuesday,
  Wednesday,
  Thursday,
  Friday,
  Saturday,
  Sunday,
}

let day: Days = Days.Saturday;
day = 10; // 6이상의 숫자도 할당이 가능함
```

enum을 union으로 대체해서 사용하는 이유는 enum에 다른 값을 할당 할 수 있기 때문이다.
