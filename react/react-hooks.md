# React Hooks 정리

## **useRef**

리액트에서 상태 관리 시 상태를 바꾸는 함수를 호출해야만 조회가 가능한데  
useRef로 관리하는 변수는 설정 후 바로 조회가 가능하며, 리렌더링 되지 않는 특징이 있다.

useRef를 사용하는 경우는 다음과 같다.

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

## **참고**

---

[벨로퍼트와 함께하는 모던 리액트 >> useRef 로 컴포넌트 안의 변수 만들기](https://react.vlpt.us/basic/12-variable-with-useRef.html)
