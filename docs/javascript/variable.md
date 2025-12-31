---
layout: default
title: 변수
parent: Javascript
nav_order: 1
---

# Variable (변수)

자바스크립트의 변수는 기본적으로 **호이스팅(Hoisting)**이 발생합니다. `var`는 호이스팅 시 초기화까지 이루어지는 반면, `let`과 `const`는 TDZ(Temporal Dead Zone)로 인해 초기화 전 접근이 제한됩니다.

## 1. 변수의 생명주기
1. **선언 (Declaration)**: 스코프와 변수 객체가 생성되고 스코프가 변수 객체를 참조합니다.
2. **초기화 (Initialization)**: 변수 값을 저장하기 위한 메모리 공간을 할당합니다. 이 단계에서 변수는 `undefined`로 초기화됩니다.
3. **할당 (Assignment)**: 변수에 실제 데이터를 저장합니다.

---

## 2. Hoisting (호이스팅)
변수의 선언부를 코드의 최상단으로 끌어올리는 것처럼 동작하는 현상입니다. 인터프리터가 코드를 해석할 때 선언 단계와 실행 단계를 나누어 처리하기 때문에 발생합니다.

- **`var`**: 선언과 동시에 `undefined`로 초기화됩니다. 선언 전에도 접근이 가능하지만 값은 `undefined`입니다.
- **`let`, `const`**: 선언은 되지만 초기화는 되지 않습니다. 선언문 이전에 접근하려고 하면 참조 에러가 발생합니다.

```javascript
/* --- 호이스팅 예시 --- */
console.log(myName); // undefined
var myName = "ne-eun";

// console.log(name2); // ReferenceError
let name2 = "Evan";

/* --- 블록 레벨 호이스팅 --- */
let name = "ne-eun park";
if (name === "ne-eun park") {
  // console.log(name); // ReferenceError (TDZ 영향)
  let name = "terabyte";
  console.log(name); // "terabyte"
}
```

---

## 3. Temporal Dead Zone (TDZ)
`let`과 `const`로 선언된 변수들이 스코프의 시작 지점부터 초기화 지점까지 일시적으로 접근할 수 없는 구간을 의미합니다. 선언은 되었으나 아직 메모리 공간이 할당(초기화)되지 않은 상태입니다.

---

## 4. 변수 선언 키워드 비교

### `var`
- **함수 레벨 스코프**: 함수 내부에서 선언된 변수만 지역 변수로 인정됩니다.
- **중복 선언 가능**: 동일한 이름의 변수를 여러 번 선언해도 에러가 나지 않습니다.
- **호이스팅**: 선언 전 접근 가능 (`undefined`).

### `let` & `const`
- **블록 레벨 스코프**: 모든 코드 블록(if, for, while, try/catch 등) 내에서 선언된 변수를 지역 변수로 인정합니다.
- **중복 선언 불가능**: 이미 선언된 이름을 다시 사용할 수 없습니다.
- **`const`**: 선언과 동시에 반드시 값을 할당해야 하며, 재할당이 불가능합니다(상수).
