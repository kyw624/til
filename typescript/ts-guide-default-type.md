# 타입스크립트 기초 개념 가이드

## 기본 타입

### 부울 (Boolean)

```ts
let isDone: boolean = false;
```

### 숫자 (Number)

TypeScript는 10진수, 16진수, 2진수, 8진수 문자를 지원한다.

```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

### 문자열 (String)

```ts
let color: string = "blue";
color: "red";
```

템플릿 문자열도 사용 가능하다.

```ts
let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${fullName}.

I'll be ${age + 1} years old next month.`;
```

### 배열 (Array)

배열 선언 방법은 두 가지가 있다.

```ts
let list: number[] = [1, 2, 3];

let list: Array<number> = [1, 2, 3];
```

### 튜플 (Tuple)

고정된 개수의 요소 타입을 알고는 있지만 반드시 같을 필요 없는 배열을 표현할 때 사용한다.

```ts
let x: [string, number];
// 튜플 타입 선언

x = ["hello", 10];
// 초기화

x = [10, "hello"];
// 부정확한 초기화 (오류)
```

알려진 인덱스에 접근

```ts
console.log(x[0].substr(1));
// 정상 작동
console.log(x[1].substr(1));
// 오류, 'number' 타입은 'substr'을 가지고 있지 않음.
```

알려진 인덱스 외부 요소에 접근할 때는 Union 타입이 사용됨.

> <b>Union 타입</b>  
> 2개 이상의 타입을 허용하는 경우 (합집합)  
> `let union: (string | number)`

```ts
x[3] = "world"; // 'string'은 'string | number'에 할당될 수 있다.

console.log(x[5].toString()); // 'string' 및 'number' 모두 'toString'이 있다.

x[6] = true; // 오류, 'boolean'은 'string | number' 타입이 아니다.
```

<br />

## 열거 (Enum)

JS의 표준 데이터 타입 집합에 추가할 수 있는 유용한 추가 자료이다.

```ts
enum Color {
  Red,
  Green,
  Blue
}
let c: Color = Color.Green;
```

enums는 기본적으로 `Zero Based Numbering` 으로 번호를 매기지만  
멤버들의 값을 수동으로 설정해 이를 바꿀 수 있다.

> 1부터 시작

```ts
enum Color {
  Red = 1,
  Green,
  Blue
}
let c: Color = Color.Green;
```

> 모든 값을 수동 설정

```ts
enum Color {
  Red = 1,
  Green = 2,
  Blue = 4
}
let c: Color = Color.Green;
```

> enum 호출의 예

```ts
enum Color {
  Red = 1,
  Green,
  Blue
}
let colorName: string = Color[2];

console.log(colorName); // 'Green'
```

<br />

## Any

코드 작성 시 알지 못하는 변수의 타입을 설명해야 할 수 있다.  
이러한 경우에 타입 검사 대신 그 값이 컴파일-타입 검사를 통과하도록 한다.

```ts
let notSure: any = 4;
notSure = "문자열일수도 있다";
notSure = false;
```

때론 타입의 일부를 알고 있는 경우에도 유용하지만 모두 그렇지는 않다.
그 예로 배열이 있는데 배열에는 다른 타입들이 섞여있다.

```ts
let list: any[] = [1, true, "free"];

list[1] = 100;
// list = [1, 100, 'free']
```

<br />

## Void

`any`와 정반대이면서 조금 비슷한 면이 있다.  
일반적으로 반환 값이 없는 함수의 반환 타입

```ts
function warnUser(): void {
  console.log("This is my warning message");
}
```

`void` 타입의 변수 선언은 `undefined` 또는 `null`만 할당이 가능해 유용하지는 않다.

```ts
let unusable: void = undefined;
```

<br />

## Null / Undefined

TypeScript에서 `null`와 `undefined`은 실제로 각각 자체적인 타입을 가진다.
`void`와 마찬가지로 매우 유용하지 않다.

```ts
let u: undefined = undefined;
let n: null = null;
```

기본적으로 `null`과 `undefined`는 다른 모든 타입의 서브 타입이다.  
`--strictNullChecks` 플래그를 사용하면 `void`와 그 각각의 타입에만 할당이 가능해 오류를 피할 수 있다.

<br />

## Never

절대로 발생하지 않는 값의 타입을 나타냄. throw되는 함수에 주로 사용된다.  
`never` 타입도 다른 모든 타입의 서브 타입이지만 다른 타입들은 `never` 타입을 가질 수 없다.  
(자기 자신은 제외)

<!-- ```ts
function error(message: string): never {
  throw new Error(message);
}

function fail() {
  return error('Something failed');
}

function infiniteLoop(): never {
	while (true) {
		...
	}
}
``` -->

<br />

## 타입 단언 (Type assertions)

타입 추론의 기능은 강력하지만 한계가 존재한다.  
컴파일러의 비정상적인 추론을 방지하기 위해 프로그래머가 특정 변수에 타입 힌트를 주는 것.

타입 단언은 두 가지 형태가 있다.

> angle-bracket (꺾쇠괄호) 구문

```ts
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

> as 구문

```ts
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

두 샘플은 동일하고 선호도의 차이지만,  
JSX와 함께 사용할 때는 as 구문만 허용된다.
