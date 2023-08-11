---
layout: default
title: 스코프
parent: Javascript
nav_order: 1
---

# Scope (유효 범위)

## 1. 함수 스코프
- 함수 안에서 선언한 변수, 함수의 인자값은 함수안에서만 접근할 수 있다.
- var, let, const 모두 함수 범위를 가진다.

## 2. 블록 스코프
- 블록은 한 쌍의 중괄호({})로 구성되며 선택적으로 레이블(break, continue)을 붙일 수 있다.
- var로 선언한 변수는 블록 범위를 가지지 않는다.
- 블록 내에서 var로 선언한 변수의 범위는 함수나 스크립트가 되어, 값 할당의 영향이 블록 바깥까지 미친다.
- let이나 const로 선언한 변수는 블록 범위를 가진다.
- if문, for문, switch문 등이 블록에 해당되며 {}로 감싼 경우도 블록에 포함된다.
```js
var x = 1;
  {
    var x = 2;
  }
  console.log("block", x); // 2
```

## 3. 렉시컬 스코프 (Lexical Scope)
- 함수의 호출이 아닌 함수를 어디서 '선언'하였는지에 따라 상위 스코프가 결정되는 것을 말한다.
- 스코프가 컴파일타임에 결정되고 변하지 않아 정적 스코프(Static scope)로도 불린다.

```js
  let a = 3;
  function aa() {
    let a = 4;
    bb();
  }
  function bb() {
    console.log("lexical", a);
  }

  aa(); // 3
  bb(); // 3
```
## 4. 변수명 중복 허용
- 스코프의 범위가 다르다면 같은 이름의 변수명 선언이 가능하다.

## 5. 스코프 체인 (Scope Chain)
- 스코프 내부에 식별자를 찾지 못한 경우 외부함수로 나아가며 식별차를 탐색하는 것을 뜻한다.
- 최상위 함수에서도 식별자를 찾지 못한경우 에러가 발생한다.
- console.dir()을 사용하여 [Scopes]안에 있는 내용으로 조회가 가능하다.