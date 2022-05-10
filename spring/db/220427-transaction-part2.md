## 문제점

### 애플리케이션 구조

<img width="481" alt="image" src="https://user-images.githubusercontent.com/56334513/167434301-b8f2ebfa-ca8b-41e0-9371-8cd5cd95e963.png">

#### 프레젠테이션 계층
+ UI와 관련된 처리 담당
+ 웹 요청, 응답
+ 사용자 요청 검증
+ 주 사용 기술: 서블릿과 HTTP 같은 웹 기술, 스프링MVC

#### 서비스 계층
+ 비즈니스 로직 담당
+ 주 사용 기술: 최대한 순수 자바 코드로

#### 데이터 접근 계층
+ 실제 데이터베이스에 접근
+ 주 사용 기술: JDBC, JPA, File, Redis 등

<hr>

**순수한 서비스 계층을 만들려면...**
+ 프레젠테이션 계층이나 데이터 접근 계층의 기술이 변경되도 비즈니스 로직은 최대한 변경 없이 유지.
+ 이로써 서비스 계층은 특정 계층에 종속되지 않으므로 비즈니스 로직을 유지보수하거나 테스트하기도 쉽다.

<br>

**트랜잭션이 서비스에 위치하게 된다면?**
+ DataSource, Connection, SQLException, 트랜잭션을 실행하기 위한 여러 코드들..

결국, **트랜잭션 문제**, **예외 누수 문제**, **JDBC 반복 문제**가 존재하게 된다.

### 트랜잭션 문제

1. JDBC 구현 기술이 서비스 계층에 누수되는 문제
2. 트랜잭션 동기화 문제
   - 같은 트랜잭션을 유지하기 위해 커넥션을 파라미터로 넘기는 것
   - 트랜잭션용 메서드와 트랜잭션을 유지하지 않는 메서드로 분리해야 하는 번거로움
3. 트랜잭션 적용 반복 문제
4. `setAutoCommit`, `commit`, `rollback`, 등등..

<br>

## 문제해결 1. 트랜잭션 추상화

#### 구현 기술에 따른 트랜잭션 사용법
+ 트랜잭션은 원자적 단위의 비즈니스 로직을 처리하는 데 사용
+ JDBC: `conn.setAutoCommit(false)`, JPA: `transaction.begin()`

### 스프링의 트랜잭션 추상화

인터페이스를 직접 만들어서 DI를 사용하면 OCP 원칙을 지킬 수 있다. 스프링 또한 같은 고민을 하게 됐고 `PlatformTransactionManager` 인터페이스를 제공하고 있다.
+ `getTransaction()`
  + 기존에 이미 진행중인 트랜잭션이 있는 경우, 해당 트랜잭션에 참여할 수 있다는 의미
+ `commit()`
+ `rollback()`

<br>

## 문제해결 2. 트랜잭션 동기화

스프링의 트랜잭션 매니저는 **트랜잭션 추상화**, **리소스 동기화** 역할을 한다. <br>

**리소스 동기화** - 이전에는 같은 커넥션을 동기화하기 위해 파라미터로 커넥션을 전달하는 방식을 채택했었음.

### 트랜잭션 매니저와 트랜잭션 동기화 매니저
<img width="398" alt="image" src="https://user-images.githubusercontent.com/56334513/167438979-c21a4300-6984-456c-ac5f-94763a0b648e.png">

+ 스프링은 트랜잭션 동기화 매니저를 제공. `ThreadLocal`을 사용해서 커넥션을 동기화하고 트랜잭션 매니저는 내부에서 이 트랜잭션 동기화 매니저를 사용.
+ 멀티쓰레드 상황에서 안전하게 커넥션을 동기화 가능. 커넥션이 필요하다면 트랜잭션 동기화 매니저를 통해서 커넥션을 획득하므로, 더이상 파라미터로 커넥션을 전달하지 않아도 됨

<br>

#### `DataSourceUtils.getConnection(DataSource dataSource)`

- 트랜잭션 동기화 매니저가 관리하는 커넥션이 있으면 해당 커넥션을 반환
- 관리하는 커넥션이 없는 경우, 새로운 커넥션을 생성해서 반환

#### `DataSourceUtils.releaseConnection(Connection connection, DataSource dataSource)`

- 커넥션은 트랜잭션을 종료(commit or rollback)할 때까지 반환해서는 안된다.
- 트랜잭션을 위한 커넥션은 닫히지 않고 그대로 유지됨
- 트랜잭션 동기화 매니저가 관리하는 커넥션이 없을 경우에 해당 커넥션을 닫음

<br>

### TransactionManager 사용

- `PlatformTransactionManager transactionManager`로 트랜잭션 매니저를 주입 받음.
- `transactionManager.getTransaction()`으로 트랜잭션을 시작
  - 트랜잭션 시작은 커넥션이 필요하므로 트랜잭션 매니저는 내부에서 dataSource를 통해 커넥션을 생성
