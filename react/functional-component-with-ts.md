# 리액트에서 타입스크립트 사용하기

## 컴포넌트 선언

### **function vs arrow function**

리액트 공식 문서나 해외 유명 개발자들은 보통 function 키워드를 사용하여 함수형 컴포넌트를 선언한다.

화살표 함수로 `React.FC`를 사용하고 안하는 것의 차이점은?

<br />

### **React.FC 사용**

`React.FC`를 사용 할 때는 props의 타입을 Generics으로 넣어 사용한다.

> 결론 : `React.FC`는 사용하지 **_않는 것_**을 권장

```tsx
import React from "react";

type GreetingsProps = {
  name: string;
};

const Greetings: React.FC<GreetingsProps> = ({ name }) => (
  <div>Hello, {name}</div>
);

export default Greetings;
```

이를 이용해 **이점**은 2가지가 있는데,

1. props에 기본적으로 `children`이 들어가있다.
2. 컴포넌트의 defaultProps, propsTypes, contextTypes를 설정 할 때 자동완성이 된다.

물론 **단점**도 존재한다.

1. `children`이 옵션의 형태로 들어가있다보니 props 타입이 명백하지 않다.  
   컴포넌트마다 children이 필요할 수도 있고 아닐 수도 있기 때문이다.  
    이에 대한 처리를 하려면 Props 타입 내부에 `children`을 설정해야한다.

   ```tsx
   type GreetingsProps = {
     name: string;
     children: React.ReactNode;
   };
   ```

2. `defaultProps`가 제대로 작동하지 않는다.

<br />

## **참고**

---

[velopert's GitHub >> React Tutorial >> Using TypeScript >> 02](https://github.com/velopert/react-tutorial/blob/master/using-typescript/02-ts-react-basic.md)
