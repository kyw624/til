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
> ### **안넣어주면?** useEffect 함수가 실행될 때 최신 `상태/props` 를 가리키지 않는다.

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

## **참고**

---

[벨로퍼트와 함께하는 모던 리액트 >> useRef 로 컴포넌트 안의 변수 만들기](https://react.vlpt.us/basic/12-variable-with-useRef.html)  
[벨로퍼트와 함께하는 모던 리액트 >> useEffect를 사용하여 마운트/언마운트/업데이트시 할 작업 설정하기](https://react.vlpt.us/basic/16-useEffect.html)
