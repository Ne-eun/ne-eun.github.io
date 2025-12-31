---
layout: default
title: 자료형
parent: Javascript
nav_order: 1
---

# 자료형 (Data Types)
{: .no_toc}

자바스크립트의 변수는 특정 타입에 고정되지 않으며, 모든 타입의 값을 할당하거나 재할당할 수 있습니다.

<details open markdown="block">
  <summary>Table of contents</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## 1. 원시 자료형 (Primitive Type)
객체가 아니며 메서드를 가지지 않는 데이터입니다. 값이 메모리에 직접 저장되며 **불변(Immutable)**합니다.

### String
- 텍스트 데이터를 나타냅니다.
- 자바스크립트에서는 문자열을 기본형으로 취급합니다.
- 한 번 생성된 문자열은 수정할 수 없으며, 새로운 문자열을 생성하는 방식(concat, substr 등)으로 조작해야 합니다.

### Number
- 부동소수점 숫자 외에 `+Infinity`, `-Infinity`, `NaN` 값을 가집니다.
- 긴 숫자를 다룰 때 `_`를 구분자로 사용할 수 있습니다. (예: `1_000_000`)

### Boolean
- 논리적 요소인 `true`와 `false`를 가집니다.

### Null & Undefined
- **Null**: 값이 비어있거나 존재하지 않음을 의도적으로 나타낼 때 사용합니다.
- **Undefined**: 값을 할당하지 않은 변수에 자동으로 할당되는 값입니다.

### BigInt
- `Number.MAX_SAFE_INTEGER`를 넘어서는 매우 큰 정수를 안전하게 다룰 때 사용합니다.
- 정수 끝에 `n`을 붙여 표현합니다. (예: `100n`)

### Symbol
- 고유하고 변경 불가능한 값으로, 주로 객체의 속성 키로 사용됩니다.
- `Symbol()` 함수를 통해 생성하며, 매번 고유한 값이 생성됩니다.

---

## 2. 참조형 데이터 (Reference Type)
원시 자료형의 집합이며, 유동적이고 메서드를 가질 수 있습니다. 메모리에는 실제 데이터가 저장된 주소(Reference)를 저장합니다.

- **Object** (객체)
- **Array** (배열)
- **Function** (함수)
- **Date, RegExp** 등

---

## 3. 메모리 구조와 복사

### 스택(Stack) vs 힙(Heap)
- **Stack**: 기본형 데이터, 정적 할당, 변수 등이 저장됩니다.
- **Heap**: 참조형 데이터, 동적 할당되는 객체들이 저장됩니다.

### 데이터 복사의 차이
- **원시형**: 값 자체가 복사됩니다. 복사된 값을 변경해도 원본에 영향을 주지 않습니다.
- **참조형**: 데이터가 저장된 메모리 주소(포인터)가 복사됩니다. 복사된 변수를 통해 데이터를 수정하면 원본 데이터도 함께 변경됩니다.

---

## 4. 가비지 컬렉션 (Garbage Collection)
어떠한 곳에서도 참조되지 않는(Reference Count가 0인) 메모리는 가비지 컬렉터에 의해 자동으로 해제되어 메모리에서 제거됩니다.
