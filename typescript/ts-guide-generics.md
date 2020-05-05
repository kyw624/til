# 타입스크립트 기초 개념 가이드

## Generics

### **개요**

타입스크립트에서 함수, 클래스, interface, type alias 사용 시 여러 종류의 타입을 호환해야 하는 상황에 쓰이는 문법이다.

<br />

### **문법**

선언 시에 `<T>`라는 임의의 타입이 들어갈 식별자를 적어주고 선언한다.  
Generics 사용시 관용적으로 `T`부터 순서대로 사용한다.

이후 생성자로 호출 시에 사용 할 타입을 `<T>` 자리에 지정해준다.

<br />

### **함수에서 사용**

- 객체 A, B를 합쳐주는 임의의 함수 생성 **_(Generics X)_**

  ```ts
  function merge(a: any, b: any): any {
    return {
      ...a,
      ...b,
    };
  }

  const merged = merge({ foo: 1 }, { bar: 2 });
  ```

  1. 어떤 타입이 올 지 모르니 any 타입 선언
  2. 결과가 any 이므로 타입 유추 깨짐
  3. Generics 사용해 문제 해결

<br />

- Generics 사용 (2가지 예시)

  ```ts
  function merge<A, B>(a: A, b: B): A & B {
    return {
      ...a,
      ...b,
    };
  }

  const merged = merge({ foo: 1 }, { bar: 2 });
  ```

  ```ts
  function wrap<T>(param: T) {
    return {
      param,
    };

    const wrapped = wrap(10);
  }
  ```

<br />

### **Interface에서 사용**

```ts
interface Items<T> {
  list: T[];
}

const items: Items<string> = {
  list: ["a", "b", "c"],
};
```

<br />

### **Type에서 사용**

```ts
type Items<T> = {
  list: T[];
};

const items: Items<string> = {
  list: ["a", "b", "c"],
};
```

<br />

### **클래스에서 사용**

```ts
class Queue<T> {
  list: T[] = [];
  get length() {
    return this.list.length;
  }
  enqueue(item: T) {
    this.list.push(item);
  }
  dequeue() {
    return this.list.shift();
  }
}

const queue = new Queue<number>();

queue.enqueue(0); // Queue = [0];
queue.enqueue(1); // Queue = [0, 1];
queue.enqueue(2); // Queue = [0, 1, 2];
queue.enqueue(3); // Queue = [0, 1, 2, 3];

queue.dequeue(); // 0 | Queue = [1, 2, 3];
queue.dequeue(); // 1 | Queue = [2, 3];
queue.dequeue(); // 2 | Queue = [3];
queue.dequeue(); // 3 | Queue = [];
```

<br />

## **참고**

---

[velopert's GitHub >> React Tutorial >> Using TypeScript >> 01](https://github.com/velopert/react-tutorial/blob/master/using-typescript/01-practice.md)
