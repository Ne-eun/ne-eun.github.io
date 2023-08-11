---
layout: default
title: Typescript
has_children: true
has_toc: true
nav_order: 2
---

# Typescript

## 타입스크립트의 컴파일러
기존의 컴파일러는 텍스트를 파싱하여 추상 문법 트리(abstract syntax tree, AST)라는 자료구조로 변환한다.   
그 후 변환된 AST를 바이트코드로 변환하고 런타임이라는 프로그램에 입력해 평가하고 결과를 얻는다.   
그러나 타입스크립트는 AST로 변환하기전 타입을 확인하고 자바스크립트로 컴파일한다.   
자바스크립트로 컴파일하는 과정에서의 타입은 확인하지 않는다.   
따라서 자바스크립트 실행에 기존에 작성한 타입은 아무런 영향을 주지 않는다.   
