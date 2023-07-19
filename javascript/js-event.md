# 이벤트 캡쳐 & 버블링

브라우저의 이벤트 감지 방식에는 2가지가 있다. (캡쳐, 버블링)

<br>

## **이벤트 버블링 (Event Bubbling)**

이벤트 버블링은 아래의 그림처럼  
특정 요소에서 이벤트가 발생했을 때 해당 이벤트가 상위의 요소들로 전달되어 가는 특성을 의미한다.

![img](https://joshua1988.github.io/images/posts/web/javascript/event/event-bubble.png)

### 예시

- 이벤트가 부모 요소까지 전파되어  
  가장 하위 요소인 `three` 만 클릭해도 콘솔에 순서대로 찍히게 된다.

  > // console  
  > three  
  > two  
  > one

- `two` 클릭 시 two -> one 순으로 이벤트 동작
  > // console  
  > two  
  > one

```html
<body>
  <div class="one">
    <div class="two">
      <div class="three"></div>
    </div>
  </div>
</body>
```

```js
const divs = document.querySelectorAll('div');

divs.forEach(function (div) {
  div.addEventListener('click', logEvent);
});

function logEvent(event) {
  console.log(event.currentTarget.className);
}
```

<br>

## **이벤트 캡쳐 (Event Capture)**

이벤트 캡쳐는 버블링과 반대 방향으로 진행되는데  
이벤트 발생 시 최상위 요소에서 해당 요소를 찾아내려가는 방식이다.

![img](https://joshua1988.github.io/images/posts/web/javascript/event/event-capture.png)

### 예시

- 가장 하위 요소인 `three` 를 클릭했지만  
  최상위 요소에서부터 이벤트 발생 지점을 찾아가므로  
  상위 요소들부터 순서대로 출력됨.

  > // console  
  > one  
  > two  
  > three

- `two` 클릭 시 one -> two 순으로 이벤트 동작
  > // console  
  > one  
  > two

```html
<body>
  <div class="one">
    <div class="two">
      <div class="three"></div>
    </div>
  </div>
</body>
```

```js
const divs = document.querySelectorAll('div');

divs.forEach(function (div) {
  div.addEventListener('click', logEvent, {
    capture: true, // default 값은 false입니다.
  });
});

function logEvent(event) {
  console.log(event.currentTarget.className);
}
```

<br>

## **event.stopPropagation()**

이 API는 해당 이벤트가 전파되는 것을 막는 역할을 한다.

- 이벤트 버블링의 경우 클릭한 요소의 이벤트만 발생시키고,  
  상위 요소로의 전파를 방해.

```js
function logEvent(event) {
  event.stopPropagation();
}
```

<br>

## **이벤트 위임 (Event Delegation)**

앞에서 살펴본 버블링, 캡쳐는 `이벤트 위임`을 위한 선수 지식이라 해도 무방하다.

이벤트 위임은 실제 바닐라 JS로 웹 앱을 구현할 때 자주 사용하게 되는 코딩 패턴인데,  
이벤트 위임을 한 문장으로 요약하면 다음과 같다.

- `하위 요소에 각각 이벤트를 붙이지 않고, 상위 요소에서 하위 요소의 이벤트들을 제어하는 방식`

### 예시

아래와 같은 2개의 할 일 목록이 있는 코드를 예로 들면,

```html
<h1>오늘의 할 일</h1>
<ul class="itemList">
  <li>
    <input type="checkbox" id="item1" />
    <label for="item1">이벤트 버블링 학습</label>
  </li>
  <li>
    <input type="checkbox" id="item2" />
    <label for="item2">이벤트 캡쳐 학습</label>
  </li>
</ul>
```

```js
const inputs = document.querySelectorAll('input');

inputs.forEach(function (input) {
  input.addEventListener('click', function (event) {
    alert('clicked');
  });
});
```

해당 2개의 리스트에는 정상적으로 클릭 이벤트가 작동하지만  
다른 리스트들이 추가되면 해당 리스트들은 이벤트가 작동하지 않는데,  
아이템이 추가될 때마다 리스너를 일일히 달아주는 것은 비효율적.

**이를 해결할 방법이 바로 `이벤트 위임`이다.**

**다음은 위의 js 코드를 수정한 코드이다.**

```js
// const inputs = document.querySelectorAll('input');

// inputs.forEach(function(input) {
// 	input.addEventListener('click', function() {
// 		alert('clicked');
// 	});
// });

const itemList = document.querySelector('.itemList');

itemList.addEventListener('click', function (event) {
  alert('clicked');
});

// 새 리스트 아이템을 추가하는 코드
/**
  ...
 */
```

이벤트 리스너를 상위 요소인 ul 태그에 달아놓고 하위 요소에서 발생한 이벤트를 감지하면 새로운 아이템이 추가되도 이벤트가 정상적으로 작동한다.

<br>

## **참고**

---

[캡틴판교님 블로그 >> 이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
