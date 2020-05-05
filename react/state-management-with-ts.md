# 리액트에서 타입스크립트 사용하기

## 상태 관리

### **useState**

`useState<number>()` 와 같이 `Generics`을 사용해 해당 상태의 타입을 설정해주기만하면 된다.

참고로 `Generics`을 사용하지 않아도 타입을 알아서 유추하기때문에 생략해도 문제는 없으나 **사용해야 할 상황**이 있다.  
상태가 `null`일 수도 있고, 아닐 수도 있을 때이다.

- 카운터 예시

  ```tsx
  import React, { useState } from "react";

  function Counter() {
    const [count, setCount] = useState < number > 0;
    const onIncrease = () => setCount(count + 1);
    const onDecrease = () => setCount(count - 1);

    return (
      <div>
        <h1>{count}</h1>
        <div>
          <button onClick={onIncrease}>+1</button>
          <button onClick={onDecrease}>-1</button>
        </div>
      </div>
    );
  }

  export default Counter;
  ```

- null 예시

  ```tsx
  type Information = { name: string; description: string };

  const [info, setInfomation] = useState<Information | null>(null);
  ```

### **useReducer**

액션은 `|` 로 연달아 쭉 작성하며 그 외에는 같으며,

> `type Action = { type: '액션1' } | { type: '액션2'} | ...`

액션 객체에 여러 값들이 있을 경우 각 타입을 명시해주면 아래와 같은 이점이 있다.

```
1. 리듀서 작성 시 자동완성
2. 디스패치 시 타입검사
3. 필요한 값 없을 시 에러 발생 (실수방지)
```

- 리듀서 예시

  ```tsx
  import React, { useReducer } from "react";

  type Color = "red" | "orange" | "yellow";

  type State = {
    count: number;
    text: string;
    color: Color;
    isGood: boolean;
  };

  type Action =
    | { type: "SET_COUNT"; count: number }
    | { type: "SET_TEXT"; text: string }
    | { type: "SET_COLOR"; color: Color }
    | { type: "TOGGLE_GOOD" };

  function reducer(state: State, action: Action): State {
    switch (action.type) {
      case "SET_COUNT":
        return {
          ...state,
          count: action.count,
        };
      case "SET_TEXT":
        return {
          ...state,
          text: action.text,
        };
      case "SET_COLOR":
        return {
          ...state,
          color: action.color,
        };
      case "TOGGLE_GOOD":
        return {
          ...state,
          isGood: !state.isGood,
        };
      default:
        throw new Error("Unhandled action");
    }
  }

  function ReducerSample() {
    const [state, dispatch] = useReducer(reducer, {
      count: 0,
      text: "hello",
      color: "red",
      isGood: true,
    });

    const setCount = () => dispatch({ type: "SET_COUNT", count: 5 });
    // count 없으면 에러
    const setText = () => dispatch({ type: "SET_TEXT", text: "bye" });
    // text 없으면 에러
    const setColor = () => dispatch({ type: "SET_COLOR", color: "orange" });
    // orange 없으면 에러
    const toggleGood = () => dispatch({ type: "TOGGLE_GOOD" });

    return (
      <div>
        <p>
          <code>count: </code> {state.count}
        </p>
        <p>
          <code>text: </code> {state.text}
        </p>
        <p>
          <code>color: </code> {state.color}
        </p>
        <p>
          <code>isGood: </code> {state.isGood ? "true" : "false"}
        </p>
        <div>
          <button onClick={setCount}>SET_COUNT</button>
          <button onClick={setText}>SET_TEXT</button>
          <button onClick={setColor}>SET_COLOR</button>
          <button onClick={toggleGood}>TOGGLE_GOOD</button>
        </div>
      </div>
    );
  }

  export default ReducerSample;
  ```

<br />

## **참고**

---

[velopert's GitHub >> React Tutorial >> Using TypeScript >> 03](https://github.com/velopert/react-tutorial/blob/master/using-typescript/03-ts-manage-state.md)
