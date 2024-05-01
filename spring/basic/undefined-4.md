# 게시글 삭제

### 게시글 삭제하기 : Delete

* 데이터 삭제 과정
  * 클라이언트가 HTTP 메서드로 특정 게시글의 삭제를 요청
  * 삭제 요청 받아 컨트롤러는 리파지터리를 통해 DB에 저장된 데이터 찾아 삭제. 이 작업은 기존 데이터가 있는 경우에만 수행됨
  * 삭제가 완료되면 클라이언트를 결과 페이지로 리다이렉트함 : 리다이렉트시 삭제 완료 메시지 띄우기

<figure><img src="../../.gitbook/assets/5-1 (1).png" alt=""><figcaption></figcaption></figure>

* 삭제 완료 메시지 띄우는 방법
  * RedirectAttribytes 객체의 addFlashAttribute() 메서드 : 리다이렉트된 페이지에서 사용할 일회성 데이터 등록

<figure><img src="../../.gitbook/assets/5-2 (1).png" alt=""><figcaption></figcaption></figure>

1. <상세페이지> 에 Delete 버튼 추가하기

```html
<a href="/articles/{{article.id}}/edit" class="btn btn-primary">Edit</a>
<a href="/articles/{{article.id}}/delete" class="btn btn-danger">Delete</a>
```

2. Delete 요청을 받아 데이터 삭제하기
   1. delete() 메서드 기본 틀 만들기
   2. 삭제할 대상 가져오기
   3. 대상 엔티티 삭제하기
   4. 결과 페이지로 리다이렉트하기

```java
@GetMapping("/articles/{id}/delete")
    public String Delete(@PathVariable Long id) {
        log.info("삭제 요청이 들어왔습니다!");
         //1. 삭제할 대상 가져오기
           Article target = articleRepository.findById(id).orElse(null);
           log.info(target.toString());
        //2. 대상 엔티티 삭제하기
            if(target != null) {
                articleRepository.delete(target);             }
        //3. 결과 페이지로 리다이렉트하기
              return "redirect:/articles";
    }
}
```

* Delete 요청을 받아 데이터 삭제하기 : 확인
  * localhost:8080/h2-console
  * Article 테이블 ->< RUN 확인

3. 삭제 완료 메시지 남기기
   * RedirectAttributes 객체의 addFlashAttribute() 메서드 활용
   * 리다이렉트 시점에 한 번만 사용할 데이터 등록
   * 한 번 쓰고 사라지는 휘발성 데이터 등록

```java
//형식 객체명.addFlashAttribute(넘겨_주려는_키_문자열, 넘겨_주려는_값_객체);
```

```java
//delete 메서드의 매개변수로 RedirectAttributes 객체 받아옴
 public String Delete(@PathVariable Long id, RedirectAttributes rttr) {
       (중략)
            if(target != null) {
                articleRepository.delete(target);
   // if문에서 삭제 수행 후 msg 키 문자열에 “삭제됐습니다” 값의 메시지 넣음
                rttr.addFlashAttribute("msg", "삭제됐습니다!");
            }
      (중략)
    }

```

