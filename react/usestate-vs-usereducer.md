# useState vs useReducer

리액트에서 상태 관리 방법인 `useState`, `useReducer` 중 어떤 것을 사용해야할까?  
답은 정해져있지 않고, 상황에 맞게 사용하는 것이 좋다고한다.

1. 컴포넌트에서 관리하는 값이 딱 하나면서 그 값이 단순한 값이라면 `useState`가 편할 수 있다.
2. 관리하는 값이 여러개라 상태의 구조가 복잡해지면 `useReducer`로 관리하는게 편할 수 있다.

예를 들어 `setter`를 한 함수에서 여러번 사용하는 일이 발생하면 `useReducer`에 대한 고민 후  
편해질 것 같으면 사용하고 아니면 `useState`로 그대로 유지하면 될 것 같다.

<br />

## **참고**

---

[벨로퍼트와 함께하는 모던 리액트 >> useReducer 를 사용하여 상태 업데이트 로직 분리하기](https://react.vlpt.us/basic/20-useReducer.html)
