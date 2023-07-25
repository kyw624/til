# Event Web API의 속성들

<br>

## **Event.isTrusted**

사용자 액션에 의해 생성된 이벤트에서는 `true`,  
스크립트에서 생성/수정했거나 `EventTarget.dispatchEvent()`로 발송한 이벤트의 경우 `false`

```js
if (e.isTrusted) {
  // 신뢰할 수 있는 이벤트
} else {
  // 신뢰 불가능한 이벤트
}
```

<br>

## **참고**

---

[MDN >> Event >> isTrusted](https://developer.mozilla.org/ko/docs/Web/API/Event/isTrusted)
