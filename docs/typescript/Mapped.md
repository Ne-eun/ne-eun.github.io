---
layout: default
title: 기본내장타입
parent: Typescript
nav_order: 1
---

# Utility Types (기본내장타입)
버전에 따라 타입이 없을 수 있음

{: .no_toc}

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>


[공식문서: 유틸리티 타입](https://www.typescriptlang.org/docs/handbook/utility-types.html#picktype-keys)

### Awaited\<T>

### Partial\<T>
```ts
type ToDo = {
  title: string;
  description: string;
  label: string;
  priority: 'high' | 'low';
};

const toDoA: Partial<ToDo> = {
  title: "제목",
  description: "설명"
}
```

### Required\<T>
### Readonly\<T>
### Record\<T>
```ts
type PageInfo = {
  title: string;
};
type Page = 'home' | 'about' | 'contact';

const nav: Record<Page, PageInfo> = {
  home: { title: 'Home' },
  about: { title: 'About' },
  contact: { title: 'Contact' },
};
```

### Pick\<T, Keys>
Type중 Keys만 포함

### Omit\<T, Keys>
Keys를 제외시킨 Type


### Exclude\<T, ExcludedMembers>
```ts
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; x: number }
  | { kind: "triangle"; x: number; y: number };
 
type T3 = Exclude<Shape, { kind: "circle" }>
     
type T3 = {
  kind: "square";
  x: number;
} | {
  kind: "triangle";
  x: number;
  y: number;
}
```

### Extract\<T, Union>
Exclude의 반대

### NonNullable\<T>

### Parameters\<T>
함수의 인자를 추출해 튜플로 만든다.
```ts
type T1 = Parameters<(s: string) => void>; //[s: string]
```

### ReturnType\<T>
### InstanceType\<T>

## 문자열 변형
### Uppercase\<StringType>
### Lowercase\<StringType>
### Capitalize\<StringType>
### Uncapitalize\<StringType>