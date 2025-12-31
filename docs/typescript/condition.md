---
layout: default
title: 상황에 따른 타입 (Conditional Types)
parent: Typescript
nav_order: 1
---

# Condition (조건부 타입)

타입 시스템 내에서 조건에 따라 다른 타입을 결정하는 방법을 정리합니다.

## 1. Discriminated Unions (구분된 유니온)
공통된 리터럴 타입 속성(식별자)을 사용하여 유니온 타입 내에서 타입을 안전하게 분기할 수 있습니다.

```typescript
type SuccessState = {
  result: "success";
  response: { body: string; };
};

type FailState = {
  result: "fail";
  reason: string;
};

type ApiState = SuccessState | FailState;

function printLoginState(state: ApiState) {
  if (state.result === "success") {
    // result가 success인 것이 보장되므로 response에 접근 가능
    console.log(`🎉 성공: ${state.response.body}`);
  } else {
    // result가 fail인 것이 보장되므로 reason에 접근 가능
    console.log(`😭 실패: ${state.reason}`);
  }
}
```

---

## 2. Conditional Types (조건부 타입)
`extends` 키워드와 삼항 연산자를 사용하여 타입 간의 관계에 따라 타입을 결정합니다.

```typescript
// T가 string을 상속받으면 boolean, 아니면 number 타입이 됨
type Check<T> = T extends string ? boolean : number;

type T1 = Check<string>;  // boolean
type T2 = Check<number>;  // number
```

### 활용 예시: 타입 이름 추출
```typescript
type TypeName<T> = 
  T extends string ? "string" :
  T extends number ? "number" :
  T extends boolean ? "boolean" :
  T extends undefined ? "undefined" :
  T extends Function ? "function" :
  "object";

type T0 = TypeName<string>;         // "string"
type T3 = TypeName<() => void>;     // "function"
```
