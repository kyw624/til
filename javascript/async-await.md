# async / await

Promise Chaining이 계속되면 코드의 가독성이 떨어진다.  
이 때 간결하고, 간편하게 Promise를 사용할 수 있게해주는 API이다.

그러나 항상 Promise를 대체하는 것이 아닌, 오히려 Promise를 사용해야 더 깔끔한 경우도 있으니 주의!

<br />

## **async 사용 예시**

**1. Promise 사용**

```js
function fetchUser() {
  return new Promise((resolve, reject) => {
    resolve('result');
  });
}

const user = fetchUser();
user.then(console.log);
```

**2. async로 변환**

async 사용 시 함수의 코드 블록이 자동으로 Promise로 적용된다.

```js
async function fetchUser() {
  return 'result';
}

const user = fetchUser();
user.then(console.log);
```

<br />

## **await 사용 예시**

**1. Promise 사용**

```js
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

function getApple() {
  return delay(1000).then(() => `🍎`);
}

function getBanana() {
  return delay(1000).then(() => `🍌`);
}

function pickFruits() {
  return getApple().then((apple) => {
    return getBanana().then((banana) => `${apple} + ${banana}`);
  });
}

pickFruits().then((result) => console.log(result));

const user = fetchUser();
user.then(console.log);
```

**2. async로 변환**

- await 사용 시 해당 구문의 실행을 기다린다.

```js
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function getApple() {
  await delay(1000);
  // throw new Error(`Error: apple`); // Error 발생시키는 구문
  return `🍎`;
}

async function getBanana() {
  await delay(2000);
  // throw new Error(`Error: banana`);
  return `🍌`;
}

// (1) 순차적 처리
// getApple과 getBanana는 각각 독립적인 함수지만 순차적으로 실행됨. (비효율적)
async function pickFruits() {
  const apple = await getApple();
  const banana = await getBanana(); // getApple() 호출을 기다린 후 실행됨.

  return `${apple} + ${banana}`;
}

// (2) 병렬 처리
// 위의 코드를 개선 (Promise의 병렬 처리)
async function pickFruits() {
  const applePromise = getApple();
  const bananaPromise = getBanana();
  const apple = await applePromise;
  const banana = await bananaPromise;

  return `${apple} + ${banana}`;
}

// (3)
// 독립된 함수를 호출할 경우 Promise.all을 사용하면 더 효율적!
function pickAllFruits() {
  return Promise.all([getApple(), getBanana()]).then((fruits) =>
    fruits.join(' + ')
  );
}

pickFruits().then((result) => console.log(result));

// Promise.race: 가장 먼저 완료 된 Promise만을 return.
function pickOnlyOne() {
  // delay가 1000으로 더 빨리 처리 된 apple만 return한다.
  return Promise.race([getApple(), getBanana()]);
}

pickOnlyOne().then(console.log);
```

<br />

## **참고**

---

[유튜브 >> 드림코딩 by 엘리](https://youtu.be/aoQSOZfz3vQ)
