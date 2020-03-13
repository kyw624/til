# 함수 (Function)

## 여러가지 선언 방법

- ### 함수 선언식

  ```js
  function fn() {
    ...
  }
  ```

- ### 함수 표현식

  ```js
  const fn = function() {
    ...
  };	// 세미콜론
  ```

- ### 화살표 함수 (ES6)

  ```js
  const fn = param => {
    ...
  }
  ```

  - 매개변수 1개일 때 괄호 생략 가능.
  - 별다른 처리 없이 바로 return 할 경우 간소화 가능.
  - 나머지 매개변수 및 기본 매개변수를 지원.
    ```js
    2. const fn = param => param + 1;
    // 함수의 바디에 중괄호를 쓰면 return을 써야한다.
    // 반대로 생략하면 return 필요 없음.
    3-1. const fn = (param1, param2, ...rest) => {...}
    // 나머지 매개변수
    3-2. const fn = (param1 = defaultValue1, param2) => {...}
    // 기본 매개변수
    ```

- ### 즉시실행 함수 (IIFE, Immediately Invoked Function Expression)

  ```js
  (function() {
    ...
  })();
  ```

  IIFE 자체를 변수에 할당 시 함수가 실행된 결과만 저장된다.

<br />

## 매개변수 재할당 금지

```js
// Bad Case
function badFunction(a) {
  a = 1;
}

// Good Case
function goodFunction1(a) {
  const b = a || 1;
}

function goodFunction2(a = 1) {
  ...
}
```

<br />

## 1급시민 / 1급객체 / 1급함수

> ### 1급시민

- 변수에 담을 수 있다.
- 인자로 전달이 가능하다.
- 함수의 반환값으로 전달이 가능하다.

> ### 1급객체

자바스크립트의 객체는 1급시민으로 취급된다.

```js
const a = { color: "blue" };
// 변수에 담을 수 있다.

function fn(a) {
  // 인자로 전달이 가능하다.
  const b = a;
  console.log(b.color); // blue

  return b;
  // 반환값으로 전달이 가능하다.
}
console.log(fn(a)); // result: { color: "blue" }
```

> ### 1급함수

자바스크립트의 함수는 1급시민으로 취급된다.

```js
const a = function(n) {
  return n * 10;
};
// 변수에 담을 수 있다.

function fn(func) {
  // 인자로 전달이 가능하다.

  return a(func);
  // 반환값으로 전달이 가능하다.
}
console.log(fn(a(2))); // result: 200
console.log(fn(a(3))); // result: 300
```

<br />

## 함수 (Function) vs 메소드 (Method)

### 1. 함수

객체의 일종으로 그 자체가 기능을 수행.

```js
function functionName() {
  ...
}
// 선언

functionName(); // 호출
```

### 2. 메소드

객체 내부의 값으로 존재하는 함수

```js
const object = {
  methodName: function() {
    ...
  }
};
// 선언

object.methodName();  // 호출
```
