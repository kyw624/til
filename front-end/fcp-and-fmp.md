# FCP & FMP

## **FCP (First Contents Paint)**

사용자가 웹 사이트 탐색 중 화면에 렌더링되는 첫번째 콘텐츠를 의미한다.  
브라우저가 DOM에 정의된 첫번째 콘텐츠를 렌더링하는 시점까지의 시간을 측정.

FCP는 2단계로 구성되는데,

1. _First Paint_

   - 브라우저에서 렌더링이 감지되면 첫번째 페인트가 트리거된다.  
     ex) 배경색 등등...
   - 반드시 사용자의 소비를 위한 콘텐츠나 정보가 아닐 수 있다.

2. _Contentful Paint_

   - DOM의 첫번째 콘텐츠비트가 그려지면 첫번째 콘텐츠 페인트가 트리거된다.  
     ex) DOM에 정의된 텍스트, 이미지 등등...
   - 콘텐츠에 중점을 둔다.

![First Contents Paint Level](https://www.acmethemes.com/blog/wp-content/uploads/2020/01/First-Contentful-Paint-Diagram-e1588261472160.jpg)

<br />

## **FMP (First Meaningful Paint)**

사용자가 찾는 웹 사이트의 콘텐츠가 완전히 로드된 상태이다. (= 페이지의 기본 콘텐츠가 표시되는 것을 느끼는 시간)

FMP 렌더링 후에는 사용자에게 의미있는 정보를 제공한다.  
의미있는 정보는 사이트마다 다르지만 일반적으로 hero element로 알려진 중요한 콘텐츠의 렌더링을 지정하는 것이다.

> ※ hero element: 사용자에게 더 의미있을 수 있는 이미지 및 제목 등이 포함된 요소.
>
> ex) hero element 예시  
> 블로그 : 블로그 게시물 등...  
> 소셜 사이트: 타임라인, 사용자 프로필 등...

![First Meaningful Paint](https://www.acmethemes.com/blog/wp-content/uploads/2020/01/First-Meaningful-paint-e1588261514486.jpg)

<br />

## **FCP와 FMP를 빠르게 하는 방법**

1. 페이지에 종속되는 외부 CSS, 스크립트의 렌더링 차단 수 줄이기.

   - 렌더링 차단 리소스를 제거하여 성능을 높일 수 있는 첫번째 방법이다.
   - 렌더 블록은 DOM을 렌더링할 때 얻는 모든 것을 뜻한다.

2. HTTP 캐싱 사용.

   - HTTP 캐싱은 OS가 모든 웹 사이트의 페인트 시간을 줄이는 또 다른 방법이다.
   - 누군가가 웹사이트 방문 시 사이트를 표시하기 위한 자원들을 브라우저에서 검색해야하는데 이는 페이지 로드 속도에 큰 차이를 만들 수 있다.

3. 텍스트 기반 자산의 최소화 및 압축

   - 필요하지 않은 자원의 최소화와, 파일을 제거할 수 없을 경우에는 파일 크기를 가능한 줄인다.
   - 파일 크기의 최소화/압축 방법
     1. Minification (축소): 브라우저에서 리소스 처리에 영향을 주지않는 불필요한 데이터나 중복 데이터를 제거하는 프로세스.  
        ex) 사용하지 않는 코드 제거, 짧은 변수 및 함수명 사용 등...
     2. Compression (압축): 텍스트 파일에서 패턴과 중복을 감지하는 프로세스의 경우에 더 효율적으로 인코딩.

4. JavaScript 최적화
   - JavaScript 파일은 웹사이트의 다른 파일에 비해 가장 무거운 파일로, 다음과 같은 방법으로 최적화할 수 있다.
     1. Tree Shaking: 사용하지 않는 코드를 제거하는 방식.
     2. Code Splitting: 당장 필요하지 않은 코드들을 따로 분리시켜서 나중에 필요할 때 불러와서 사용하는 식의 코드 분할 방식.

<br />

## **결론**

웹 사이트 로딩 속도에 영향을 미치는 것은 다양한 요인이 있지만  
최상의 사용자 경험을 위해서는 FCP, FMP 모두 1초 미만이어야 한다.

페이지 속도 분석 사이트: [Google PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)

<br />

## **참고**

[What is First Contentful Paint & First Meaningful Paint](https://www.acmethemes.com/blog/first-contentful-paint-and-first-meaningful-paint/)
