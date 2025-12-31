---
layout: default
title: Basic
parent: Typescript
nav_order: 1
---

# Typescript 기본
{: .no_toc}

타입스크립트의 핵심 개념과 주요 보조 타입들을 정리합니다.

<details open markdown="block">
  <summary>Table of contents</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## 1. interface vs type
- **`interface`**: 주로 객체의 규격(구조)을 정의할 때 사용하며, 선언 병합이 가능합니다.
- **`type`**: 데이터의 타입을 정의하거나 유니온, 인터섹션 등 복잡한 타입을 조합할 때 유용합니다.
- 최근 버전에서는 두 방식 모두 대부분의 기능을 상호 지원하므로 프로젝트 컨벤션에 따라 선택합니다.

### 상속과 확장
```typescript
// interface 확장
interface Position2D { x: number; y: number; }
interface Position3D extends Position2D { z: number; }

// type 확장 (Intersection)
type TPosition2D = { x: number; y: number; };
type TPosition3D = TPosition2D & { z: number; };
```

---

## 2. 유용한 타입 기능

### 인덱스 접근 타입 (Indexed Access Types)
객체의 특정 키 값을 사용하여 타입을 추출할 수 있습니다.
```typescript
type Animal = { name: string; age: number; };
type Name = Animal["name"]; // string
```

### 유니온(`|`)과 인터섹션(`&`)
- **Union (`|`)**: 여러 타입 중 하나일 수 있음을 의미 (OR)
- **Intersection (`&`)**: 여러 타입을 하나로 합침 (AND)

---

## 3. 특수 타입들

### `unknown`
무엇이든 올 수 있지만, 사용하기 전에 타입을 반드시 확인(Type Guard)해야 하는 안전한 타입입니다. (`any` 대신 사용 권장)
```typescript
let a: unknown = 30;
if (typeof a === "number") {
  let d = a + 10; // 안전하게 작업 가능
}
```

### `void` & `never`
- **`void`**: 리턴값이 없는 함수 등에 사용합니다.
- **`never`**: 에러를 던지거나 무한 루프 등, 절대 결과값을 리턴하지 않는 경우에 사용합니다.

### `Readonly`
요소를 수정할 수 없도록 제한합니다.
```typescript
const arr: readonly number[] = [1, 2, 3];
// arr.push(4); // Error
```

### `Tuple`
배열의 각 요소의 타입과 길이를 고정하여 정의합니다.
```typescript
type Point = [number, number];
const p: Point = [10, 20];
```

---

## 4. enum 보다는 Union 타입을 권장하는 이유
`enum`은 타입스크립트가 자바스크립트로 변환될 때 별도의 객체를 생성하며, 예기치 않은 숫자 할당이 가능할 수 있는 등 몇 가지 단점이 있습니다. 가능한 한 **Union 타입**을 활용하는 것이 번들 사이즈와 타입 안정성 측면에서 유리합니다.

```typescript
// enum 예시
enum Days { Monday, Tuesday }

// Union 타입 권장 (더 가볍고 안전함)
type DaysOfWeek = "Monday" | "Tuesday";
```
