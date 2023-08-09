---
layout: default
title: 프로미스
parent: Javascript
nav_order: 1
---

# Promise
기본적으로 promise는 함수에 콜백을 전달하는 대신에, 콜백을 첨부하는 방식의 객체이다.  
프로미스가 생성된 시점에는 확인되지 않은 값을 위한 대리자로, 비동기 연산이 종료된 이후에 결과값 혹은 실패 사유를 처리하기 위한 처리기를 연결할 수 있다.  
프로미스를 사용하면 비동기 메서드에서 마치 동기 메서드처럼 값을 반환할 수 있다.  
다만 최종 결과를 반환하는 것이 아니고, 미래의 어떤 시점에 결과를 제공하겠다는 '약속'(프로미스)을 반환한다.

## Promise의 상태
- 대기(pending): 초기 상태
- 이행(fulfilled): 연산이 성공적으로 완료
- 거부(rejected): 연산이 실패

프로미스가 이행이나 거부될 때 프로미스의 then 메서드에 의해 대기열에 추가된 처리기들이 호출된다.  
Promise.prototype.then() 및 Promise.prototype.catch()의 반환값은 **"새로운 프로미스"** 이므로 서로 연결할 수 있다.  
프로미스가 대기에서 벗어나 이행 또는 거부된다면 프로미스가 처리(settled)됐다고 말한다.

---

## Promise의 메소드

### Promise.all()
- 주어진 모든 프로미스가 이행하거나, 한 프로미스가 거부될 때까지 대기하는 새로운 프로미스를 반환한다.
- 모든 프로미스가 이행한다면 매개변수로 제공한 각각의 이행 값을 모아놓은 배열을 반환한다.
- 거부된 프로미스가 있다면 거부된 첫 프로미스의 사유와 함께 실패로 반환한다.

### Promise.allSettled()
- 주어진 모든 프로미스가 이행되거나 거부된 후 각 프로미스에 대한 결과를 나타내는 객체배열을 반환한다.
- 반환된 각 객체별로 status를 확인할 수 있다.
- 만약 fulfilled 상태라면 value를, rejected 상태라면면 reason 속성을 확인할 수 있다.
- value나 reason을 통해 각 Promise가 어떻게 이행(또는 거부)됐는지 알 수 있다.

{: .note }
> Promise.allSettled(iterable);   
> interable: 멤버가 모두 Promise인, 배열(Array)과 같은 이터러블 객체입니다.

### Promise.any()
- 주어진 모든 프로미스 중 하나라도 이행하는 순간 즉시 이행된 값을 반환한다.

### Promise.race()
- 가장먼저 끝난 프로미스를 반환한다.

### Promise.reject()
- 실패하는 Promise 객체를 반환한다.

### Promise.resolve()
- 이행하는 Promise 객체를 반환한다.
- 어떤 값이 프로미스인지 아닌지 알 수 없는 경우 Promise.resolve()로 값을 감싸서 항상 프로미스가 되도록 만든 후 작업하는 것이 좋다.

### Promise.prototype.catch()
- 프로미스가 거부됐을 때 할 일을 정의

### Promise.prototype.then()
- 프로미스를 이행했을 때 할 일을 정의

### Promise.prototype.finally()
- 이행과 거부 여부랑 상관없이 해당 순서에 실행된다.
