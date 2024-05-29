# Spring Security

#### 스프링 시큐리티

* **스프링 시큐리티란?**
  * 스프링 기반의 애플리케이션 보안(인증, 인가, 권한)을 담당하는 스프링 하위 프레임워크. 세션 기반 인증 제공
* **인증과 인가**
  * 인증(Authentication) : 사용자의 **신원을 입증**
  * ex) 로그인할때 누구인지 확인하는 과장
  * 인가(authorization) : 사이트의 특정 부분에 접근할 수 있는 **사용자인지 권한을 확인하는 작업**&#x20;
  * 스프링 시큐리티를 사용하면 쉽게 처리 가능 하다.
* CSRF 공격(사용자 권한을 가지고 특정 동작 유도하는 공격) 등을 방어 한다.
* 세션 고정 공격(사용자의 인증 정보를 탈취하거나 변조하는 공격) 등을 방어 한다.



#### CSRF / 세션 고정 공격

* **CSRF(Cross Site Request Forgery) 공격**
  * 사용자가 의도하지 않은 요청을 보내는 것을 의미 한다.
  * 사이트간에 요청을 위조하는 것을 의미 한다 .
  * ex) 사용자가 은행 웹 사이트에 로그인한 상태에서 악성 웹 사이트를 방문하면, 해당 웹 사이트는 사용자의 인증 정보를 사용하여 은행 계좌 이체 등의 요청을 보낼 수 있다, 이는 사용자의 동의 없이 이루어지는 공격이다.
* **CSRF 토큰**
  * 스프링 시큐리티는 기본적으로 서버에 영향을 줄 수 있는 POST, PUT, DELETE 등의 요청에서는 시큐리티가 관리하는 토큰인 CSRF 토큰을 사진 요청만 처리하도록 한다.
  * CSRF 토큰은 서버에서 생성되어 클라이언트로 전달되며, 클라이언트는 이 토큰을 요청에 함께 포함시켜야 한다.
  * 서버는 요청이 정상적인 경로로 들어왔는지 확인하고, 위조 요청을 방지할 수 있다.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

* **세션 고정(Session Fixation) 공격**
  * 사용자가 로그인할 때 발급받은 세션 ID를 악용하는 공격 방식 이다.
  * 사용자 로그인하여 서버에 고유한 세션 ID를 발급 받아 로그인 상태를 유지한다.
  * 세션 하이재킹 공격의 한 유형이다.
  * 세션 하이재킹 공격은 사용자의 세션 정보를 탈취하여 불법적으로 사용자의 계정에 접근하는 공격 방식이다.



* CSRF 토큰을 생성하여 HTML 포에 삽입하는 코드

```html
<input type="hidden" th:name="${_csrf?.parameterName}" th:value="{_csrf?.token}" />
```

* 숨겨진 input 태그로, th:name과 th:value 속성에는 서버에서 생성된 CSRF 토큰의 이름과 값을 동적으로 설정 한다.
* 생성된 토큰은 서버로 요청을 보낼 때 함께 전송되어, 서버는 이 토큰을 검증하여 정상적인 요청인지 확인한다.



* 스프링 시큐리티는 다양한 필터들로 나누어져 있으며, 각 필터에서 인증, 인가와 관련된 작업을 처리한다.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>스프링 시큐리티 필터 구조</p></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

* 핵심 역할은 Authentication Manager(인증 매니저)를 통해 이루어진다.
* Authentication Provider는 인증 매니저가 어떻게 동작해야 하는지 결정 한다.
* 최종적으로 실제 인증은 UserDetailsServices에 의해 이루어진다.

#### 스프링 시큐리티 폼 로그인 인증 흐름

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

1. 사용자가 폼에 아이디와 패스워드 입력하면 AuthenticationFilter가 아이디와 패스워드 유효성을 검사한다.
2. 실제 구현체인 UsernamePasswordAuthentiation Token을 만들어 넘겨준다
3. 전달받은 인증용 객체인 UsernamePasswordAuthentiation Token을 **Authentication Manager**에 보낸다.
4. UsernamePasswordAuthentiation Token을 Authentication Provider에게 보낸다.
5. 사용자 아이디를 **UserDetailService**에 보냄 UserDetailService는 사용자 아이디로 찾은 사용자 정보를 UserDetails객체로 만들어 Authentication Provider에게 전달 한다.
6. DB에 있는 사용자 정보를 가져온다.
7. 입력 정보와 UserDetails의 정보를 비교해 실제 인증을 처리한다.
8. 인증처리가 완료되면 SecurityContextHolder에 Authentication을 저장한다.

* 성공하면 AuthenticationSuccessHandler을  실행 한다.
* 실패하면 AuthenticationFailureHandler을 실행 한다.



#### 실습 : 스프링 시큐리티를 사용해 인증, 인가 기능 구현

