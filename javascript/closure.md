# 클로저

반환된 내부 함수가 자신이 선언됐을 때의 스코프를 기억하여  
자신이 선언됐을 때의 스코프 밖에서 호출되어도 그 스코프에 접근할 수 있는 함수를 말한다.  
이를 간단히 말하면 **클로저는 자신이 생성될 때의 환경을 기억하는 함수**다.

```js
// innerFunc를 outerFunc 내에서 호출
function outerFunc() {
  var x = 10;
  var innerFunc = function () {
    console.log(x);
  };
  innerFunc();
}

outerFunc(); // 10
```

```js
// innerFunc를 outerFunc 내에서 반환
function outerFunc() {
  var x = 10;
  var innerFunc = function () {
    console.log(x);
  };
  return innerFunc;
}

/**
 *  함수 outerFunc를 호출하면 내부 함수 innerFunc가 반환된다.
 *  그리고 함수 outerFunc의 실행 컨텍스트는 소멸한다.
 */
var inner = outerFunc();
inner(); // 10
```

이처럼 자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우,  
외부함수 밖에서 내부함수가 호출되더라도 외부함수의 지역 변수에 접근이 가능한데  
이러한 함수를 **클로저(Closure)** 라고 부른다.

클로저에 의해 참조되는 외부함수의 변수,  
즉 outerFunc 함수의 변수 x를 **자유 변수(Free variable)** 라고 부른다.

**실행 컨텍스트**의 관점에서 설명하면,  
 내부함수가 유효한 상태에서 외부함수가 종료하여 외부함수의 실행 컨텍스트가 반환되어도,  
 외부함수 실행 컨텍스트 내의 **활성 객체(Activation object)** 는 **내부함수에 의해 참조되는 한 유효**하여 내부함수가 **스코프 체인을 통해 참조**할 수 있다.

> **활성 객체 (Activation object)**  
> 변수나 함수 선언 등의 정보를 가지고 있다.

<br />

## **클로저의 활용**

클로저가 가장 유용하게 사용되는 상황은  
**현재 상태를 기억하고 변경된 최신 상태를 유지**하는 것이다.

<br>

### **1. 전역 변수의 사용 억제**

카운터 예제

```js
// 이전 상태를 기억하지 못하는 Bad case.
var counter = 0;

function increase() {
  return ++counter;
}

incleaseBtn.onclick = function () {
  count.innerHTML = increase();
};
```

```js
// 올바른 예제
var increase = (function () {
  var counter = 0;

  return function () {
    return ++counter;
  };
})();

incleaseBtn.onclick = function () {
  count.innerHTML = increase();
};
```

즉시실행함수가 호출되고 변수 increase에 함수가 할당되는데,  
해당 함수는 자신이 생성됐을 때의 렉시컬 환경을 기억하는 클로저다.  
즉시실행함수는 호출된 이후 소멸되지만 반환한 함수는 변수 increase에 할당되어  
지역변수 counter를 기억하고 접근할 수 있다.

변수 counter는 외부에서 직접 접근할 수 없는 `private` 변수이므로  
 의도치않은 변경을 걱정할 필요도 없기때문에 안정적인 프로그래밍이 가능하다.

<br />

### **2. 정보의 은닉**

```js
function Counter() {
  // 카운트를 유지하기 위한 자유 변수
  var counter = 0;

  // 클로저
  this.increase = function () {
    return ++counter;
  };

  // 클로저
  this.decrease = function () {
    return --counter;
  };
}

const counter = new Counter();

console.log(counter.increase()); // 1
console.log(counter.decrease()); // 0
```

생성자 함수 Counter는 increase, decrease 메소드를 갖는 인스턴스를 생성한다.  
이 메소드들은 모두 Counter의 스코프에 속한 변수 counter를 기억하는 클로저이며,  
렉시컬 환경을 공유한다.

생성자 함수가 생성한 객체의 메소드는 객체의 프로퍼티뿐만 아니라 렉시컬 환경의 변수에도 접근할 수 있다.

이때 생성자 함수 Counter의 변수 counter는 this에 바인딩된 프로퍼티가 아닌 변수다.  
또한 해당 변수는 Counter 외부에서는 접근할 수 없어 클래스 기반 언어의 `private` 키워드를 흉내낼 수 있다.

<br />

## **참고**

---

[Poiemaweb.com >> 클로저](https://poiemaweb.com/js-closure)
