# 타입스크립트 Interface vs Type

## Interface

### 용도: 클래스와 관련된 타입에 사용

```ts
interface Person {
  name: string;
  age?: number;
}
interface Developer {
  name: string;
  age?: number;
  skills: string[];
}

const person: Person = {
  name: "사람",
  age: 26,
};

const expert: Developer = {
  name: "개발자",
  skills: ["javascript", "react"],
};
```

## Type

### 용도: 일반 객체의 타입에 사용하나 객체는 무엇이든 상관 X, 일관성만 유지

```ts
type Person = {
  name: string;
  age?: number;
};

type Developer = Person & {
  skills: string[];
};

const person: Person = {
  name: "사람",
  age: 26,
};

const expert: Developer = {
  name: "개발자",
  skills: ["javascript", "react"],
};

type People = Person[];

const people: People = [person, expert];

type Color = "red" | "orange" | "yellow";
const color: Color = "red";
const colors: Color[] = ["red", "orange"];
```

<br />

## **참고**

---

[velopert's GitHub >> React Tutorial >> Using TypeScript >> 01](https://github.com/velopert/react-tutorial/blob/master/using-typescript/01-practice.md)
