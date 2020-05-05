# 타입스크립트 기초 개념 가이드

## 클래스

### 클래스 정의

ES6의 클래스는 클래스 바디에 메소드만을 포함이 가능했다.  
바디에 프로퍼티 선언이 불가능하고 생성자 내부에서 선언하고 초기화했어야한다.

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  walk() {
    console.log(`${this.name} is walking.`);
  }
}
```

위 예제의 확장자를 ts로 바꾸면 컴파일 에러가 발생한다.  
TypeScript의 클래스는 바디에 프로퍼티를 선언하기 때문이다.

```ts
class Person {
  // 프로퍼티를 사전에 선언.
  name: string;

  constructor(name: string) {
    // 프로퍼티에 값 할당
    this.name = name;
  }

  walk() {
    console.log(`${this.name} is walking.`);
  }
}
```

<br />

### 접근 제한자

- public
- private
- protected

위 3가지를 지원하며, 명시하지 않았을 경우 암묵적으로 public으로 선언된다.

> #### 접근 가능성 예제

| 접근 가능성      | public | protected | private |
| ---------------- | :----: | :-------: | :-----: |
| 클래스 내부      |   ◯    |     ◯     |    ◯    |
| 자식 클래스 내부 |   ◯    |     ◯     |    ✕    |
| 클래스 인스턴스  |   ◯    |     ✕     |    ✕    |

```ts
class Foo {
  public x: string;
  protected y: string;
  private z: string;

  constructor(x: string, y: string, z: string) {
    // 3가지 모두 클래스 내부에서 참조 O
    this.x = x;
    this.y = y;
    this.z = z;
  }
}

const foo = new Foo("x", "y", "z");

// public: 클래스 인스턴스 (객체) 를 통해 클래스 외부에서 참조 ◯
console.log(foo.x);

// protected: 클래스 인스턴스를 통해 클래스 외부에서 참조 ✕
console.log(foo.y);

// private: 클래스 인스턴스를 통해 클래스 외부에서 참조 ✕
console.log(foo.z);

class Bar extends Foo {
  constructor(x: string, y: string, z: string) {
    super(x, y, z);
	}

	// public: 자식 클래스 내부에서 참조 ◯
	console.log(this.x);

	// protected: 자식 클래스 내부에서 참조 ◯
	console.log(this.y);

	// private: 자식 클래스 내부에서 참조 ✕
	console.log(this.z);
}
```

> #### 생성자 파라미터에 접근 제한자 선언

생성자 파라미터에도 선언이 가능하다.  
접근 제한자가 사용된 파라미터는 암묵적으로 클래스 프로퍼티로 선언되고 별도의 초기화가 없어도 초기화가 수행된다.

- public: 클래스 외부에서도 참조 ◯
- private: 클래스 내부에서만 참조 ◯

```ts
class Foo {
  // 자동으로 초기화, public으로 선언되어 외부에서도 참조가 가능하다.
  constructor(public x: string) {}
}

const foo = new Foo("Hello");
console.log(foo); // Foo { x: 'Hello' }
console.log(foo.x); // Hello

class Bar {
  // 자동으로 초기화, private로 선언되어 내부에서만 참조가 가능하다.
	constructor(private x: string) { }
}

const bar = new Bar('Hello');
console.log(bar);	Bar { x: 'Hello' }
console.log(bar.x);	// 에러
```

접근 제한자를 선언하지 않으면 지역변수로써 생성자 외부에서 참조 ✕

```ts
class Foo {
  constructor(x: string) {
    console.log(x);
  }
}

const foo = new Foo("Hello");
console.log(foo); // Foo {}
```

<br />

### 키워드

> #### readonly 키워드

`readonly` 키워드를 사용할 수 있는데 readonly가 선언된 클래스 프로퍼티는  
선언 시 or 생성자 내부에서만 값 할당이 가능하다. 그 외에는 오직 읽기만 가능한 상태이다.  
이를 이용해 상수의 선언에 사용한다.

```ts
class Foo {
  private readonly MAX_LEN: number = 5;
  private readonly MSG: string;

  constructor() {
    this.MSG = "hello";
  }

  log() {
    // readonly가 선언되어 재할당 금지
    this.MAX_LEN = 10; // 에러
    this.MSG = "Hi"; // 에러
	}

	console.log(`MAX_LEN: ${this.MAX_LEN}`);	// MAX_LEN: 5
	console.log(`MSG: ${this.MSG}`);	// MSG: hello
}
```

> #### static 키워드

ES6 클래스에서는 정적 메소드를 정의한다.  
정적 메소드는 클래스명으로 호출한다.  
따라서 클래스 인스턴스를 생성하지 않아도 호출이 가능하다.

```js
class Foo {
  constructor(prop) {
    this.prop = prop;
  }

  static staticMethod() {
    // 정적 메소드는 this를 사용할 수 없다.
    // 정적 메소드 내부에서 this는 클래스 인스턴스가 아닌 클래스 자신을 가리킨다.
    return "staticMethod";
  }

  prototypeMethod() {
    return this.prop;
  }
}
// 정적 메소드는 클래스명으로 호출한다.
console.log(Foo.staticMethod());

const foo = new Foo(123);
// 정적 메소드는 인스턴스로 호출할 수 없다.
console.log(foo.staticMethod()); // 에러
```

TypeScript에서는 클래스 프로퍼티에도 static 키워드 사용이 가능하다.  
정적 메소드와 마찬가지로 클래스명으로 호출하며 인스턴스 생성이 필요없다.

```ts
class Foo {
  // 생성된 인스턴스 개수
  static instanceCounter = 0;
  constructor() {
    // 생성자가 호출될 때마다 카운터 1씩 증가
    Foo.instanceCounter++;
  }
}

let foo1 = new Foo();
let foo2 = new Foo();

console.log(Foo.instanceCounter); // 2
console.log(foo2.instanceCounter); // 에러, TS2339: Property 'instanceCounter' does not exist on type 'Foo'.
```

<br />

### 추상 클래스

추상 클래스는 하나 이상의 추상 메소드를 포함하며 일반 메소드도 포함이 가능하다.

- 추상 메소드: 내용이 없이 메소드 이름과 타입만 선언된 메소드

abstract 키워드를 사용해 선언하며, 직접 인스턴스 생성할 수 없고 상속만을 위해 사용된다.  
추상 클래스를 상속한 클래스는 해당 추상 메소드를 반드시 구현해야한다.

```ts
abstract class Animal {
	// 추상 메소드
	abstact makeSound(): void;

	// 일반 메소드
	move(): void {
		console.log('이동중...');
	}
}

class Dog extends Animal {
	// 추상 클래스를 상속했기때문에 추상 메소드 구현
	makeSound() {
		console.log('멍멍!');
	}
}

const myDog = new Dog();
myDog.makeSound(); // 멍멍!
myDog.move();	// 이동중...
```
