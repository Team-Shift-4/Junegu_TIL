# 게시글 수정

### 게시글 수정하기 : Update

**데이터 수정 과정**

* 데이터 수정 과정 2단계
  * 1단계 : 수정 페이지 만들고 기존 글 불러오기 : 해당 글을 불러와 수정할 수 있는 입력 상태로 만든다
  * 2단계 : 데이터 수정해 DB에 반영한 후 상세 페이지로 리다이렉트한다
* 1단계 : 수정 페이지 만들고 기존 글 불러오기

<figure><img src="../../.gitbook/assets/4-1 (1).png" alt=""><figcaption></figcaption></figure>

* 2단계 : 데이터 수정해 DB에 반영한 후 상세 페이지로 리다이렉트한다

<figure><img src="../../.gitbook/assets/4-2 (1).png" alt=""><figcaption></figcaption></figure>

1. <상세 페이지>에 Edit 버튼 만들기
   * 상세 페이지 show.mustache
   * 선택한 id에 해당하는 글을 수정하는 edit페이지로 이동 한다
   * 부트스트랩 CSS 적용한다

```html
<a href="/articles/{{article.id}}/edit class="btn btn-primary">Edit</a>
```

* 수정 요청 받아 처리할 edit() 메서드와 수정 페이지 edit.mustache 작성

1. Edit 요청을 받아 데이터 가져오기
   1. edit() 메서드 기본 틀 만들기
   2. 수정할 데이터 가져오기
   3. 모델에 데이터 등록하기
2. 수정 폼 만들기

* edit() 메서드 기본 틀 만들기
  * @GetMapping() 작성
    * show.mustache 파일에서 "/articles/\{{article.id\}}/edit" 주소 연결 요청
    * url "/articles/{id}/edit" 작성
    * 주의 : 변수 id는 \{{id\}} 아니라 {id} 이다
    * 뷰 페이지에서 변수 사용할 떄는 \{{\}} 중괄호 2개
    * 컨트롤러의 url 변수를 사용할 때는 {} 중괄호 1개

```java
@GetMapping("/articles/{id}/edit")
public String edit() {
    // 뷰 페이지 설정
    return "articles/edit"
}
```

* 수정할 데이터 가져오기
  * @GetMapping 어노테이션의 url 주소에 있는 id 받아오는 @PathVariable 어노테이션 추가하여 매개변수 작성
* articleRepository의 findById(id) 메서드로 데이터 찾아옴

```java
@GetMapping("/article/{id}/edit")
public String edit(@PathVariable Long id, Model model) {
    log.info("id="+id);

    // 수정할 데이터 가져오기
    Article articleEntity = articleRepository.findById(id).orElse(null);

    // 뷰 페이지 설정
    return "article/edit";
}
```

* 모델에 데이터 등록하기
  * 모델 사용하기 위해 메서드의 매개변수로 model 객체 받아옴
  * addAttribute() 메서드로 모델에 데이터 등록하여 DB에서 가져온 데이터를 article 이름으로 뷰 페이지에서 사용할 수 있다

```java
@GetMapping("/article/{id}/edit")
public String edit(@PathVariable Long id, Model model) {
    log.info("id="+id);

    // 수정할 데이터 가져오기
    Article articleEntity = articleRepository.findById(id).orElse(null);
	
    // 모델에 데이터 등록하기
    model.addAttribute("article", articleEntity);

    // 뷰 페이지 설정
    return "article/edit"; // 수정 폼 만들어야 함
}
```

* 수정 폼 (edit.mustache) 만들기 : new.mustache를 복사하여 수정

```html
{{>layouts/header}}
<form class="container" action="" method="post"> //action 속성값 삭제
    <div class="mb-3">
        <label class="form-label">제목</label>
        <input type="text" class="form-control" name="title" value="{{article.title}}">
    </div>
    <div class="mb-3">
        <label class="form-label">내용</label>
        <textarea class="form-control" rows="3" name="content">
           {{article.content}}
        </textarea>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
    <a href="/articles/{{article.id}}">Back</a>
</form>
{{>layouts/footer}}
```

