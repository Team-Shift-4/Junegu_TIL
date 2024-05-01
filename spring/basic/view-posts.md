# 게시글 조회

### 게시글 읽기 : Read

**단일 데이터 조회하기**

<figure><img src="../../.gitbook/assets/3-1 (2).png" alt=""><figcaption></figcaption></figure>

1. URL 요청 받기
   * DB에 저장된 데이터를 웹페이지에서 보려 해당 출력 페이지에 접속해야 한다
   * ex) 게시글 1번 id를 조회할때 : localhost:8080/articles/1 URL 요청
   * articles/id 로 url 요청시 이를 처리할 컨트롤러 생성 필요

* ArticleController 하단에 추가
  * url 요청을 받아 수행할 show() 메서드 생성
  * @PathVariable
    * URL요청으로 들어온 전달값을 컨트롤러의 매개변수로 가져오는 어노테이션

```java
@GetMapping("/articles/{id}")
public String show(@PathVariable Long id){
	log.info("id="+id);
    return "";
}
```

2. 데이터 조회해 출력하기 3단계
   1. id를 조회해 데이터 가져오기(리파지터리가)
   2. 가져온 데이터를 모델에 등록하기
   3. 조회한 데이터를 사용자에게 보여주기 위한 view 페이지 만들고 반환하기

* id를 조회해 데이터 가져오기
  * 조회(read) : DB의 SELECT 작업 -> CrudRepository의 findById() 메서드
  * findById()
    * JPA의 CrudRepository의 findById() 메서드
    * 특정 엔티티의 id 값을 기준으로 데이터를 찾아 반환

<figure><img src="../../.gitbook/assets/3-2 (1).png" alt=""><figcaption></figcaption></figure>

* 엔티티의 id 값을 기준으로 데이터를 찾아 반환
* 해당 id 값이 없으면 null 반환

```java
@GetMapping("/articles/{id}")
public Sring show(@PathVariable Long id) {
    log.info("id=" + id);
    //1. id를 조회해서 데이터 가져오기
    Article articleEntity = articleRepository.findById(id).orElse(null);
    
    return "";
}
```

* 모델에 데이터 등록하기
  * articleEntity에 담긴 데이터를 모델에 등록
  * 데이터를 모델에 등록하는 이유는 MVC 패턴에 따라 조회한 데이터를 뷰 페이지에서 사용하기 위함이다

<figure><img src="../../.gitbook/assets/3-3 (1).png" alt=""><figcaption></figcaption></figure>

* 매개변수로 model 객체를 받아온다
* 모델에 데이터 등록시 addAttribute() 메서드 사용

```java
형식 model.addAttribute(String name, Object value) // name이라는 이름으로 value 객체 추가
```

```java
@GetMapping("/articles/{id}")
public String show(@PathVariable Long id, Model model){
    log.info("id="+id);

    //1. id를 조회해서 데이터 가져오기
    Article articleEntity = articleRepository.findById(id).orElse(null);

    //2. 모델에 데이터 등록하기. article이라는 이름으로 articleEntity 객체 등록
    model.addAttribute("article", articleEntity);

    return "";
}
```

* 뷰 페이지 반환하기

<figure><img src="../../.gitbook/assets/3-4 (1).png" alt=""><figcaption></figcaption></figure>

* 뷰페이지가 articles/show.mustache 작성되어 있다 가정하고 입력

```java
@GetMapping("/articles/{id}")
    public String show(@PathVariable Long id, Model model){
        log.info("id="+id);

        //1. id를 조회해서 데이터 가져오기
        Article articleEntity = articleRepository.findById(id).orElse(null);

        //2. 모델에 데이터 등록하기. article이라는 이름으로 articleEntity 객체 등록
        model.addAttribute("article", articleEntity);

        //3. 뷰 페이지 반환하기
        return "articles/show";
    }
```

* 모델에 등록한 article을 뷰 페이지에서 머스테치 문법인 \{{이중괄호\}}를 이용해 출력함
* \{{#article\}} \~\~ \{{/article\}}, #을 이용해 열고 /을 이용해 닫는다
* article에 담긴 id, title, content를 이중 중괄호를 이용해 가져온다
* show.mustache

```html
{{>layouts/header}}
<table class="table">
    <tbody>
    {{#article}}
        <tr>
            <th>{{id}}</th>
            <td>{{title}}</td>
            <td>{{content}}</td>
        </tr>
    {{/article}}
    </tbody>
</table>
{{>layouts/footer}}
```

* 추가 단계 : 기본 생성자 추가하기
  * @NoArgsContructor : 기본 생성자를 추가해 주는 롬복 어노테이션
  * 기본 생성자 : 매개 변수가 없는 생성자

```java
@AllArgsConstructor
@NoArgsConstructor
@ToString
```

<figure><img src="../../.gitbook/assets/3-7 (1).png" alt=""><figcaption></figcaption></figure>

**여러 데이터 목록 조회하기**

* 단일 데이터 조회 : 리파지터리 엔티티 반환
* 데이터 목록 조회 : 엔티티의 묶음인 리스트(list) 반환

<figure><img src="../../.gitbook/assets/3-5 (2).png" alt=""><figcaption></figcaption></figure>

1. URL 요청받기

```java
@GetMapping("/articles")
public String index() {
    return "";
}
```

2. 데이터 조회해 출력하기
   1. 모든 데이터 가져오기
   2. 모델에 데이터 등록하기
   3. 뷰 페이지 설정하기

* 모든 데이터 가져오기
  * findAll() : JPA의 CrudRepository가 제공하는 메서드로, 특정 엔티티를 모두 가져와 Iterable 타입으로 반환됨. 작성한 타입은 List

```java
@GetMapping("/articles")
public String index() {
	// 1. 모든 데이터 가져오기
    List<Article> articleEntityList = articleRepository.findAll(); // 반환 타입이 맞지 않아서 오류 발생
    
    return "";
}
```

* 해결방법 1) articleEntityList 타입을 findAll() 메서드 반환타입으로 맞춤

```
@GetMapping("/articles")
public String index() {
	// 1. 모든 데이터 가져오기
    Iterable<Article> articleEntityList = articleRepository.findAll();
    
    return "";
}
```

* 해결방법 2) Iterable을 List로 downcasting
* ListarticleEntityList = (List)articleRepository.findAll();

