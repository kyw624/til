# 실행 컨텍스트

실행 컨텍스트는 자바스크립트가 동작하는 원리를 담고있는 핵심 개념이다.

<br />

## **컨텍스트의 4가지 원칙**

1. 먼저 전역 컨텍스트 하나를 생성 후, 함수 호출 시마다 컨텍스트가 생긴다.
2. 컨텍스트 생성 시 컨텍스트 안에 **변수객체(arguments, variable), scope chain, this**가 생성된다.
3. 컨텍스트 생성 후 함수가 실행되는데, 사용되는 변수들은 변수 객체 안에서 값을 찾고, 없다면 스코프 체인을 따라 올라가며 찾는다.
4. 함수 실행이 마무리되면 해당 컨텍스트는 사라진다. (클로저 제외)  
   페이지가 종료되면 전역 컨텍스트가 사라진다.

   ```js
   var name = 'zero'; // (1)변수 선언 (6)변수 대입
   function wow(word) {
     // (2)변수 선언 (3)변수 대입
     console.log(word + ' ' + name); // (11)
   }
   function say() {
     // (4)변수 선언 (5)변수 대입
     var name = 'nero'; // (8)
     console.log(name); // (9)
     wow('hello'); // (10)
   }
   say(); // (7)
   // nero
   // hello zero
   ```

<br />

### **전역 컨텍스트**

전역 컨텍스트가 생성된 후 두 번째 원칙에 따라 변수객체, scope chain, this가 들어온다.  
전역 컨텍스트는 **arguments가** 없고, **variable은** 해당 스코프의 변수들이다.  
위의 예시에서는 `name`, `wow`, `say`가 있다.

**scope chain**은 자기 자신인 전역 변수객체이다.  
**this**는 따로 설정되어 있지 않으면 window다.

- **new를 호출**하거나, **다른 this값을 bind**하면 this를 바꿀 수 있다.

이를 객체 형식으로 표현해보면 다음과 같다.

```js
'전역 컨텍스트': {
  변수객체: {
    arguments: null,
    variable: ['name', 'wow', 'say'],
  },
  scopeChain: ['전역 변수객체'],
  this: window,
}
```

함수 wow랑 say는 **호이스팅** 때문에 선언과 동시에 대입이 된다.

### **함수 컨텍스트**

그 후 (7)번에서 `say();` 하는 순간 새로운 컨텍스트인 **say 함수 컨텍스트**가 생성된다.

**arguments**는 없고, **variable**은 name,  
**scope chain**은 say 변수객체, 상위의 전역 변수객체이다.  
**this**는 따로 설정해주지 않았기때문에 window이다.

```js
'say 컨텍스트': {
  변수객체: {
    arguments: null,
    variable: ['name'], // 초기화 후 [{ name: 'nero' }]가 됨
  },
  scopeChain: ['say 변수객체', '전역 변수객체'],
  this: window,
}
```

이제 컨텍스트가 생성되고 함수들이 실행되는데 say 함수는 아직 종료된 게 아니다.  
각 컨텍스트마다 **scope chain**을 따라 값을 찾아가면서  
wow 함수 종료 후 wow 컨텍스트가 사라지고,  
say 함수의 실행이 마무리되면서 say 컨텍스트가 사라지고,  
마지막으로 전역 컨텍스트가 사라진다.

<br />

(10)번에서 wow 함수를 호출돼 wow 컨텍스트도 생성된다.

**arguments**는 word='hello', **variable**은 없고,  
 **scope chain**은 wow 스코프, 전역 스코프이다.  
 **this**는 마찬가지로 window이다.

```js
'wow 컨텍스트': {
  변수객체: {
    arguments: [{ word : 'hello' }],
    variable: null,
  },
  scopeChain: ['wow 변수객체', '전역 변수객체'],
  this: window,
}
```

<br />

## **참고**

---

[ZeroCho's Blog >> 실행 컨텍스트](https://www.zerocho.com/category/JavaScript/post/5741d96d094da4986bc950a0)
