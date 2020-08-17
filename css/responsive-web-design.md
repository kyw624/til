# Media Query 이용한 반응형 레이아웃

## **개요**

반응형 디자인은 `width` 기준으로 `breakpoint`를 정의해 사용한다.

![breakpoint](https://poiemaweb.com/img/media-query-breakpoints.jpg)

2가지 디자인 방식이 있어 `CSS 적용 우선 순위`에 따라 `Media Query`의 기술 순서를 주의해야한다.

1. `Mobile First` (모바일 우선)

2. `Non Mobile First` (모바일 우선 X)

> **CSS 적용 우선순위**  
> 나중에 선언된 스타일이 우선 적용됨.

<br />

### **Mobile First (모바일 우선)**

해상도가 작은 순서로 기술.

```css
/*==========  Mobile First Method  ==========*/
/* All Device */

/* Custom, iPhone Retina : 320px ~ */
@media only screen and (min-width: 320px) {
}

/* Extra Small Devices, Phones : 480px ~ */
@media only screen and (min-width: 480px) {
}

/* Small Devices, Tablets : 768px ~ */
@media only screen and (min-width: 768px) {
}

/* Medium Devices, Desktops : 992px ~ */
@media only screen and (min-width: 992px) {
}

/* Large Devices, Wide Screens : 1200px ~ */
@media only screen and (min-width: 1200px) {
}
```

### **Non Mobile First (모바일 우선 X)**

해상도가 큰 순서로 기술.

```css
/*==========  Non-Mobile First Method  ==========*/
/* All Device */

/* Large Devices, Wide Screens : ~ 1200px */
@media only screen and (max-width: 1200px) {
}

/* Medium Devices, Desktops : ~ 992px */
@media only screen and (max-width: 992px) {
}

/* Small Devices, Tablets : ~ 768px */
@media only screen and (max-width: 768px) {
}

/* Extra Small Devices, Phones : ~ 480px */
@media only screen and (max-width: 480px) {
}

/* Custom, iPhone Retina : ~ 320px */
@media only screen and (max-width: 320px) {
}
```

<br />

## **참고**

---

[Poiemaweb >> CSS >> 반응형 레이아웃](https://poiemaweb.com/css3-responsive-web-design)
