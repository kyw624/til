# Scope (범위)

기본적으로 외부에서 var로 선언한 변수는 전역 변수로 전역 스코프(Global Scope)에 저장된다.  
전역 변수를 만드는 일은 **지양**해야 한다. (변수가 섞일 위험이 있다.)

```js
var x = 'global';
function ex() {
  var x = 'local';
  x = 'change'; // ex 함수 내 지역 변수 x의 값에 접근
}
ex();
console.log(x); // global
```

<br />

## **스코프 (Scope)**

위의 상황에서 함수 스코프로 인해 지역변수는 전역변수에 영향을 끼칠 수 없다.  
함수 안에서 선언된 변수는 해당 함수 안에서만 사용할 수 있다.

자바스크립트는 변수의 범위를 호출한 함수의 지역 스코프부터 전역 스코프까지 점차 넓혀가며 탐색한다.

```js
var x = 'global';
function ex() {
  x = 'change'; // 지역 변수가 없으므로 전역 변수 x에 접근
}
ex();
console.log(x); // change
```

<br />

## **스코프 체인 (Scope Chain)**

함수는 변수를 찾을 때 먼저 자신의 스코프에서 찾고,  
없으면 한 단계 위 스코프에서 찾고, 반복하며 결국 전역 스코프까지 올라간다.  
이렇게 꼬리를 물고 범위를 넓히면서 찾는 것을 **스코프 체인**이라고 부른다.

내부 함수에서는 외부 함수의 변수에 접근이 가능하지만,  
그 반대인 외부 함수에서 내부 함수의 변수에는 접근할 수 없다.

```js
var name = 'zero';
function outer() {
  console.log('외부', name); // 외부 zero
  function inner() {
    var enemy = 'nero';
    console.log('내부', name); // 내부 zero
  }
  inner();
}
outer();
console.log(enemy); // undefined
```

<br />

## **렉시컬 스코핑 (Lexical Scoping)**

주의 할 점으로 스코프는 함수를 **호출**할 때가 아닌 **선언**할 때 생긴다.  
정적 스코프라고도 불린다.

```js
var name = 'zero';
function log() {
  console.log(name);
}

function wrapper() {
  name = 'nero';
  log();
}
wrapper(); // nero
```

```js
var name = 'zero';
function log() {
  console.log(name);
}

function wrapper() {
  var name = 'nero';
  log();
}
wrapper(); // zero
```

<br />

## **스코프, IIFE를 활용한 비공개 변수 구현**

> **IIFE (즉시 호출 함수 표현식)**  
> 모듈 패턴이라고도 하며, 라이브러리를 만들 때 기본인 구문으로 함수를 선언하자마자 실행시킨다.  
> 비공개 변수가 없는 자바스크립트에 비공개 변수 기능을 만들어준다.

```js
var newScope = (function () {
  var x = 'local';
  return {
    y: function () {
      console.log(x);
    },
  };
})();
```

<br />

## **참고**

---

[ZeroCho's Blog >> 함수의 범위(scope)](https://www.zerocho.com/category/Javascript/post/5740531574288ebc5f2ba97e)
