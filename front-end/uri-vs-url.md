# URI vs URL

## **개요**

둘의 차이점을 확실하게 몰라서 정리한 문서.

- URI, URL 모두 인터넷 상의 특정 자원에 대한 위치를 나타내는 주소이다.
- URI에 URL, URN이 포함되어 있다. 따라서 모든 URL은 URI이다 (TRUE)

<br />

## **URI (Uniform Resource Identifier)**

인터넷 상의 특정 자원을 나타내는 유일한 주소.

**예시**

1. rewrite 기술 이용한 의미있는 식별자
   - http://test.com/index: URI (index.html로 rewrite)
   - http://test.com/index.html: URI, URL

<br />

2. REST 서비스 (URL로 서비스 실행)  
   http://service.com/tv/turn/on

<br />

3. URL과의 구분
   - http://test.com/test.pdf?docid=111, http://test.com/test.pdf?docid=222
     - URI지만 URL은 아니다.
     - docid 값에 따라 결과가 달라지므로 식별자.
     - 같은 URL
     - 다른 URI

<br />

## **URL (Uniform Resource Locator)**

**예시**

- test.com 웹서버의 work 디렉토리에 있는 sample.pdf를 요청하는 URL  
  http://test.com/work/sample.pdf

<br />

## **참고**

---

[Blog >> URI, URL의 차이점](http://blog.naver.com/PostView.nhn?blogId=rnjsrldnd123&logNo=221501904861&parentCategoryNo=20&categoryNo=&viewDate=&isShowPopularPosts=true&from=search)  
[Blog >> URL과 URI의 의미와 차이점 (Difference between URL & URI)](https://blog.lael.be/post/61)
