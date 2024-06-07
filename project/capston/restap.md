# RestAP

## REST API

Rest API란 REST를 기반으로 만들어진 API를 의미한다.

***

### REST란?

REST(Representational State Transfer)의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미한다.

즉 REST란

HTTP URL(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고 HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해 해당 자원(URL)에 대한 CRUD Operation을 적용하는 것을 의미한다.

> **CRUD Operation이란** CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말이다.
>
>
>
> **동작 예시**&#x20;
>
> Create : 데이터 생성(POST)&#x20;
>
> Read : 데이터 조회(GET)&#x20;
>
> Update : 데이터 수정(PUT, PATCH)&#x20;
>
> Delete : 데이터 삭제(DELETE)

### REST 구성 요소

REST는 다음과 같은 3가지로 구성이 되어있다.

1. 자원(Resource) : HTTP URL
2. 자원에 대한 행위(Verb) : HTTP Method
3. 자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load

### REST의 특징

1. Server-Client(서버-클라이언트 구조)
2. Stateless(무상태)
3. Cacheable(캐시 처리 가능)
4. Layered System(계층화)
5. Uniform Interface(인터페이스 일관성)

### REST의 장단점

장점

* HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
* HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해 준다.
* HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
* Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
* REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
* 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
* 서버와 클라이언트의 역할을 명확하게 분리한다.

단점

* 표준이 자체가 존재하지 않아 정의가 필요하다.
* HTTP Method 형태가 제한적이다.
* 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
* 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다.(익스폴로어)

***

## REST API란?

> { REST API } Representational State Transfer API

* API
  * 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진하여, 서로 정보를 교환가능 하도록 하는 것
* REST API의 정의
  * REST 기반으로 서비스 API를 구현한 것
  * OpenAPI(누구나 사용할 수 있는 공개된 API) 는 대부분 REST API를 제공

### REST API의 설계 기본 규칙

* URL는 자원의 정보를 표시해야 한다.
  * resource는 동사보다는 명사를, 대문자보다는 소문자를 사용한다
  * resource의 도큐먼트 이름으로는 단수 명사를 사용해야 한다
  * resource의 컬렉션 이름으로는 복수 명사를 사용해야 한다
  * resource의 스토어 이름으로는 복수 명사를 사용해야 한다

```jsx
GET /Member/1 -> GET /members/1
```

*   자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE) 로 표현한다.

    * URI에 HTTML Method가 들어가면 안된다

    ```jsx
    GET /members/delete/1 -> DELETE /members/1
    ```

    * URI에 행위에 대한 동사 표현이 들어가면 안된다(CRUD 기능을 나타내는 것은 URI에 사용하지 않는다.)

    ```jsx
    GET /members/show/1 -> GET /members/1
    GET /members/insert/2 -> POST /members/2
    ```

    * 경로 부분 중 변하는 부분은 유일한 값으로 대체한다(즉 :id는 하나의 특정 resource를 나타내는 고유값이다)
      * Ex) student를 생성하는 route: POST/students
      * Ex) id=12인 student를 삭제하는 route: DELETE/students/12

### **REST API 설계 규칙**

1. 슬래시 구분자(/)는 계층 관계를 나타내는데 사용한다.
2. URI 마지막 문자로 슬래시를 포함하지 않는다.
   1. URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는 것은 리소스가 다르다는 것이고, 역으로 리소스가 다르며 URI도 달라져야 한다.
3. 하이픈(-) 은 URI 가독성을 높이는데 사용
   1. 불가피하게 긴 URI 경로를 사용하게 된다면 하이픈을 사용해 가독성을 높인다.
4. 밑줄(\_)은 사용하지 않는다.
   1. 밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 하므로 가독성을 위해 밑줄은 사용 x
5. URI 경로에는 소문자가 적합
   1. URI 경로에 대문자 사용은 피하도록 한다
   2. RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문
6. 파일 확장자는 URI에 포함하지 않는다.

<table><thead><tr><th width="260">CRUD</th><th>HTTP verbs</th><th>Route</th></tr></thead><tbody><tr><td>resource들의 목록을 표시</td><td>GET</td><td>/resource</td></tr><tr><td>resource 하나의 내용을표시</td><td>GET</td><td>/resource/:id</td></tr><tr><td>resource를 생성</td><td>POST</td><td>/resource</td></tr><tr><td>resource를  수정</td><td>PUT</td><td>/resource/:id</td></tr><tr><td>resource를 삭제</td><td>DELETE</td><td>/resource/:id</td></tr></tbody></table>

## RESTful의 개념

### RESTful이란

* RESTful은 일반적으로 REST라는 아키텍쳐를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
  * REST API를 제공하는 웹 서비스를 RESTful하다고 할 수 있다.
* RESTful은 REST를 REST 답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것이 아니다.
  * 즉, REST 원리를 잘 따르는 시스템은 RESTful 용어로 지칭된다.

### RESTful의 목적

* 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것

### RESTful하지 못한 경우

* CRUD 기능을 모두 POST 로만 처리하는 API
* route에 resource, id 외의 정보가 들어가는 경우
