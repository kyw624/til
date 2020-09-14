# Context API

특정 함수가 특정 컴포넌트를 거쳐서 원하는 컴포넌트에 전달되는 경우  
1개의 컴포넌트 정도는 거쳐도 큰 불편함이 없지만 3~4개 이상을 거치게 되면 번거롭게 되는데  
이 때 `Context API`를 사용해 프로젝트 내에서 전역적으로 값을 관리해 이런 구조를 해결할 수 있다.

<br />

## **Context 생성**

`React.createContext()` 함수 사용.

```js
const name = React.createContext(value);
// 변수명에는 이름, value에는 기본값을 설정할 수 있다.

<UserDispatch.Provider value={dispatch}>...</UserDispatch.Provider>;
// Context 생성 후 Provider라는 내부 컴포넌트를 통해 Context 값을 정할 수 있으며, value 값을 설정해주면 된다.
// 이렇게 설정해주면 Provider에 의해 감싸진 컴포넌트 중 어디서든 Context의 값을 조회해서 사용할 수 있다.
```

<br />

## **Context 조회**

`useContext` Hook 사용.

```js
// dispatch 함수를 불러옴.
const dispatch = useContext(name);

// 예시
const dispatch = useContext(UserDispatch);
```

<br />

## **참고**

---

[벨로퍼트와 함께하는 모던 리액트 >> Context API 를 사용한 전역 값 관리](https://react.vlpt.us/basic/22-context-dispatch.html)
