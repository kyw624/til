# Promise 정리

비동기 처리를 위해 사용하는 객체이다.  
간단한 구문은 콜백으로 처리해도되지만 코드가 길거나 복잡할 때 콜백 지옥에 빠지는 것을 방지할 수 있으며, 간편하고 깔끔한 코드 작성이 가능해진다.

<br />

## **Promise의 상태**

Promise는 3가지의 상태를 가진다.

1. Pending (준비)
2. Fulfilled (완료)
3. Rejected (실패)

<br />

## **Promise 사용 예시**

**1. Producer (생산자)**  
 Promise가 생성되면 따로 호출하지않아도 곧 바로 실행되는 특징이 있다.

```js
const promise = new Promise((resolve, reject) => {
  console.log('doing something...');
  setTimeout(() => {
    resolve('DaengKim'); // 성공 시
    reject(new Error('no network')); // 실패 시
  }, 2000);
});
```

**2. Consumer (소비자)**  
 생성된 Promise는 여러가지로 활용된다.

- then: 성공 시 호출
- catch: 실패 시 호출
- finally: 성공이던 실패던 마지막에 호출

```js
promise
  .then((value) => {
    console.log(value);
  })
  .catch((error) => {
    console.log(error);
  })
  .finally(() => {
    console.log('finally');
  });
```

**3. Promise Chaining**

then으로 계속해서 값을 리턴하며 받아올 수 있다.

```js
const fetchNumber = new Promise((resolve, reject) => {
  setTimeout(() => resolve(1), 1000);
});

fetchNumber
  .then((num) => num * 2) // return 2
  .then((num) => num * 3) // return 6
  .then((num) => {
    return new Promise((resolve, reject) => {
      setTimeout(() => resolve(num - 1), 1000);
    });
  })
  .then((num) => console.log(num)); // 5
```

**4. Error Handling**

```js
const getHen = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve('🐔'), 1000);
  });
const getEgg = (hen) =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${hen} => 🥚`), 1000);
  });
const cook = (egg) =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${egg} => 🍳`), 1000);
  });

// 호출
// (1)
getHen()
  .then((hen) => getEgg(hen))
  .then((egg) => cook(egg))
  .then((meal) => console.log(meal));

// (2) 받아오는 값을 바로 다른 함수의 파라미터로 전달하는 경우 생략 가능.
// 위의 코드와 같다.
getHen().then(getEgg).then(cook).then(console.log);
```

<br />

## **참고**

---

[유튜브 >> 드림코딩 by 엘리](https://youtu.be/JB_yU6Oe2eE)
