# 호이스팅

변수를 선언하고 초기화했을 때 선언 부분이 최상단으로 끌어올려지는 현상을 의미한다.

아래 `sayWow()` 처럼 **함수 표현식**이 아닌 **함수 선언식**일 경우에는 식 자체가 통째로 끌어올려진다.

```js
console.log(zero); // undefined
sayWow(); // 정상 동작
function sayWow() {
  console.log('wow');
}
var zero = 'zero';

// 위의 코드와 같다.
function sayWow() {
  console.log('wow');
}
var zero;
console.log(zero);
sayWow();
zero = 'zero';
```

하지만 같은 함수여도 함수 표현식으로 선언한 경우 에러가 발생한다.

```js
sayWow(); // (3)
sayYeah(); // (5) 여기서 대입되기 전에 호출해서 에러
var sayYeah = function () {
  // (1) 선언 (6) 대입
  console.log('yeah');
};
function sayWow() {
  // (2) 선언과 동시에 초기화(호이스팅)
  console.log('wow'); // (4)
}

// 전역 컨텍스트
'전역 컨텍스트': {
  변수객체: {
    arguments: null,
    variable: [{ sayWow: Function }, 'sayYeah'], // sayYeah 에러 발생
  },
  scopeChain: ['전역 변수객체'],
  this: window,
}
```

<br />

## **챕터**

본문

```lang
예시코드
```

<br />

## **챕터**

본문

```lang
예시코드
```

<br />

## **참고**

---

[ZeroCho's Blog >> 실행 컨텍스트 >> 호이스팅](https://www.zerocho.com/category/JavaScript/post/5741d96d094da4986bc950a0)
