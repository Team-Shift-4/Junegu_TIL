# REST API와 JSON

### REST API

* IT 기술 발전에 따라 수많은 클라이언트 만들어짐
* 서버는 수 많은 클라이언트 기기에 맞는 뷰(VIEW) 페이지로 응답해야 함
* 서버가 다양한 클라이언트에 맞게 일일이 대응하기 쉽지 않음
  * JSON 데이터로 응답하는 REST API 등장

<figure><img src="../../.gitbook/assets/6-1 (1).png" alt=""><figcaption></figcaption></figure>

* REST API(Representational State Transfer API)
  * 서버의 자원을 클라이언트에 구애받지 않고 사용할 수 있게 하는 설계 방식. 모든 기기에 통용될 수 있는 데이터를 반환
* JSON(JavaScript Object Notation)
  * 서버가 클라이언트의 요청에 대한 응답으로 화면(view)가 아닌 데이터(data)를 전송할 때 사용하는 응답 데이터
  * JSON 데이터 : 키(key)와 값(value)로 구성된 정렬되지 않는 속성(property)의 집합

```json
{
    "키1": 값,
    "키2": 값,
    "키3": 값
}
```

* REST API의 동작

<figure><img src="../../.gitbook/assets/6-2 (1).png" alt=""><figcaption></figcaption></figure>

1. {JSON} Placeholder 사이트 : https://jsonplaceholder.typicode.com
   * 가짜(fake) API를 사용해 무료로 각종 테스트를 진행할 수 있는 서비스 제공

* HTTP 상태 코드 : 클라이언트가 보낸 요청이 성공했는지, 실패했는지 알려 주는 코드. 100번대에서 500번대 5개 그룹으로 구분

| 상태 코드            | 설명                                          |
| ---------------- | ------------------------------------------- |
| 1XX(정보)          | 요청이 수신돼 처리 중입니다.                            |
| 2XX(성공)          | 요청이 정상적으로 처리됐습니다.                           |
| 3XX(리다이렉션 메세지)   | 요청을 완료하려면 추가 행동이 필요합니다.                     |
| 4XX(클라이언트 요청 오류) | 클라이언트의 요청이 잘못돼 서버가 요청을 수행할 수 없습니다.          |
| 5XX(서버 응답 오류)    | 서버 내부에 에러가 발생해 클라이언트 요청에 대해 적절히 수행하지 못했습니다. |

### HTTP와 REST 컨트롤러

* HTTP 요청 메시지의 구조

<figure><img src="../../.gitbook/assets/6-3 (1).png" alt=""><figcaption></figcaption></figure>

* HTTP 응답 메시지의 구조

<figure><img src="../../.gitbook/assets/6-4 (1).png" alt=""><figcaption></figcaption></figure>

* REST와 API의 의미
  * REST : HTTP URL로 서버의 자원(resource)을 명시하고, HTTP 메서드(POST, GET, PATCH/PUT, DELETE)로 해당 자원에 대해 CRUD(생성, 조회, 수정, 삭제)하는 것
  * API : 클라이언트가 서버의 자원을 요청할 수 있도록 서버에서 제공하는 인터페이스(interface)
* Article 데이터를 CRUD하기 위한 REST API 주소 설계
  * 조회요청 /api/articles 또는 /api/articles/{id}
  * GET 메서드로 Article 목록 전체 또는 단일 Article을 조회
* 생성요청 /api/articles
  * POST 메서드로 새로운 Article을 생성해 목록에 저장
* 수정요청 /api/articles/{id}
  * PATCH 메서드로 특정 Article의 내용을 수정
* 삭제요청 /api/articles/{id}
  * DELETE 메서드로 특정 Article을 삭제
* Article 데이터를 CRUD하기 위한 REST API 주소 설계

<figure><img src="../../.gitbook/assets/6-5 (1).png" alt=""><figcaption></figcaption></figure>

* REST 컨트롤러와 ResponseEntity 클래스
  * URL요청을 받아 그 결과를 JSON으로 반환해 줄 컨트롤러 생성
    * 게시판을 만들 때는 일반 컨트롤러(ArticleController) 사용
    * REST API로 요청과 응답을 주고받을 때는 REST 컨트롤러 사용
* ResponseENtity 클래스
  * REST API의 응답을 위해 사용하는 클래스
  * REST API 요청을 받아 응답할 때 HTTP 상태 코드, 헤더, 본문을 실어 보낼 수 있음

<figure><img src="../../.gitbook/assets/6-6 (1).png" alt=""><figcaption></figcaption></figure>

### Rest API 구현하기

1. REST 컨트롤러 맛보기
   1. com.example.firstproject.api 패키지 생성
   2. api/FirstApiController.java 파일 생성
2. REST 컨트롤러와 일반 컨트롤러의 차이

* REST 컨트롤러 : JSON이나 텍스트 같은 데이터만 반환
* 일반 컨트롤러 : 뷰 페이지 (HTML 코드) 반환

```java
@RestController  //@Controller대신 REST API용 컨트롤러
public class FirstApiController {
    @GetMapping("/api/hello")
    public String hello() {
        return "hello world!";
    }
}
```

<figure><img src="../../.gitbook/assets/6-7 (1).png" alt=""><figcaption></figcaption></figure>
