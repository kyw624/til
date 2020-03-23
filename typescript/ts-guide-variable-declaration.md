# 타입스크립트 기초 개념 가이드

## 변수 선언

### 변수의 Shadowing

중첩된 스코프에서 기존의 변수 이름을 사용하는 것을 뜻하는데,  
버그 유발 및 예방을 동시에 일으킬 수 있다.

```ts
function f(condition, x) {
  if (condition) {
    let x = 100;

    return x;
  }
  return x;
}

f(false, 0); // '0'
f(true, 0); // '100'
```

Shadowing은 보통 더 명확한 코드 작성을 위해 피해야한다.

### 블록-스코프 변수 캡쳐

스코프는 실행될 때마다 변수의 환경을 생성하는데,  
이 환경 및 캡쳐링 된 변수는 모든 항목이 완료된 후에도 존재할 수 있다.

```ts
function theCityThatAlwaysSleeps() {
  let getCity;

  if (true) {
    let city = "Seattle";
    getCity = function() {
      return city;
    };
  }
  return getCity();
}
```

### 비구조화 (Destructuring)

#### 배열 비구조화 (Array destructuring)

가장 간단한 구조 해체 방법이다.

```ts
let input = [1, 2];
let [first, second] = input;
// first = input[0];
// second = input[1];

console.log(first, second); // 1 2
```

비구조화는 이미 선언된 변수에서도 작동한다.

```ts
[first, second] = [second, first];
// 변수 교환
```

매개 변수로 쓰일 경우

```ts
function f([first, second]: [number, number]) {
  console.log(first, second);
}
f([1, 2]); // 1 2
```

`...` 구문 (Spread)을 사용해 나머지 항목에 대한 변수 생성 가능

```ts
let [first, ...rest] = [1, 2, 3, 4];
console.log(first, rest); // 1 [2, 3, 4]
```

또 다른 예

```ts
let [first] = [1, 2, 3, 4];
console.log(first); // 1

let [, second, , fourth] = [1, 2, 3, 4];
console.log(second, fourth); // 2 4
```

#### 객체 비구조화 (Object destructuring)

객체도 해체가 가능하다.

```ts
let o = {
  a: "foo",
  b: 12,
  c: "bar"
};
let { a, b } = o;
console.log(a, b); // 'foo' 12
```

배열 비구조화처럼 선언없이 할당도 가능하다.

```ts
({ a, b } = { a = "baz", b: 101 });
// 괄호로 묶어줘야한다.
```

`...` 구문을 사용해 객체 나머지 항목에 대해 변수 생성 가능.

```ts
let { a, ...passthrough } = o;
// a: 'foo', passthrough: { b: 12, c: 'bar }
let total = passthrough.b + passthrough.c.length;
```

### 프로퍼티 이름 변경 (Property renaming)

프로퍼티의 이름 변경도 가능하다.

```ts
let { a: newName1, b: newName2 } = o;
// let newName1 = o.a;
// let newName2 = o.b;
```

또 다른 예

```ts
let { a, b }: { a: string; b: number } = o;
// 전체 형식이 비구조화된 후에도 형식 작성이 필요하다.
```

#### 기본값 (Default values)

프로퍼티가 정의되지 않은 경우의 기본값 지정이 가능하다.

```ts
function keepWholeObject(wholeObject: { a: string; b?: number }) {
  let { a, b = 1001 } = wholeObject;
}
```

### 함수 선언 (Function declarations)

```ts
typc C = {a:string, b?: number}

function f({a, b}: C): void {
	...
}
```

함수 선언시에도 비구조화가 가능하지만 매개변수에 기본값을 지정하는 것이 더 일반적이며,  
비구조화시 기본값을 가져오기 까다로울 수 있다.

그러므로 우선 기본값 앞에 패턴을 두는 것을 기억해야 한다.

```ts
function f({ a, b } = { a: "", b: 0 }): void {
	...
}

f();	// 기본값 { a: '', b: 0 }
```

그런 다음 기본 초기화가 아닌 비구조화 프로퍼티에 대해 선택적인 프로퍼티를 기본값으로 지정한다.

```ts
function f({ a, b = 0 } = { a: '' }) : void {
	...
}

f({ a: 'yes' });	// 기본값 b = 0
f();	// 기본값은 { a: '' } 이면서 이 경우 기본값은 b = 0
f({});	// 에러, 인수 제공하려면 'a'가 필요함.
```

이렇듯 비구조화는 조심히 사용해야하며, 이해할 수 없는 너무 깊은 형태는 혼란을 준다.  
비구조화 표현식은 작고 심플하게 유지해야하며, 생성한 비구조화는 항상 스스로 쓸 수 있어야한다.

### 전개 연산자 (Spread)

전개는 비구조화의 반대이다.  
배열을 다른 배열로, 객체를 다른 객체로 전개하는 것을 허용한다.  
그 예로

```ts
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];
// bothPlus = [0, 1, 2, 3, 4, 5];
```

전개는 first와 second의 얕은 복사본을 만들며 그것들은 전개에 의해 변하지 않는다.

아래는 객체의 전개

```ts
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { ...defaults, food: "rich" };
// search = { food: 'rich', price: '$$', ambiance: 'noisy' };
```

객체의 전개는 나중에 전개한 객체의 프로퍼티가 이전에 있던 프로퍼티를 덮어쓴다. (왼쪽 -> 오른쪽)  
이를 수정하면

```ts
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { food: "rich", ...defaults };
// search = { food: 'spicy', price: '$$', ambiance: 'noisy' };
// food 값 다시 덮어써짐.
```

Spread의 한계

1. 객체의 인스턴스를 전개할 때 메소드를 잃어버린다.

   ```ts
   class Ex {
     value = 12;
     method() {
       ...
     }
   }

   let a = new Ex();
   let clone = { ...a };
   clone.p;  // 12
   clone.m();  // 에러
   ```

2. TypeScript 컴파일러는 일반 함수의 매개변수를 전개로 허용하지 않는다.
