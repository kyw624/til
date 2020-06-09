# 타입스크립트 Interface vs Type

## 결론

가장 큰 차이점은 **Declaration Merging (선언 병합)** 이다.

> ### **선언 병합?**
>
> 같은 이름으로 선언된 두 개의 선언을 컴파일러에 의해 하나로 정의해 병합된다.  
> 병합할 선언의 수는 상관없다.
>
> ```ts
> interface Box {
>   height: number;
>   width: numebr;
> }
>
> interface Box {
>   scale: number;
> }
>
> let box: Box = { height: 5, width: 6, scale: 10 };
> ```

- `interface`는 컴파일 시점에서 동일한 이름으로 선언해도 컴파일 시점에서 합쳐지기 때문에 확장성이 좋다.  
  따라서 일반적으로는 `interface`를,  
  `union`, `tuple` 등이 필요한 경우 `type`을 사용하자 (TypeScript Handbook)

- 외부에 노출되는 타입은 `interface`,  
  그게 아니라면 `type`을 사용하자

<br />

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
