---
layout: default
title: 상황에 따른 타입
parent: Typescript
nav_order: 1
---

# Condition (조건)

## 키에 따라 다른 타입

```ts
type SuccessState = {
  result: "success";
  response: {
    body: string;
  };
};
type FailState = {
  result: "fail";
  reason: string;
};
type ApiState = SuccessState | FailState;

function login(): ApiState {
  return {
    result: "fail",
    response: {
      body: "logged in!", // 에러: result가 fail인경우 response.body를 가질 수 없음
    },
  };
}

function printLoginState(state: LoginState) {
  if (state.result === "success") {
    // 필수key로 조건을 만들고 분기할 수 있음
    console.log(`🎉 ${state.response.body}`);
  } else {
    console.log(`😭 ${state.reason}`);
  }
}
```

## 조건문
```ts
type Check<T> = T extends string ? boolean : number;
type Type = Check<string>; // boolean

// 조건을 달아서 타입을 설정 할 수 있음
type TypeName<T> = T extends string
  ? "string"
  : T extends number
  ? "number"
  : T extends boolean
  ? "boolean"
  : T extends undefined
  ? "undefined"
  : T extends Function
  ? "function"
  : "object"

type T0 = TypeName<string>  // type string
type T2 = TypeName<() => void>  // type function
```