* msg 키 값에 담긴 메시지는 리다이렉트 되는 페이지 index.mustache 에서 사용
* index.mustache의 헤더 안에 메시지 출력하도록 함 : header.mustache
* \{{#msg\}}\{{/msg\}} : msg 키를 사용할 범위 잡아 줌
* 메시지 창과 닫기버튼 부트스트랩 (부트스트랩 접속 -> Docs -> alert, close button 검색)

```html
</nav>
{{#msg}}
    <div class="alert alert-primary alert-dismissible">    
      {{msg}}
      <button type="button" class="btn-close" data-bs-dismiss="alert" 
              aria-label="close"></button>
    </div>
{{/msg}}
```

* Delete 요청을 받아 데이터 삭제하기 : 삭제 메시지 확인

<figure><img src="../../.gitbook/assets/5-3 (1).png" alt=""><figcaption></figcaption></figure>

### JPA 로깅 설정하기

* CRUD와 SQL 쿼리
  * 서버에서 CRUD 요청을 하면 JPA의 리파지터리가 DB에 해당 요청 전달
  * DB는 각 요청에 따라 INSERT 문, SELECT 문, UPDATE 문, DELETE 문 등의 쿼리문 수행

<figure><img src="../../.gitbook/assets/5-4 (1).png" alt=""><figcaption></figcaption></figure>

* 로깅(logging)
  * 시스템이 작동할 때 당시의 상태와 작동 정보를 기록하는 것
* 로깅(logging) 레벨 7 단계
  * 출력 레벨 설정하면 레벨 이상의 로그가 출력됨
  * 예) INFO 레벨로 설정하면 그 이상 레벨, 즉 INFO/WARN/ERROR/FATAL/OFF 레벨의 모든 로그가 기록됨

| 레벨 1 | TRACE | DEBUG 레벨보다 더 상세한 정보        |
| ---- | ----- | -------------------------- |
| 레벨 2 | DEBUG | 응용 프로그램을 디버깅하는 데 필요한 정보    |
| 레벨 3 | INFO  | 응용 프로그램의순조로운 진행 정보         |
| 레벨 4 | WARN  | 잠재적으로 유해한 상황 정보            |
| 레벨 5 | ERROR | 응용 프로그램이 수행할 수 있는 정도의오류 정보 |
| 레벨 6 | FATAL | 응용 프로그램이 중단될 만한 심각한 오류 정보  |
| 레벨 7 | OFF   | 로깅 기능 해제                   |

* JPA 로깅 설정은 application.properties 파일에 작성 추가

```java
# JPA 로깅 설정
# 디버그 레벨로 쿼리 출력
logging.level.org.hibernate.SQL=DEBUG
```

* JPA가 동작할 때 수행되는 SQL 쿼리 보기 위해 DEBUG 레벨로 설정
* 서버 재시작해서 SQL 로그 확인
  * org.hibernate.SQL 옆에 drop, create 등 확인
* application.properties 파일에 SQL 쿼리 줄바꿈 추가

```
 # 디버그 레벨로 쿼리 출력
logging.level.org.hibernate.SQL=DEBUG

# 쿼리 줄바꿈하기
spring.jpa.properties.hibernate.format_sql=true
```

* 한 줄로 작성된 SQL 쿼리에 줄바꿈 적용
* JPQ 쿼리에서 DB에 넘어가는 매개 변수 값 확인 코드 추가

```
 # 쿼리 줄바꿈하기
spring.jpa.properties.hibernate.format_sql=true

# 매개변수 값 보여주기
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
```

* H2 DB 접속 시 URL 고정 방법
  * 유니크 URL 설정을 false로 하고, 어떤 값을 고정 URL로 할 건지 작성

**SQL 쿼리 로그 확인하기**

* 데이터 생성 시 : INSERT 문 사용
  * id 자동 생성 전략 추가하기
    * 기본키(primary key) : 테이블에 저장된 각 데이터를 유일하게 구분할수 있도록 지정한 속성. 보통 id를 기본키로 많이 사용한다
    * @GeneratedValue 어노테이션의 전략을 IDENTITY로 설정하면 DB가 id를 자동으로 생성하므로 id 값이 중복되지 않는다

```java
 public class Article {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // DB가 id 자동 생성  
    private Long id;
```

* DB가 ID를 자동생성 하므로 data.sql 파일에 id 값 삭제

```sql
INSERT INTO article(title, content) VALUES('가가가가', '1111');
INSERT INTO article(title, content) VALUES('나나나나', '2222');
INSERT INTO article(title, content) VALUES('다다다다', '3333');
```

* 테이블 생성 시 : CREATE문 사용
  * coffee 테이블 만들기
    * CREATE TABLE 문

```sql
CREATE TABLE 테이블명 (
	속성명1 자료형,
    속성명2 자료형,
    속성명3 자료형,
    PRIMARY KEY(기본키)
);
```

* 데이터 삽입 시 : INSERT문 사용
  * coffe 데이터 생성하기
    * INSERT 문

```sql
INSERT
INTO
	테이블명
	(속석명1, 속성명2, 속성명3, ...)
VALUES
	(값1, 값2, 값3, ...);
```

* 데이터 조회 시 : SELECT 문 사용
  * coffee 데이터 조회하기
    * SELECT 문

```sql
SELECT
	속성명1, 속성명2, 속성명3
FROM
	테이블명
WHERE
	조건;
```

* 데이터 수정 시 : UPDATE 문 사용
  * coffee 데이터 수정하기
    * UPDATE 문

```sql
UPDATE
	테이블명
SET
	속성명=변경할 값
WHERE
	조건;
```

* 데이터 삭제 시 : DELETE 문
  * coffee 데이터 삭제하기
    * DELETE 문

```sql
DELETE
[FROM] -- []: 생략 가능
	테이블명
WHERE
	조건;
```
