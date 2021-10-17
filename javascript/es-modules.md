# ES Modules

ES6에 도입된 모듈 시스템으로 import, export문을 통해 분리된 파일들끼리 서로 접근할 수 있게 해준다.  
Export 방식은 Named Export, Default Export 두 종류가 있다.

<br />

## **Named Export**

한꺼번에 많은 것들을 export, import 하고 싶을 때 사용하는 방식으로,  
기본적으로 import 할 때와 export 할 때의 이름이 동일해야 하지만 변경해서 사용할 수 있다.  
하나의 파일에 여러 개 존재할 수 있다.

```js
// 1. export name과 import name이 동일
// math.js
export const plus = (a, b) => a + b;
export const minus = (a, b) => a - b;
export const divide = (a, b) => a / b;
// export { plus, minus, divide } -> 미리 선언한 식별자 내보내기
// export { plus as add, minus, divide } -> 내보내기 전 이름을 변경

// main.js
import { plus, minus, divide } from './math';

plus(2, 2);
.
.
.
// 2. import name을 변경해서 사용
// math.js
export const plus = (a, b) => a + b;
export const minus = (a, b) => a - b;
export const divide = (a, b) => a / b;

// main.js
import { plus as add } from './math';

add(2, 2);
```

<br />

## **Default Export**

한꺼번에 많은 것들을 export, import 하고 싶을 때 사용하는 방식으로,  
사용자가 원하는 이름으로 import 할 수 있다.  
각 파일마다 하나만 존재할 수 있다.

```js
// math.js
const plus = (a, b) => a + b;
const minus = (a, b) => a - b;
const divide = (a, b) => a / b;
export default { plus, minus, divide };

// main.js
import math from './math';

math.plus(2, 2);
```

<br />

## **여러 Import 방식**

```js
// 1. named export와 default export가 섞여 있는 경우
// db.js
const connectToDB = () => {};
export const getUrl = () => {};
export default connectToDB;

// main.js
import connect, { getUrl } from './db';
.
.
.
// 2. Star(*) Import
// default export가 없고, 모든 export들을 import하고 싶을 때 원하는 이름으로 import 가능.
// math.js
export const plus = (a, b) => a + b;
export const minus = (a, b) => a - b;
export const divide = (a, b) => a / b;

// main.js
import * as myMath from './math';
myMath.plus(2, 2);
```

<br />

## **로딩을 조금 더 빠르게**

1. 필요한 것만 import  
   default export 또는 모든걸 한번에 import 할 필요 없이 필요한 것만 import하는 방법.

```js
// math.js
export const plus = (a, b) => a + b;
export const minus = (a, b) => a - b;
export const divide = (a, b) => a / b;

// main.js
import { plus, minus, divide } from './math'; // Bad
import { plus } from './math'; // Good
```

2. Dynamic Import 활용  
   보통의 import 방식은 파일의 위에서부터 모든걸 import 하기때문에 속도가 느릴 수 있는데,  
   실제 사용할 모듈만 그때그때 import 하는 기법.

```js
// 요청이 있을 때마다 모듈을 로딩
function doMath() {
  import('./math').then((math) => math.plus(2, 2));
}
btn.addEventListener('click', doMath);

// await / async 활용
async function doMath() {
  const math = await import('./math');
  math.plus(2, 2);
}
btn.addEventListener('click', doMath);
```

---

[노마드 코더 Youtube >> Export Default 과연 제대로 알고계십니까?](https://www.youtube.com/watch?v=WUirHxOBXL4)  
[MDN >> export](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/export)  
[MDN >> import](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/import)