* 테이블 생성 : 회원 정보 저장
* 도메인 생성 : 테이블과 연결한 도메인
* 엔티티 생성 : 테이블과 연결할 회원 엔티티
* 레포지터리 생성 : 회원 엔티티와 연결되어 데이터 조회
* 스프링 시큐리티에서 사용자 정보 가져오는 서비스

#### 회원 도메인 만들기

1. 의존성 추가 Build.gradle
2. 엔티티 생성 User.java
3. 레포지터리 생성 UserRepository.java
4. 서비스 메서드 코드 작성 UserDetailService.java



1. 의존성 추가 Build.gradle

```gradle
dependencies {
 (중  략 )
 // 스프링 시큐리티를 사용하기 위한 스타터 추가
  implementation 'org.springframework.boot:spring-boot-starter-security’

 // 타임리프에서 스프링 시큐리티를 사용하기 위한 의존성 추가
  implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity6’

 // 스프링 시큐리티를 테스트하기 위한 의존성 추가
 testImplementation 'org.springframework.boot:spring-boot-starter-test'
 }
```

2. 엔티티 생성 User.java

* 회원 엔티티와 매핑할 테이블 구조

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

* 엔티티 생성 domain/User.java
  * UserDetails 클래스를 상속하는 user 클래스 생성 한다.
* **UserDetails** 클래스
  * 스프링 시큐리티에서 **사용자의 인증 정보를 저장하는 메서드를 제공하는 인터페이스** 이다.
  * 해당 객체를 통해 인증 정보를 가져오려면 **필수 오버라이드 메서드**들을 여러 개 사용해야 한다.
* **UserDetailsService** 클래스
  * 스프링 시큐리티에서 **사용자의 정보를 가져오는 인터페이스** 이다
* UserDetails 인터페이스 필수 오버라이드 메서드

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* 엔티티 생성 domain/User.java
  * userdetails 오버라이드 메서드들

```java
public class User implements UserDetails {  //UserDetails 인터페이스를 상속받아 인증 객체로 사용

(생  략 )

@Override  //권한 반환
    public Collection<? Extends GrantedAuthority> getAuthorities() {
        return List.of(new SimpleGrantedAuthority(＂user＂));
    }
 ….
(생 략)

```



3. 리포지터리 생성 repository/UserRepository.java

* User 엔티티에 대한 리포지터리 생성 한다.
* 이메일로 사용자 식별한다 .

```java
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
}
```

* 리포지터리 생성 repository/UserRepository.java
  * 스프링 데이터 JPA는 **메서드 규칙에 맞추어 메서드 선언**하면 이름을 분석해 자동으로 쿼리 생성 해준다.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

4. 서비스 메서드 코드 작성 service/UserDetailService.java

* 스프링 시큐리티에서 로그인을 진행할 때 사용자 정보를 가져오는 코드 작성 한다.
* 스프링 시큐리티에서 **사용자의 정보를 가져오는 UserDetailService 인터페이스**를 구현 한다.

```java
//스프링 시큐리티에서 사용자 정보를 가져오는 인터페이스
public class UserDetailService implements UserDetailsService {

    private final UserRepository userRepository;

// 사용자 이름(email)으로 사용자의 정보를 가져오는 메서드
    @Override
    public User loadUserByUsername(String email) {
        return userRepository.findByEmail(email)
                 .orElseThrow(() -> new IllegalArgumentException((email)));
    }
}

```

#### 시큐리티 설정하기

* 인증을 위한 도메인, 리포지터리, 서비스는 완성
* 실제 인증을 처리하는 시큐리티 설정 파일을 작성해야 한다.
* WebSecutiryConfig.java

1. config/WebSecurityConfig.java

* 실제 인증을 처리하는 시큐리티 설정 파일

1. 스프링 시큐리티 기능 비활성화

* 스프링 시큐리티의 기능(인증, 인가 서비스)을 사용하지 않게 설정하는 코드 이다.
* 정적 리소스(/static/하위경로), h2Console 하위 url에 ignoring() 메서드 사용 한다.
* requestMatchers() : 특정 요청과 일치하는 url, 에 대한 액세스를 설정 한다.

2. 특성 HTTP 요청에 대한 웹 기반 보안 구성

* 인증/인가 및 로그인, 로그아웃 관련 설정 이다.

3. 인증 인가 설정

* 특정 경로에 대한 액세스 설정
* permitAll() : 누구나 접근 가능 서정. 즉 /login, /signup, /user로 요청이 오면 인증/인가 없이도 접근 가능 하다.
* anyRequest() : 위에서 설정한 url이외의 요청에 대해 설정 한다.
* authenticated() : 별도의 인가가 필요하지 않지만 인증이 성공된 상태여야 접근할 수 있다.

4. 폼을 기반 로그인 설정

* loginPage() : 로그인 페이지 경로 설정
* defaultSuccessUrl() : 로그인 완료되면 이동할 경로 설정

5. 로그아웃 설정

* logOutSucessUrl() : 로그아웃 완료시 이동할 경로 설정
* invalidateHttpSession() : 로그아웃 이후에 세션 전체 삭제할지 여부 설정