* 3번 반복되는 \{{article/xxx\}}를 범위로 변경 가능 \{{#article\}},\{{/article\}}
* \{{article.xxx\}} -> \{{xxx\}}. ex) \{{article.title\}} -> \{{title\}}

```html
{{>layouts/header}}
<form class="container" action="" method="post">
    {{#article}}
    <div class="mb-3">
        <label class="form-label">제목</label>
        <input type="text" class="form-control" name="title" value="{{title}}">
    </div>
    <div class="mb-3">
        <label class="form-label">내용</label>
        <textarea class="form-control" rows="3" name="content">
            {{content}}
        </textarea>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
    <a href="/articles/{{id}}">Back</a>
    {{/article}}
</form>
{{>layouts/footer}}

```

* localhost:8080/articles 접속 New Article 링크 클릭
* 내용 작성 후 submit 클릭하여 상세 페이지로 이동
* 상세 페이지에서 \[Edit] 버튼 클릭 하여 확인

<figure><img src="../../.gitbook/assets/4-1 (2).png" alt=""><figcaption></figcaption></figure>

* 데이터 수정 단계
  * 1단계 : 수정 페이지를 만들고 기존 데이터 불러오기
  * 2단계 : 데이터를 수정해 DB에 반영한 후 상세페이지로 리다이렉트하기

<figure><img src="../../.gitbook/assets/4-2 (2).png" alt=""><figcaption></figcaption></figure>

* 클라이언트-서버 간 데이터 처리를 위한 4가지 기술

<figure><img src="../../.gitbook/assets/4-3 (1).png" alt=""><figcaption></figcaption></figure>

* MVC : 서버 역활을 분담해 처리하는 기법
* JPA : 서버와 DB 간 소통에 관여하는 기술
* SQL : DB 데이터를 관리하는 언어
* HTTP : 데이터를 주고받기 위한 통신 규약

**HTTP(HyperText Transfer Protocol**

* 웹 서비스에 사용하는 프로토콜
* 클라이언트의 다양한 요청을 메서드를 통해 서버로 보내는 역할함
* HTTP의 대표 메서드 : 데이터 관리에 가장 기본이 되는 동작
  * POST : 데이터 생성 요청 -> Create
  * GET : 데이터 조회 요청 -> Read
  * PATCH(PUT) : 데이터 수정 요청 -> Update
  * DELETE : 데이터 삭제 요청 -> Delete

| 데이터 관리         | SQL    | HTTP        |
| -------------- | ------ | ----------- |
| 데이터 생성(Create) | INSERT | POST        |
| 데이터 조회(Read)   | SELECT | GET         |
| 데이터 수정(Update) | UPDATE | PATHCH(PUT) |
| 데이터 삭제(Delete) | DELETE | DELETE      |

1. 더미 데이터 설정하기
   * 인메모리 방식 h2 불편함. 서버 재시작하면 데이터 삭제됨
   * 실습의 원활함 위해 더미 데이터 설정하기

* src/main/resources/data.sql 생성

```sql
INSERT INTO article(id, title, content) VALUES(1,'가가가가','1111');
INSERT INTO article(id, title, content) VALUES(2,'나나나나','2222');
INSERT INTO article(id, title, content) VALUES(3,'다다다다','3333');
```

* 스프링부트 2.5 버전부터 data.sql 이용한 데이터 초기화 권장 안함
* src/main/resources/application.properties에 옵션 추가하여 오류 해결

```java
server.servlet.encoding.force=true
spring.h2.console.enabled=true
spring.jpa.defer-datasource-initialization=true
```

* 서버 실행 후 localhost:8080/articles 확인

1. <수정 페이지> 변경하기

* action 속성의 url 추가
* id가 몇 번인 article을 수정할지 알려주기 위해 숨김(hidden)으로 id 전달

```html
<form class="container" action="/articles/update" method="post">
    <input type="hidden" name="id" value="{{id}}">
    <div class="mb-3">
```

※ 수정인데 method가 PATCH가 아니라 POST인 이유

* 태그가 예전에 만들어진 규격이라 PATCH 메서드 지원안함
* 태그는 get, post 메서드만 지원
* patch 메서드 지원하는 방법도 있으나 post 그대로 데이터 수정

2. 수정 데이터 받아 오기
   1. update() 메서드 기본 틀 만들기
   2. 수정 데이터를 DTO에 담기

```java
@PostMapping("/articles/update")
    public String update(ArticleForm form) {   //매개변수로 DTO 받아오기  
      log.info(form.toString());  //수정 데이터 잘 받았는지 로그 찍어 확인  
      return "";  
    }  
}
```

```java
public class ArticleForm {
    private Long id;  // 수정 폼에서 input 으로 id추가했으므로 DTO에도 추가
    private String title; 
    private String content; 

    public Article toEntity() {
        return new Article(id, title, content);
    } 
} // id 필드 없어 null  추가했으므로 생성자에도 id 작성해줌
```

3. DB에 저장하고 결과 페이지로 리다이렉트하기
   1. DTO를 엔티티로 변환하기
   2. 엔티티를 DB에 저장하기

<figure><img src="../../.gitbook/assets/4-2 (3).png" alt=""><figcaption></figcaption></figure>

3. 결과 페이지로 리다이렉트하기

<figure><img src="../../.gitbook/assets/4-4 (1).png" alt=""><figcaption></figcaption></figure>

4. DB에 저장하고 결과 페이지로 리다이렉트하기
   * 게시글 수정이 반영되는지 확인하기

```java
public String update(ArticleForm form) {
        log.info(form.toString());
        //1. DTO를 엔티티로 변환하기
        Article articleEntity = form.toEntity();
        log.info(articleEntity.toString());
        //2. 엔티티를 DB에 저장하기
        //2-1. DB에서 기존 데이터 가져오기
        Article target=articleRepository.findById(articleEntity.getId()).orElse(null);
        //2-2. 기존 데이터 값 갱신하기
        if (target!=null) {
            articleRepository.save(articleEntity);
        } return "";
    } 
}
```

```java
public String update(ArticleForm form) {
       (중략)
        //3. 수정 결과 페이지로 리다이렉트하기
        return "redirect:/article“ + articleEntity.getId();
    }
}
```

<figure><img src="../../.gitbook/assets/4-5 (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4-6 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4-7 (1).png" alt=""><figcaption></figcaption></figure>
