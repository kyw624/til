# 테스트 도구 비교 (Jest, RTL, Cypress)

## 요약

주로 아래와 같이 묶어서 또는 단독으로 사용하는듯 하다.

- **`Jest + RTL`**: 컴포넌트 및 단위 테스트에 적합.
- **`Cypress`**: E2E 테스트에 적합.

## 1. Jest

> 페이스북에서 개발한 자바스크립트 테스팅 라이브러리. (CRA 내장)

이전까지는 여러 테스팅 라이브러리를 조합해 사용했었는데,  
Jest는 설치하면 Test Runner, Test Matcher, Test Mock 프레임워크까지 제공해줘서  
사용성이 좋아 대세라고 할 수 있다.

### 기본 사용법

#### 설치

```
npm i -D jest
```

설치 후 `package.json`의 test 스크립트를 "jest"를 추가해 `npm test` 로 실행할 수 있다.

> 특정 파일만을 테스트하려면 `npm test <파일명>`

```json
{
  "scripts": {
    "test": "jest"
  }
}
```

#### 문법

우선 test 파일을 만든다. `(테스트 대상)파일명.test.js`

```js
/* 예시
describe('그룹 테스트 설명 문자열', () => {
   const a = 1, b = 2; // 테스트에 사용할 일회용 가짜 변수 선언

   test('개별 테스트 설명 문자열', () => {
      expect(검증대상).toXxx(기대결과);
   });
});
*/

describe('계산 테스트', () => {
  const a = 1,
    b = 2;

  test('a + b는 3이다.', () => {
    expect(a + b).toEqual(3);
  });
});
```

- **`describe`**: 테스트 그룹을 묶어주는 역할
  그안의 콜백함수 내에 테스트에 쓰일 변수,객체들을 선언하여 일회용으로 사용 할 수 있다.

- **`to~~`**: Test Mathcher라고 하는데, 위에서 사용된 toEqual() 함수는 값을 비교할때 사용한다.
  > expect(a + b).toEqual(3) // a + b의 값이 3이라면 true

<br>

## 2. React-Testing Library (RTL)

> DOM을 테스트하기 위한 도구로, React 컴포넌트를 테스트 하기 위해 만들어진 도구.

리액트 테스팅에서 Jest와 대립 관계가 아닌 함께 사용되는 상호 보완 관계의 도구이다.

> Jest로는 기능 테스트를 진행하고,  
> 리액트 컴포넌트의 렌더링 및 테스트를 위해서는 RTL이 필요하기 때문.

### 테스트 코드 예시

```js
// Counter.js
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  const decrement = () => setCount(count - 1);

  const increment = () => setCount(count + 1);

  return (
    <>
      <div>{count}</div>
      <button onClick={decrement}>-</button>
      <button onClick={increment}>+</button>
    </>
  );
};

export default Counter;
```

```js
// Counter.test.js
import { render } from '@testing-library/react';

import Counter from './Counter';

describe('Counter test', () => {
  it('should render Counter', () => {
    render(<Counter />);
  });
});
```

- Test Runner는 테스트 실행 시 `*.test.js` 파일들을 자동으로 탐색하며 테스트를 진행함.

- **`describe`**: 같은 맥락의 테스트들을 그룹화함.

- **`it`**: 개별 테스트를 수행함. (test 메서드와 동일)

- **`render`**: 테스트를 위해 특정 컴포넌트를 JSDOM에 렌더링함.

### 테스트 흐름

기본적으로 단위별로 끊어보면 `컴포넌트를 띄우고 → 특정 액션을 발생시킨 후 → 결과를 확인` 하는 패턴으로 이루어지는데,  
해당 패턴들을 위해 여러 API들이 활용된다.

> (Query, Action, Assertion, Asynchronous Test, ... 등등)

<br>

## 3. Cypress

> 단위, 통합, E2E 모든 테스트가 가능하지만 주로 E2E 테스트에 쓰이는 자바스크립트 테스트 프레임워크.

실제 브라우저를 띄우고 테스트를 진행하는 방식으로,  
설치 직후 E2E 테스트 코드 작성은 바로 가능하지만  
컴포넌트 테스트를 위해서는 추가적인 모듈 등의 세팅이 필요하다.

### 특징

- 완벽한 E2E Tesing의 경험.

- Cypress Test Runner를 설치하고 로컬에서 테스트를 작성. (Test Runner로 Mocha 사용)

- CI 테스트를 구축 및 결과를 기록.

- 테스트가 실행될 때 스냅샷을 만들어 각 단계에서 정확히 어떤 일이 발생했는지 확인 가능.

- Chrome DevTools과 같은 익숙한 도구에서 빠르게 직접 디버깅 가능.

- 테스트를 변경할 때마다 자동으로 다시 로드.
