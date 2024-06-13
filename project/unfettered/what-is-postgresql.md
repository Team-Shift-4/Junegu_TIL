# What is PostgreSQL?

## PostgreSQL

PostgreSQL는 오픈 소스 관계형 데이터베이스 관리 시스템(RDBMS)이다. 이 시스템은 데이터를 안전하게 저장하고 관리하며 다양한 애플리케이션에 필요한 기능을 제공한다. PostgreSQL는 ANSI SQL 표준을 준수하며 확장성과 표준 준수성이 높은 데이터베이스로 평가받고 있다.

### 역사

PostgreSQL는 1986년 캘리포니아 대학교 버클리 캠퍼스에서 시작된 POSTGRES 프로젝트에서 유래했다. 원래 POSTGRES로 불리다가 1996년에 SQL 표준을 지원하기 시작하면서 PostgreSQL로 이름이 바뀌었다. 이후 여러 버전 업데이트를 통해 현재의 강력한 데이터베이스 시스템으로 발전해왔다.

### 주요 특징

1. **오픈 소스**: PostgreSQL는 자유롭게 사용, 수정, 배포할 수 있는 오픈 소스 소프트웨어이다. 이는 다양한 커뮤니티와 기업에서 광범위하게 사용되며 지속적으로 발전하고 있다.
2. **확장성**: 사용자 정의 함수, 데이터 타입, 연산자, 인덱스 메서드 등을 추가할 수 있어 다양한 요구에 맞게 확장 가능하다.
3. **표준 준수**: ANSI SQL 표준을 준수하며, 다양한 SQL 기능과 구문을 지원한다.
4. **다양한 데이터 타입 지원**: 정수, 부동 소수점 숫자, 문자열, 날짜와 시간, 배열, JSON, XML 등의 다양한 데이터 타입을 지원한다.
5. **고급 데이터 무결성**: 트랜잭션, 외래 키, 유니크 키, 체크 제약 조건 등을 지원하여 데이터 무결성을 보장한다.
6. **복제 및 클러스터링**: 스트리밍 복제, 논리적 복제 등을 통해 데이터베이스의 가용성과 확장성을 높일 수 있다.
7. **강력한 확장성 및 성능**: 대용량 데이터 처리에 적합한 아키텍처를 갖추고 있으며, 인덱스, 파티셔닝, 쿼리 최적화 기능을 통해 성능을 극대화할 수 있다.
8. **보안:** GSSAPI, SSPI, LDAP, SCRAM-SHA-256, 인증서 등 강력한 액세스 제어 시스템을 사용하고, 인증서 및 추가 방법을 사용한 다단계 인증을 사용한다.

### 주요 사용 사례

1. **웹 애플리케이션**: Django, Ruby on Rails, Node.js 등 다양한 웹 프레임워크와 호환되어 웹 애플리케이션에서 많이 사용된다.
2. **데이터 웨어하우징**: 데이터 분석 및 보고를 위한 데이터 웨어하우징 솔루션으로 사용된다.
3. **GIS 애플리케이션**: PostGIS 확장을 통해 지리 정보 시스템(GIS) 애플리케이션에서 사용된다.
4. **엔터프라이즈 애플리케이션**: 고가용성과 데이터 무결성이 중요한 대규모 엔터프라이즈 애플리케이션에서 널리 사용된다.

### 예제 쿼리

* **DATABASE CREATE**

```plsql
CREATE DATABASE example_db
```

* **TABLE CREATE**

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    position VARCHAR(100),
    salary NUMERIC
);
```

* **INSERT**

```sql
INSERT INTO employees (name, position, salary) 
VALUES ('Deepjune', 'Manager', 40000);
```

* **SELECT**

```sql
SELECT * FROM employees;
```

* **UPDATE**

```sql
UPDATE employees 
SET salary = 65000 
WHERE name = 'Deepjune';
```

* **DELETE**

```sql
DELETE FROM employees WHERE name = 'Deepjune';
```

