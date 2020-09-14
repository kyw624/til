# React Hooks 정리

## **useRef**

리액트에서 상태 관리 시 상태를 바꾸는 함수를 호출해야만 조회가 가능한데  
`useRef`로 관리하는 변수는 설정 후 바로 조회가 가능하며, 리렌더링 되지 않는 특징이 있다.

`useRef`를 사용하는 경우는 다음과 같다.

- JavaScript의 `document.querySelector()`와 같이 특정 DOM을 선택해야할 때
- 컴포넌트 안에서 조회 및 수정 가능한 변수를 관리할 때

이 변수를 사용해 다음과 같은 값들을 관리할 수 있다.

- setTimeout, setInterval을 통해 만들어진 id
- 외부 라이브러리를 사용해 생성된 인스턴스
- scroll 위치

<br />

**사용예시**

`const exam = useRef(value)`  
파라미터 값이 해당 `exam.current`의 기본값이 된다.  
이 값을 조회, 수정할 때는 `exam.current` 값을 조회, 수정하면 된다.

```js
const nextId = useRef(2); // nextId.current의 기본값
console.log(nextId.current); // 2

nextId.current += 1;
console.log(nextId.current); // 3
```

<br />

## **useEffect**

`useEffect`를 사용하면 컴포넌트의 마운트, 언마운트, 업데이트 시 작업을 설정할 수 있다.

- 마운트: 컴포넌트가 처음 나타났을 때
- 언마운트: 컴포넌트가 사라질 때
- 업데이트: 컴포넌트의 특정 props가 바뀔 때

첫번째 파라미터에는 함수, 두번째 파라미터에는 의존값이 들어있는 배열 (`deps`)을 넣어준다.  
`deps` 배열을 비우게 된다면, 컴포넌트가 처음 나타날 때만 `useEffect`의 함수가 호출된다.

또 `useEffect`에서는 함수를 반환할 수 있는데 이를 `cleanup` 함수라고 한다.

<br />

**사용예시**

```js
useEffect(() => {
  // 수행 할 작업 코드
  [return ...] // cleanup 함수 [생략 가능]
}, deps)
```

> ### **cleanup 함수?**
>
> useEffect에 대한 뒷정리를 해주는 함수로 return 뒤에 오는 함수를 의미한다.  
> deps가 비어있다면 컴포넌트가 사라질 때 호출된다.

> ### **deps?**
>
> deps에 특정 값을 넣으면: 컴포넌트의 첫 마운트, 지정값 바뀔 때, 언마운트, 값 바뀌기 직전에 호출된다.  
> 파라미터를 아예 생략하면: 컴포넌트가 리렌더링 될 때마다 useEffect 함수가 호출됨.
>
> ### **※** useEffect 안에서 사용되는 `상태/props` 가 있다면 deps에 꼭 넣어줘야하는 규칙이 있다.
>
> **안넣어주면?** useEffect 함수가 실행될 때 최신 `상태/props` 를 가리키지 않는다.

<br />

**마운트 / 언마운트 시 하는 작업들**

- **마운트**
  1. props로 받은 값을 컴포넌트의 로컬 상태로 설정
  2. 외부 API 요청 (REST API 등...)
  3. 라이브러리 사용 (D3, Video.js 등...)
  4. setInterval을 통한 반복작업, setTimeout을 통한 작업예약
- **언마운트**
  1. setInterval, setTimeout을 사용해 등록된 작업들 clear 하기  
     (clearInterval, clearTimeout)
  2. 라이브러리 인스턴스 제거

<br />

## **useMemo**

`useMemo`에서 memo는 memoized를 의미하며, 성능 최적화를 위해 이전 계산값을 재사용하게 해주는 Hook이다.
첫번째 파라미터로 어떻게 연산할지 정의하는 함수가,  
두번째 파라미터로 deps 배열을 받는다.

- 배열의 내용이 바뀌면 등록한 함수를 호출해서 값을 연산.
- 내용이 바뀌지 않았다면 이전에 연산한 값을 재사용.

<br />

## **useCallback**

`useMemo`를 기반으로 만들어진 Hook.

