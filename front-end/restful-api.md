# RESTful API란?

## **개요**

일반적으로 웹이나 앱에서 개발할 때는 HTTP 프로토콜을 사용하여 API를 만들게된다.  
이 때 사용하는 것이 REST (**RE**presentational **S**tate **T**ransfer) 아키텍처이다.  
이런 REST의 기본 원칙을 잘 지킨 서비스를 `RESTful`이라고 표현한다.

<br />

## **REST API의 중심 규칙**

1. **URI는 정보의 자원을 표현해야 한다.**

   - 리소스명은 동사보다는 명사를 사용.
   - URI는 자원 표현을 중점으로.

   ```
   # bad
   GET /getTodos/1
   GET /todos/show/1

   # good
   GET /todos/1
   ```

2. **자원에 대한 행위는 HTTP 메소드(GET, POST, PUT, DELETE 등)로 표현한다.**

   ```
   # bad
   GET /todos/delete/1

   # good
   DELETE /todos/1
   ```

<br />

## **HTTP Method**

일반적으로 5가지의 Method를 사용하여 `CRUD`를 구현한다.

| Method | 역할               | 멱등성 보장 유무 |
| ------ | ------------------ | :--------------: |
| GET    | 리소스를 조회      |        O         |
| POST   | 리소스를 생성      |        X         |
| PUT    | 리소스 전체를 교체 |        O         |
| PATCH  | 리소스 일부를 수정 |        X         |
| DELETE | 리소스를 삭제      |        O         |

<br />

**PUT PATCH의 차이**

기본적으로

- `PUT`: 수정하지 않을 사항까지도 리소스를 요청 바디에 모두 보내야한다.
- `PATCH`: 수정하고 싶은 사항만 보내면되기 때문에 효율적이다.

가장 중요한 차이점은 `PUT`은 반드시 멱등성이 보장되지만 `PATCH`는 구현 방식에 따라 보장될 수도, 안될 수도 있다.

> **멱등성?**  
> 특정 대상에 같은 연산을 여러 번 적용해도 결과가 달라지지 않는 성질

<br />

## **REST API의 구성**

자원, 행위, 표현의 3요소로 구성된다.

| 구성 요소              | 내용             | 표현 방법            |
| ---------------------- | ---------------- | -------------------- |
| 자원 (Resource)        | 자원             | HTTP URI             |
| 행위 (Verb)            | 자원에 대한 행위 | HTTP Method          |
| 표현 (Representations) | 행위의 내용      | HTTP Message PayLoad |

<br />

## **참고**

---

[Poiemaweb](https://poiemaweb.com/js-rest-api)  
[Blog >> 프론트엔드와 백엔드가 소통하는 엔드포인트, RESTful API](https://evan-moon.github.io/2020/04/07/about-restful-api/#http-%EB%A9%94%EC%86%8C%EB%93%9C%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EC%96%B4%EB%96%A4-%ED%96%89%EC%9C%84%EC%9D%B8%EC%A7%80-%ED%91%9C%ED%98%84%ED%95%98%EC%9E%90)