6. CSRF 공격 방지 설정 실습 편리하도록 비활성화
7. 인증 관리자 관련 설정
8. 사용자 정보 서비스 성정

* userDetailsService() : 사용자 정보를 가져올 서비스 설정. 반드시 UserDetailsService를 상속 받은 클래스여야 한다.
* PasswordEncoder() : 비밀번호 암호화를 위한 인코더 설정

9. 패스워드 인코더를 빈으로 등록

#### 회원 가입 구현하기

1. 사용자 정보를 담을 dto/AddUserRequest.java
2. 회원 정보 추가 메서드 Service/UserService.java
3. 회원 가입 요청시 controller/UserApiController.java
4. 회원가입, 로그인 화면으로 연결해 주는 뷰 컨트롤러 구현



1. 사용자 정보를 담을 dto/AddUserRequest.java

```java
public class AddUserRequest {
    private String email;
    private String password;
}
```

2. 회원 정보 추가 메서드 Service/UserService.java

* AddUserRequest 객체를 인수로 받는 회원 정보 추가 메서드
* 패스워드 저장시 시큐리티 설정. 패스워드 인코딩을 등록한 빈을 사용해서 암호화한 후 데이터베이스에  저장

```java
(생  략) 
public Long save(AddUserRequest dto) {
        return userRepository.save(User.builder()
                .email(dto.getEmail())
                 //패스워드 암호화
                .password(bCryptPasswordEncoder.encode(dto.getPassword()))
                .build()).getId();
    }
(생 략)
```

3. 회원 가입 요청 controller/UserApiController.java

* 회원 가입 처리가 된 다음 로그인 페이지로 이동 redirect:

```java
public class UserApiController {

    private final UserService userService;

    @PostMapping("/user")
    public String signup(AddUserRequest request) {
        userService.save(request); //회원 가입 메서드 호출
        return "redirect:/login";     //회원 가입이 완료된 이후에  /login URL에 해당하는 화면으로 이동    }
```

4. 회원가입, 로그인 화면으로 연결해 주는 뷰 컨트롤러 구현

* 회원가입, 로그인 화면으로 연결해 주는 뷰 컨트롤러 구현
  * controller/UserViewController.java
* 뷰 작성
  * 로그인 뷰 templates/login.html
  * 회원가입 뷰 templates/signup.html



* 회원가입, 로그인 화면으로 연결해 주는 뷰 컨트롤러 구현
  * controller/UserViewController.java

```java
@Controller
public class UserViewController {

    @GetMapping("/login")
    public String login() {
        return "login";
    }

    @GetMapping("/signup")
    public String signup() {
        return "signup";
    }
}
```

* 뷰 작성
  * 로그인 뷰 templates/login.html
  * 회원가입 뷰 templates/signup.html

```html
//CSRF토큰을 추가하여 CSRF 공격 방지
            <input type="hidden" th:name="${_csrf?.parameterName}" th:value="${_csrf?.token}" />
```

* CSRF(Cross-Site Request Forgery) 사이트간 요청 위조
* Thymeleaf의 템플릿 변수를 사용하여 폼에 CSRF 토큰을 포함
* 토큰을 사용하여 사용자가 로그인할 때 서버로 보내는 요청이 실제로 해당 사용자가 의도한 것인지 확인함
* Type을 hidden 설정 - 사용자는 보이지 않지만 요청이 서버로 전송될 때 함께 전송된다.

#### 로그아웃 구현하기

* UserApiController.java 에 logout() 메서드 추가하기
* articleList.html 에 \[로그 아웃] 버튼 추가

1. Controller/UserApiController.java 에 logout() 메서드 추가하기

```java
public class UserApiController {
( 생    략  )

    @GetMapping("/logout")
    public String logout(HttpServletRequest request, HttpServletResponse response) {

         //SecurityContextLogoutHandler()의 logout() 메서드 호출하여 로그아웃함
        new SecurityContextLogoutHandler().logout(request, response,            SecurityContextHolder.getContext().getAuthentication());

        return "redirect:/login";
    }
}
```

2. articleList.html 에 \[로그 아웃] 버튼 추가

```html
 (  생    략  )
//로그아웃 버튼 추가
<button type="button" class="btn btn-secondary" onclick="location.href='/logout'">로그아웃</button>
</div>

<script src="/js/article.js"></script>
</body>
```

#### 실행 테스트하기

* application.yml 환경변수 확인하기

```java
spring:
  jpa:
    show-sql: true
    properties:
      hibernate:
        format_sql: true
    defer-datasource-initialization: true
  datasource:
    url: jdbc:h2:mem:testdb
    username: sa
  h2:
    console:
      enabled: true

```

* http://localhost:8080/articles
* 회원 가입 화면에서 회원 가입
* 로그인 화면에서 로그인
* 블로그 기본 화면 확인
* 로그아웃 확인