<figure><img src="../../.gitbook/assets/3-6 (1).png" alt=""><figcaption></figcaption></figure>

* 해결방법 3) findAll() 메서드가 Iterable이 아닌 ArrayList 타입으로 반환하도록 수정 한다
* CrudRepository의 findAll() 메서드를 Overriding하여 반환값을 ArrayList로 수정 한다
* ArticleRepository > 블록 안 마우스 우클릭 > generate > Override Methods > findAll():Iterable 선택

```java
public interface ArticleRepository extends CrudRepository<Article,Long> {
    @Override // 상위클래스 메서드를 하위 클래스가 재정의해서 사용
    ArrayList<Article> findAll(); //Iterable -> ArrayList로 수정
}
```

```java
@GetMapping("/articles")
public String index() {
    // ArrayList의 상위 타입인 List로 upcasting
    List<Article> articleEntityList = articleRepository.findAll();
    return "";
}
```

* 특정 메서드가 반환되는 데이터 타입과 사용자가 작성한 반환 데이터 타입이 다를 경우, 3가지 방법으로 해결
  1. 메서드가 반환하는 데이터 타입을 사용자가 작성한 데이터 타입으로 캐스팅(형 변환)하기
  2. 사용자가 작성한 데이터 타입을 메서드가 반환하는 데이터 타입으로 수정하기
  3. 메서드의 반환 데이터 타입을 원하는 타입으로 오버라이딩하기
* index() 메서드의 매개변수로 model 객체 받아옴
* model.addAttribute() 메서드로 articleEntityList를 articleList이름으로 등록

```java
@GetMapping("/articles")
public String index(Model model) {
    // ArrayList의 상위 타입인 List로 upcasting
    List<Article> articleEntityList = articleRepository.findAll();

    // 모델에 데이터 등록하기
    model.addAttribute("articleList", articleEntityList);

    return "articles/index";
}
```

*   뷰 페이지 설정하기

    * index.mustache 파일이 뷰 페이지로 설정되도록 return문 수정

    ```java
    return "articles/index";
    ```
* index.mustache 파일 생성하기

```html
{{>layouts/header}}
<table class="table">
    <tbody>
    {{#articleList}}
        <tr>
            <th>{{id}}</th>
            <td>{{title}}</td>
            <td>{{content}}</td>
        </tr>
    {{/articleList}}
    </tbody>
</table>
{{>layouts/footer}}
```

<figure><img src="../../.gitbook/assets/3-8 (1).png" alt=""><figcaption></figcaption></figure>

**링크와 리다이렉트**

<figure><img src="../../.gitbook/assets/3-9 (3).png" alt=""><figcaption></figcaption></figure>

*   링크(link)

    * 미리 정해 놓은 요청을 간편히 전송하는 기능
    * 태그 혹은태글 작성
    * 클라이언트가 링크를 통해 어느 페이지로 이동하겠다고 요청하면 서버는 결과 페이지를 응답

    <figure><img src="../../.gitbook/assets/3-10 (2).png" alt=""><figcaption></figcaption></figure>
* 리다이렉트(redirect)
  * 클라이언트가 보낸 요청을 마친 후 계속해서 처리할 다음 요청 주소를 재지시하는것

<figure><img src="../../.gitbook/assets/3-11 (2).png" alt=""><figcaption></figcaption></figure>

1. 새 글 작성 링크 만들기
   * <목록 페이지 index.mustache> -> <새 글 작성 new.mustache>

```java
<a href="/articles/new">New Article</a>
```

2. <입력 페이지 new.mustache> -> <목록 페이지 index> 돌아가기

```java
<button type="submit" class"bnt bnt-primary">
    Submit
</button>
<a href="/articles">Back</a>
```

3. <입력 페이지> -> <상세 페이지> 이동하기
   * 새글 작성하면 상세 페이지 나오지 않고 오류 화면 나옴
   * 리다이렉트 적용하여 해결
   * ```java
     //형식 return "redirect:URL_주소"
     ```
   * 원하는 id의 글의 페이지로 리다이렉트
     * article을 saved 객체에 저장함
     * Article.java에 getter 메서드 생성 : getId()

```java
@PostMapping("/articles/create")
public String createArticle(ArticleForm form) {
    log.info(form.toString());
​
    Article article = form.toEntity();
​
    Article saved = articleRepository.save(article);
    log.info(saved.toString());
​
    return "redirect:/articles/";
}
```

4. <상세 페이지> -> <목록 페이지> 돌아가기
   * 상세페이지의 url 'articles/{id}'를 받는 컨트롤러의 메서드는 show()
   * show() 메서드의 return "articles/show" 로 뷰파일 확인

```html
<a href="/articles">Go to Article List</a>
```

5. <목록 페이지> -> <상세 페이지> 이동하기
   * 제목 클릭하면 해당 글의 <상세 페이지>로 이동

```html
{{#articleList}}
    <tr>
        <th>{{id}}</th>
        <td><a href="/articles/{{id}}">{{title}}</a></td>
        <td>{{content}}</td>
    </tr>
{{/articleList}}
​
```

\
