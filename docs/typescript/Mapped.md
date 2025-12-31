---
layout: default
title: 내장 타입 (Utility Types)
parent: Typescript
nav_order: 2
---

# Utility Types (유틸리티 타입)
{: .no_toc}

타입스크립트에서 제공하는 내장 유틸리티 타입들을 정리합니다. 특정 기능을 손쉽게 구현할 수 있도록 도와줍니다.

<details open markdown="block">
  <summary>Table of contents</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## 1. 변형 유틸리티

### `Partial<T>`
모든 프로퍼티를 선택 사항(Optional)으로 바꿉니다.
```typescript
type ToDo = { title: string; description: string; };
const toDo: Partial<ToDo> = { title: "내일의 할 일" }; // description 생략 가능
```

### `Required<T>`
모든 프로퍼티를 필수 사항으로 바꿉니다.

### `Readonly<T>`
모든 프로퍼티를 읽기 전용으로 바꿉니다.

---

## 2. 선택 유틸리티

### `Pick<T, Keys>`
타입 `T`에서 `Keys`에 해당하는 속성만 추출하여 새로운 타입을 만듭니다.

### `Omit<T, Keys>`
타입 `T`에서 `Keys`에 해당하는 속성만 제외하고 새로운 타입을 만듭니다.

### `Record<Keys, T>`
키와 값의 타입을 매핑하여 객체 타입을 만듭니다.
```typescript
type Page = 'home' | 'about';
const nav: Record<Page, { title: string }> = {
  home: { title: '홈' },
  about: { title: '소개' }
};
```

---

## 3. 조건부 추출 유틸리티

### `Exclude<T, ExcludedMembers>`
유니온 타입 `T`에서 특정 멤버를 제외합니다.

### `Extract<T, Union>`
유니온 타입 `T`에서 특정 멤버만 추출합니다.

### `NonNullable<T>`
타입에서 `null`과 `undefined`를 제외합니다.

---

## 4. 함수 관련 유틸리티

### `Parameters<T>`
함수의 매개변수 타입을 튜플로 추출합니다.

### `ReturnType<T>`
함수의 반환 타입을 추출합니다.

---

## 5. 문자열 조작 타입
- **`Uppercase<S>`**: 대문자로 변환
- **`Lowercase<S>`**: 소문자로 변환
- **`Capitalize<S>`**: 첫 글자를 대문자로 변환
- **`Uncapitalize<S>`**: 첫 글자를 소문자로 변환

---
**공식 문서 참고**: [Typescript Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)