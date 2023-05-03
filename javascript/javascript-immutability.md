# JavaScript Immutability (불변성)

Immutability는 객체가 생성된 이후 그 상태를 변경할 수 없는 디자인 패턴을 의미하며, 함수형 프로그래밍의 핵심 원리이다.

객체는 참조 형태로 전달하고 전달 받는데, 이로 인해 상태가 언제든지 변경될 수 있어 문제가 될 가능성이 크다.  
그래서 객체를 불변 객체로 만들어 의도치않은 객체의 변경을 방지할 수도 있고,  
변경이 필요한 경우에는 참조가 아닌 방어적 복사(defensive copy)를 통한 새로운 객체를 생성해 변경한다.

<br />

## **1. immutable value vs. mutable value**

Javascript의 원시 타입(primitive data type)은 변경 불가능한 값(imutable value)이다.

> **변경 불가능한 값 (immutable value)**
>
> 메모리 영역에서의 변경이 불가능하다는 뜻. 재할당은 가능하다.

- Boolean
- null
- undefined
- Number
- String
- Symbol

원시 타입 이외의 모든 값은 객체 타입이며 객체 타입은 변경 가능한 값(mutable value)이다.

```js
// 1. 참조 예
var str = 'Hello'; // 1. 메모리에 문자열 'Hello' 생성 후 str은 해당 주소를 가리킨다.
str = 'world'; // 2. 새로운 문자열 'world'를 메모리에 생성하고 str은 이것을 가리킨다.
// 3. 이때 문자열 'Hello', 'world'는 모두 메모리에 존재하고 있다.

//
// 2. 다른 예
var user = {
  name: 'Lee',
  address: {
    city: 'Seoul',
  },
};

var myName = user.name; // 현재의 user.name이 가리키는 'Lee'를 참조.

user.name = 'Kim';
console.log(myName); // Lee. 변경 이전의 값을 참조했기에 변함 없다.

myName = user.name; // 재할당
console.log(myName); // Kim

//
// 3. 객체 참조 예
var user1 = {
  name: 'Lee',
  address: {
    city: 'Seoul',
  },
};

var user2 = user1; // 변수 user2는 객체 타입이다.

user2.name = 'Kim';

console.log(user1.name); // Kim
console.log(user2.name); // Kim
```

3번 예제와 같은 상황이 일어나는 이유는 user1과 user2가 같은 주소를 참조하고 있기 때문이다.

<br />

## **2. 불변 데이터 패턴 (immutable data pattern)**

의도하지 않은 객체의 변경이 발생하는 원인의 대다수는 “레퍼런스를 참조한 다른 객체에서 객체를 변경”하기 때문이다.  
문제 해결을 위해 불변 객체로 만들어 방지하며,  
변경이 필요한 경우에는 참조가 아닌 객체의 방어적 복사를 활용한다.

- 객체의 방어적 복사 (defensive copy)  
  `Object.assign()`
- 불변객체화를 통한 객체 변경 방지  
  `Object.freeze()`

---

### **2.1 Object.assign**

- 타겟 객체로 소스 객체의 프로퍼티를 복사한다.
- 소스 객체의 프로퍼티와 동일한 프로퍼티를 가진 타겟 객체의 프로퍼티들은 소스 객체의 프로퍼티로 덮어쓰기된다.
- 리턴값으로 타겟 객체를 반환한다.
- IE 미지원

```js
// 문법
Object.assign(target, ...sources);

//
// Copy
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
console.log(obj == copy); // false. 새로운 객체이기 때문

//
// Merge 1
const o1 = { a: 1 };
const o2 = { b: 2 };
const o3 = { c: 3 };
const merge1 = Object.assign(o1, o2, o3); // target: o1

console.log(merge1); // { a: 1, b: 2, c: 3 }
console.log(o1); // { a: 1, b: 2, c: 3 }, 타겟 객체가 변경된다!

//
// Merge 2
const o4 = { a: 1 };
const o5 = { b: 2 };
const o6 = { c: 3 };
const merge2 = Object.assign({}, o4, o5, o6); // target: {}

console.log(merge2); // { a: 1, b: 2, c: 3 }
console.log(o4); // { a: 1 }
```

