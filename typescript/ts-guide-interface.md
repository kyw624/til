# 타입스크립트 기초 개념 가이드

## 인터페이스

### 개요

인터페이스는 일반적으로 타입 체크를 위해 사용되며 변수, 함수, 클래스에 사용 가능하다.  
프로퍼티와 메소드를 가질 수 있다는 점에서 클래스와 유사하지만  
직접 인스턴스를 할 수 없고 모든 메소드가 추상 메소드이다.  
그러나 추상 클래스와는 달리 `abstract` 키워드는 사용하지 않는다.

<br />

### 변수와 인터페이스

인터페이스는 변수의 타입으로 사용이 가능하다.  
이때 인터페이스를 타입으로 선언한 변수는 해당 인터페이스를 준수해야한다.  
이것은 새로운 타입을 정의하는 것과 유사하다.

```ts
interface Todo {
  id: number;
  content: string;
  completed: boolean;
}

// 변수 todo의 타입으로 Todo 인터페이스를 선언
let todo: Todo;

todo = { id: 1, content: "typescript", completed: false };
```

인터페이스는 함수 파라미터의 타입으로도 사용 가능하다.  
인수는 해당 인터페이스에 준수해 전달해야 한다.  
함수에 객체 전달 시 복잡한 매개변수 체크가 필요없어 매우 유용하다.

```ts
interface Todo {
  id: number;
  content: string;
  completed: boolean;
}

let todos: Todo[] = [];

function addTodo(todo: Todo) {
  todos = [...todos, todo];
}

const newTodo: Todo = { id: 1, content: "typescript", completed: false };

addTodo(newTodo);
console.log(todos); // [ { id: 1, content: 'typescript', completed: false } ]
```

<br />

### 함수와 인터페이스

인터페이스는 함수의 타입으로도 사용이 가능하다.  
이때 함수의 인터페이스에는 타입이 선언된 파라미터 리스트와 리턴 타입을 정의한다.

```ts
interface SquareFunc {
  (num: number): number;
}

const squareFunc: SquareFunc = function(num: number) {
  return num * num;
};

console.log(squareFunc(10)); // 1000
```

<br />

### 클래스와 인터페이스

클래스 선언문의 implements 뒤에 인터페이스 선언 시 해당 클래스는 지정된 인터페이스를 반드시 구현해야한다.
이는 인터페이스를 구현하는 클래스의 일관성을 유지할 수 있는 장점이 있다.

```ts
interface ITodo {
  id: number;
  content: string;
  completed: boolean;
}

class Todo implements ITodo {
  constructor(
    public id: number,
    public content: string,
    public completed: boolean
  ) {}
}

const todo = new Todo(1, "Typescript", false);

console.log(todo);
```

인터페이스는 프로퍼티뿐만이 아닌 메소드도 포함할 수 있다. 단, 모든 메소드는 추상 메소드일 것!

```ts
interface IPerson {
  name: string;
  sayHello(): void;
}

class Person implements IPerson {
  constructor(public name: string) {}

  sayHello() {
    console.log(`Hello ${this.name}`);
  }
}

function greeter(person: IPerson): void {
  person.sayHello();
}

const me = new Person("Kim");
greeter(me);
```

<br />

### 덕 타이핑 (Duck typing)

주의점은 인터페이스를 구현한 것만이 타입 체크를 통과하는 유일한 방법은 아니다.  
타입 체크에서 중요한 것은 실제로 값을 가지고 있는 것이다.

```ts
interface IDuck {
  // (1)
  quack(): void;
}

class MallardDuck implements IDuck {
  // (3)
  quack() {
    console.log("Quack!");
  }
}

class RedheadDuck {
  // (4)
  quack() {
    console.log("q~uack!");
  }
}

function makeNoise(duck: IDuck): void {
  // (2)
  duck.quack();
}

makeNoise(new MallardDuck()); // Quack!
makeNoise(new RedheadDuck()); // q~uack!	// (5)
```

1. 인터페이스 `IDuck`은 `quack` 메소드를 정의
2. `makeNoise` 함수는 인터페이스 `IDuck`을 구현한 클래스 인스턴스 `duck`을 인자로 전달받음
3. 클래스 `MallardDuck`은 인터페이스 `IDuck`을 구현
4. 클래스 `RedheadDuck`은 인터페이스 `IDuck`을 구현하지않았지만 `quack` 메소드를 가짐
5. `makeNoise` 함수에 인터페이스 `IDuck`을 구현하지 않은 클래스 `RedheadDuck`의 인스턴스를 인자로 전달해도 에러없이 처리됨

이처럼 TypeScript는 해당 인터페이스에서 정의한 프로퍼티나 메소드를 가진다면 그 인터페이스를 구현한 것으로 인정한다.
이것을 덕 타이핑 또는 구조적 타이핑이라 한다.  
덕 타이핑은 변수에 사용할 경우에도 적용된다.

```ts
interface IPerson {
  name: string;
}

function sayHello(person: IPerson): void {
  console.log(`Hello ${person.name}`);
}

const me = { name: "Kim", age: 26 };
sayHello(me); // Hello Kim
```

변수 `me`는 인터페이스 `IPerson`과 일치하지는 않는다. 그러나 `IPerson`의 `name` 프로퍼티를 가지고있기때문에 인터페이스에 부합한다고 인정된다.

인터페이스는 개발 단계에서 도움을 주지만 자바스크립트의 표준은 아니다.  
따라서 위 예제들의 TypeScript 파일들을 트랜스파일링하면 아래와 같이 인터페이스가 삭제된다.

```js
function sayHello(person) {
  console.log("Hello " + person.name);
}

let me = { name: "Kim", age: 26 };
sayHello(me); // Hello Kim
```