- 수동 커밋 모드로 변경한 다음에 트랜잭션을 시작.
- 커넥션은 트랜잭션 동기화 매니저에 보관.
  - 트랜잭션 동기화 매니저는 `ThreadLocal`에 커넥션을 보관함으로써 멀티 쓰레드 환경에서도 안전함
- 리포지토리에서 `DataSourceUtils.getConnection(DataSource dataSource)`으로 트랜잭션 동기화 매니저로부터 커넥션을 꺼내 사용 (트랜잭션 유지)
- 로직이 끝나면 트랜잭션이 종료되면서 트랜잭션 동기화 매니저에서 사용하고 있는 커넥션을 획득해 커밋 또는 롤백
- **전체 리소스를 정리**
 + 트랜잭션 동기화 매니저 정리(쓰레드 로컬을 사용하면 리소스를 꼭 정리)
 + 자동 커밋 모드로 되돌린 후에 `close()`로 커넥션을 종료. 이 때 커넥션 풀을 사용하고 있다면 반환됨
   <br>

## 문제해결 3. `TransactionTemplate` (템플릿 콜백 패턴)

```java
public class Service {
    private final TransactionTemplate txTemplate;
    
    public Service(PlatfromTransactionManager transactionManager, repo) {
        // 인터페이스가 아닌 클래스이므로 별도의 인스턴스 생성
        this.txTemplate = new TransactionTemplate(transactionManager);
    }
    
    public void template() {
        txTemplate.executeWithoutResult((status) -> {
           try {
               //비즈니스 로직 위치
           } catch (SQLException e) {
               //...
           }
        });
    }
    
}
```
- 트랜잭션 시작, 커밋, 롤백의 코드가 모두 사라짐
- 언체크 예외가 발생하면 롤백, 체크 예외가 발생하면 커밋함.
- 하지만 트랜잭션을 처리하는 기술 로직이 아직까지 존재.
  - 비즈니스 로직과 기술 로직이 함께 공존하므로 두 관심사를 하나의 클래스에서 처리 -> 유지보수의 어려움

<br>

## 문제해결 4. 트랜잭션 AOP `@Transactional`

#### 프록시를 호출하게 되면? (프록시는 AOP와 연관)

<img width="483" alt="image" src="https://user-images.githubusercontent.com/56334513/167671817-0f38a731-86ed-4ad2-ac6e-4731083d293c.png">

스프링이 제공하는 AOP 기능을 사용하면 프록시를 매우 편리하게 적용 가능
+ 직접 작성해도 되지만 전세계가 공통적으로 사용할 만큼 매우 중요한 기능이므로 스프링 부트는 이를 제공해줌
+ `@Transactional`: 스프링의 트랜잭션 AOP가 이 어노테이션을 인식해서 트랜잭션 프록시를 적용함

<br>

스프링 기술이므로 스프링 컨테이너가 필요하다.
- `@Autowired`로 애플리케이션 구조에 붙이고, 설정 정보 클래스에 `DataSource`, `DataSourceTransactionManger`, 서비스, 레포지토리를 **Bean**으로 등록한다.
- 트랜잭션 AOP는 스프링 빈에 등록된 트랜잭션 매니저를 찾아서 사용.

실행 결과를 살펴보면 *memberService class=class hello.jdbc.service.MemberServiceV3_3$
$**EnhancerBySpringCGLIB**$$* 를 출력한다. (Repository는 AOP를 적용하지 않았기 때문에 일반 클래스를 출력) <br>

<br>

### 선언적 트랜잭션 관리 vs 프로그래밍 방식 트랜잭션 관리

+ **선언적 트랜잭션 관리**
  + `@Transactional` 하나만으로 편리하게 트랜잭션을 적용
  + 실무에서 선호하는 편
+ **프로그래밍 방식 트랜잭션 관리**
  + 트랜잭션 매니저 또는 트랜잭션 템플릿으로 트랜잭션 관련 코드를 직접 작성하는 방법
  + 거의 사용 X, 테스트 시에 가끔 사용

<br>

## 스프링 부트의 자동 리소스 등록

기존에는 DataSource와 TransactionManager를 스프링 빈으로 등록했어야 함. **하지만 스프링 부트가 출시되면서 자동화함.** <br>
+ `dataSource`, `transactionManager` 이름으로 된 스프링 빈 등록
+ 만약 직접 등록하면 스프링은 자동으로 등록하지 않음.

<br>

#### `application.properties` 속성을 참고

```properties
spring.datasource.url="~~~~";
spring.datasource.username=sa
spring.datasource.password=
```
- 스프링 부트에 기본으로 생성되는 dataSource는 **HikariDataSource**.
- `transactionManager`는 현재 등록된 라이브러리를 보고 판단함. JDBC 기술을 이용하면 `DataSourceTransactionManager`, JPA 기술을 이용하면 `JpaTransactionManager`.
  - Jpa 트랜잭션 매니저는 dataSource 트랜잭션 매니저가 제공하는 대부분 기능 지원