`useMemo`는 특정 결과값을 재사용하고 싶을 때 사용하지만,  
`useCallback`은 리렌더링될 때 특정 함수를 새로 만들지않고 재사용하고 싶을 때 사용.
함수 내에서 사용하는 `상태/props`가 있다면 꼭 deps 배열에 포함시켜야하며, 넣지않으면 최신 값의 참조를 보장하지 못한다.

또한 useCallback만으로는 눈에 띄는 최적화는 없고 컴포넌트 렌더링 최적화 작업 (React.memo)을 함께 해줘야한다.

> ### **React.memo**
>
> 컴포넌트의 props가 바뀌지 않았다면 리렌더링을 방지하는 성능 최적화를 위한 함수  
> 이 함수 사용 시 컴포넌트에서 필요한 상황에서만 리렌더링하도록 설정이 가능해진다.  
> 또한 두번째 파라미터에 `propsAreEqual` 함수를 사용해 특정값들만 비교하는 작업도 가능하다.
>
> **※** 렌더링 최적화를 하지 않을 컴포넌트에서의 사용은 불필요한 props만 비교하는 것이므로  
> 실제 렌더링을 방지할 수 있는 경우에만 사용해야한다.

리액트 개발 시 `useCallback`, `useeMemo`, `React.memo`는 컴포넌트 성능을 실제로 개선할 수 있는 상황에서만 사용하는걸 권장.  
예를 들어, 특정 컴포넌트에서 재사용한다고 해서 리렌더링을 막을 수 없는 경우에는 사용 X.

<br />

## **useReducer**

현재 상태와 액션 객체를 파라미터로 받아와서 새로운 상태를 반환해주는 함수로 상태관리에 있어 `useState`를 제외한 또 다른 방법이다.  
`useReducer` 사용 시 컴포넌트의 상태 업데이트 로직을 컴포넌트에서 분리시킬 수 있다.

- 컴포넌트 바깥에 작성하거나 다른 파일에 작성 후 불러 올 수도 있음.

**사용예시**

```js
// Reducer 함수
function reducer(state, action) {
  switch (action.type) {
    case "action.type1":
      // 상태 변화 로직
      return {
        ...state,
        /* 컴포넌트의 새로운 상태 */
      };
    case "action.type2":
    default:
      return state;
  }
}

const [state, dispatch] = useReducer(reducer, initialState);
// 첫번째 파라미터: reducer 함수 | 두번째 파라미터: 초기 상태
// state: 컴포넌트에서 사용하는 상태
// dispatch: 액션을 발생시키는 함수
```

- action
  - 업데이트를 위한 정보를 가지고, 주로 `type` 값을 지닌 객체로 사용된다.
  - 형태의 규칙은 없으나 `type` 값을 대문자와 `_`로 구성하는 관습이 있다.

```js
// input 값 변경하는 액션
{
  type: 'CHANGE_INPUT',
  key: 'nickname',
  value: 'DaengKim'
}
```

<br />

## **참고**

---

[벨로퍼트와 함께하는 모던 리액트 >> useRef 로 컴포넌트 안의 변수 만들기](https://react.vlpt.us/basic/12-variable-with-useRef.html)  
[벨로퍼트와 함께하는 모던 리액트 >> useEffect를 사용하여 마운트/언마운트/업데이트시 할 작업 설정하기](https://react.vlpt.us/basic/16-useEffect.html)
[벨로퍼트와 함께하는 모던 리액트 >> useMemo 를 사용하여 연산한 값 재사용하기](https://react.vlpt.us/basic/17-useMemo.html)
[벨로퍼트와 함께하는 모던 리액트 >> useCallback 을 사용하여 함수 재사용하기](https://react.vlpt.us/basic/18-useCallback.html)
[벨로퍼트와 함께하는 모던 리액트 >> React.memo 를 사용한 컴포넌트 리렌더링 방지](https://react.vlpt.us/basic/19-React.memo.html)
[벨로퍼트와 함께하는 모던 리액트 >> useReducer 를 사용하여 상태 업데이트 로직 분리하기](https://react.vlpt.us/basic/20-useReducer.html)
