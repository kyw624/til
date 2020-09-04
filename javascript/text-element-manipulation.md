# 텍스트 요소 프로퍼티 성능 비교

## **innerHTML vs innerText vs textContent**

<br />

### **innerHTML**

기본적으로 HTML 태그의 삽입이 가능한 프로퍼티이며, 내용을 HTML로 구분 분석하기 때문에 시간이 오래걸린다.  
`XSS (Cross-Site Scripting)` 의 대표적인 취약 사례이기도 하다.

<br />

### **innerText**

사람이 읽을 수 있는 요소만 표시하며, 숨겨진 요소는 무시되고 CSS 스타일을 인식하여 렌더링 된 상태의 모습을 가져온다.  
또 리플로우가 트리거되므로 비용면을 고려하여 가능하다면 피해야한다.

<br />

### **textContent**

다른 프로퍼티들에 비해 가벼워 속도가 가장 빠르다.  
스타일을 포함한 모든 요소를 텍스트 노드로 가져오기때문에 이후에 가공하여 사용이 가능하다.

<br />

## **비교**

```html
<div id="blog test">
  This element is <strong>strong</strong> and has some super fun
  <code>code</code>!
</div>
```

```js
const getValue = document.getElementById("blog-test");

getValue.innerHTML;
// This element is <strong>strong</strong> and has    some super fun <code>code</code>!

getValue.innerText;
// This element is strong and has some super fun code!

getValue.textContent;
// This element is strong and     has some super fun code!
```

<br />

## **결론**

기본적으로 `textContent`를 사용하면서  
HTML Parsing이 필요한 경우에는 `innerHTML`을,  
IE8 환경을 고려한다면 `innerText`를 사용하면 될 것 같다.

<br />

## **참고**

---

[Stack Overflow >> innerText vs innerHTML vs label vs text vs textContent vs outerText](https://stackoverflow.com/questions/24427621/innertext-vs-innerhtml-vs-label-vs-text-vs-textcontent-vs-outertext)  
[Tistory >> innerHTML, innerText, textContent 브라우저별 성능 비교](https://equal-blog.tistory.com/entry/innerHTML-innerText-textContent-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EB%B3%84-%EC%84%B1%EB%8A%A5-%EB%B9%84%EA%B5%90)  
[Medium >> Why textContent is better than innerHTML and innerText?](https://blog.cloudboost.io/why-textcontent-is-better-than-innerhtml-and-innertext-9f8073eb9061)  
[MDN >> Element.innerHTML](https://developer.mozilla.org/ko/docs/Web/API/Element/innerHTML)  
[MDN >> Node.innerText](https://developer.mozilla.org/ko/docs/Web/API/Node/innerText)  
[MDN >> Node.textContent](https://developer.mozilla.org/ko/docs/Web/API/Node/textContent)