또한 Object.assign은 완전한 깊은 복사(Deep copy)를 지원하지 않아 객체 내부의 객체는 얕은 복사(Shallow copy)가 된다.

```js
// 객체 내부의 객체 assign 예제
const user1 = {
  name: 'Lee',
  address: {
    city: 'Seoul',
  },
};

// 새로운 빈 객체에 user1을 copy한다.
const user2 = Object.assign({}, user1);
// user1과 user2는 참조값이 다르다.
console.log(user1 === user2); // false

user2.name = 'Kim';
console.log(user1.name); // Lee
console.log(user2.name); // Kim

// 객체 내부의 객체(Nested Object)는 Shallow copy된다.
console.log(user1.address === user2.address); // true

user1.address.city = 'Busan';
console.log(user1.address.city); // Busan
console.log(user2.address.city); // Busan
```

---

### **2.1 Object.freeze**

객체를 불변 객체로 만들어준다.

```js
const user1 = {
  name: 'Lee',
  address: {
    city: 'Seoul',
  },
};

// Object.assign은 완전한 Deep copy를 지원하지 않는다.
const user2 = Object.assign({}, user1, { name: 'Kim' });

console.log(user1.name); // Lee
console.log(user2.name); // Kim

Object.freeze(user1); // user1을 불변 객체로
user1.name = 'Kim'; // 무시된다!

console.log(user1); // { name: 'Lee', address: { city: 'Seoul' } }
console.log(Object.isFrozen(user1)); // true
```

하지만 객체 내부의 객체(Nested Object)는 변경가능한데,  
내부 객체까지 변경 불가능하게 만들려면 **Deep freeze**를 해야 한다.

```js
// 내부 객체 변경
const user = {
  name: 'Lee',
  address: {
    city: 'Seoul',
  },
};

Object.freeze(user);

user.address.city = 'Busan'; // 변경된다!
console.log(user); // { name: 'Lee', address: { city: 'Busan' } }

//
// Deep freeze
function deepFreeze(obj) {
  // 전달된 객체의 모든 속성들을 배열로 반환하는 메서드
  const props = Object.getOwnPropertyNames(obj);
  console.log(`props: ${props}`);

  props.forEach((name) => {
    const prop = obj[name];
    console.log(`prop: ${prop}`);

    if (typeof prop === 'object' && prop !== null) {
      deepFreeze(prop);
    }
  });

  return Object.freeze(obj);
}

const user = {
  name: 'Lee',
  address: {
    city: 'Seoul',
  },
};

deepFreeze(user);

user.name = 'Kim'; // 무시된다
user.address.city = 'Busan'; // 무시된다

console.log(user); // { name: 'Lee', address: { city: 'Seoul' } }
```

---

## **2.3 Immutable.js**

Object.assign과 Object.freeze를 사용한 방법은 번거럽고, 성능상 이슈가 있어 큰 객체에는 사용하지 않는 것이 좋다.

또 다른 대안으로 Facebook이 제공하는 Immutable.js를 사용하는 방법이 있다.

> 설치

`$ npm install immutable`

> 사용

```js
const { Map } = require('immutable');
const map1 = Map({ a: 1, b: 2, c: 3 });
const map2 = map1.set('b', 50); // 'b': 50을 반영한 새로운 객체를 반환.

map1.get('b'); // 2. map1은 불변하였다.
map2.get('b'); // 50
```

<br />

## **참고**

---

[생활코딩 >> JavaScript Immutability](https://www.opentutorials.org/module/4075)  
[Poiemaweb >> JavaScript >> 객체와 변경불가성(Immutability)](https://poiemaweb.com/js-immutability)
