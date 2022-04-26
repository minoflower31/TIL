# Connection Pool 이해

### DB 커넥션을 매번 획득한다.

<img width="582" alt="image" src="https://user-images.githubusercontent.com/56334513/165078872-9f550464-8907-4fb3-9f34-cff15201ae91.png">

1. 애플리케이션 로직은 DB 드라이버로 커넥션을 조회
2. DB 드라이버는 DB와 TCP/IP 커넥션을 연결. 3-way-handshake와 같은 네트워크 동작 발생
3. DB 드라이버는 TCP/IP 커넥션이 연결되면 URL,USERNAME,PASSWORD를 DB에 전달
4. DB는 전달된 내용으로 내부 인증을 마치고 내부에 DB 세션을 생성
5. DB는 커넥션 생성이 완료됐다는 응답 보냄
6. DB 드라이버는 커넥션 객체를 생성해서 클라이언트에 반환

이와 같이 상당히 복잡하고 시간도 많이 소모될 수 밖에 없음<br>
DB는 물론이고 애플리케이션 서버에서도 TCP/IP 커넥션을 새로 생성하기 위해 그때마다 리소스 생성<br>
**문제점**<br>
고객은 애플리케이션을 사용하면서 SQL을 실행하는 시간, 커넥션을 새로 만드는 시간까지 추가되므로 응답 속도에 영향을 줌. <br>
<br>

> c.f. 데이터베이스마다 커넥션을 생성하는 시간이 다름. MySQL은 `수 ms` 정도로 매우 빠르게 생성하지만, `수십 ms` 이상 걸리는 데이터베이스도 존재한다.

<br>

## Connection Pool

**커넥션을 관리하는 풀. 미리 커넥션을 생성해두고 사용함** <br>

<img width="580" alt="image" src="https://user-images.githubusercontent.com/56334513/165080271-3a65a57f-0a57-4e24-b9c7-55516ddeb2e4.png">

애플리케이션을 시작하는 시점에 커넥션 풀은 필요한 만큼의 커넥션을 미리 생성해서 보관해놓음. (default = 10) <br>
커넥션 풀 안에 들어있는 커넥션들은 이미 TCP/IP로 DB와 연결돼있는 상태이므로 즉시 SQL을 DB에 전달 가능

커넥션 풀이 아직 생성되지 않았을 때 커넥션을 획득하려고 시도하면 pool이 생성될 때까지 기다린다. (이것은 시간이 약간 걸릴수 있다.)
<br>

- 이제는 매번 새로운 커넥션을 생성하는 것이 아닌 이미 생성된 커넥션을 객체 참조로 가져다 씀
- 커넥션 풀에 커넥션을 요청 시, 자신이 가지고 있는 커넥션 중에 하나를 반환
- 커넥션 사용이 끝나면 종료시키는 것이 아닌 재사용할 수 있도록 커넥션 풀에 반환하게 됨. (커넥션이 살아있는 상태에서 반환)
- 서비스의 특징, 애플리케이션 서버의 스펙, DB 서버의 스펙에 따라 다르므로 성능 테스트를 통해 커넥션 수를 정해야 함
- 커넥션 풀은 서버 당 최대 커넥션 수를 제한할 수 있으므로 DB를 보호하는 효과도 있음 (무한정 생성 방지)
- **실무에서는 항상 기본으로 사용**
- 대표적인 커넥션 풀 오픈 소스: `commons-dbcp2`, `tomcat-jdbc pool`, 'HikariCP' 등
- 스프링 부트 2.0부터 HikariCP를 기본 제공. 성능, 사용의 편리함, 안전성 측면에서 검증됨

<br>

# DataSource 이해

커넥션을 획득하는 방법은 JDBC의 `DriverManager`를 사용하거나 커넥션 풀을 사용하는 등의 다양한 방법 존재 <br>

- DriverManager를 사용하다가 커넥션 풀을 사용하는 방법으로 변경했을 경우
    - 커넥션을 획득하는 애플리케이션 코드도 함께 변경
    - 의존관계가 DriverManager에서 HikariCP로 변경됐기 때문. 사용법조차 다름 자바에서 이를 해결하기 위해 `javax.sql.DataSource` 인터페이스 제공
      <br>

#### DataSource란 커넥션을 획득하는 방법을 추상화하는 인터페이스

- 대부분의 커넥션 풀은 DataSource 인터페이스를 구현함. 이제는 커넥션 생성 방법에 직접 의존할 필요가 없음.
- 커넥션 풀 변경 시, 구현체만 갈아끼우면 됨
- 스프링은 `DriverManagerDataSource` 클래스를 제공. 기본 DriverManager는 DataSource를 구현하고 있지 않기 때문

<br>

### 기본 DriverManager와 DriverManagerDataSource의 차이점

#### : 파라미터

**DriverManager**는 `DriverManager.getConnection(URL, USERNAME, PASSWORD)`로 커넥션을 생성할 때마다 정보를 넣음. **DataSource**
는 `DriverManagerDataSource dataSource = new DriverManagerDataSource(URL, USERNAME, PASSWORD`로 참조 변수를 만들고 필요할
때마다 `getConnection()`을 호출하면 됨

#### 설정과 사용의 분리

- **설정**: DataSource를 만들고 필요한 속성을 입력하는 것. 설정과 관련된 속성들이 한 곳에 있으면 유연하게 대처 가능
- **사용**: DataSource의 getConnection()만 호출해서 사용
- 곳곳에서 getConnection()을 사용해도 설정 부분은 전혀 신경쓰지 않아도 됨.
- 즉, 레포지토리는 DataSource에만 의존하면 됨.

<br>

## 커넥션 풀 사용

```java
HikariDataSource dataSource=new HikariDataSource();
        dataSource.setJdcUrl(URL);
        dataSource.setUsername(USERNAME);
        dataSource.setPassword(PASSWORD);
        dataSource.setMaximumPoolSize(10);
        dataSource.setPoolName("MyPool");

        dataSource.getConnection();

        Thread.sleep(1000); // 커넥션 풀에서 커넥션 생성 시간 대기
```

커넥션 풀에서 커넥션을 생성하는 작업은 애플리케이션 실행 속도에 영향을 주지 않기 위해 별도의 스레드에서 작동. 그래서 테스트를 돌리면 모든 커넥션들이 생성되기 전에 테스트가 종료해버림.

+ 커넥션 풀에 커넥션을 생성하는 것은 상대적으로 오래 걸림.
+ 애플리케이션을 실행할 때 생성이 마칠때까지 대기하면 실행 시간이 늦어짐

c.f. 커넥션 최대 수를 가진(=10) 상태에서 하나(10+**1**)를 더 획득했을 경우?

+ Hikari 내부 함수로 time out 시간을 조절하는 등에 대한 기능들이 존재. 이를 적절히 사용
+ Hikari github 참조

<br>

## DataSource 적용

```java

@Slf4j
public class MemberRepository() {
    private final DataSource dataSource;

    //의존 관계 주입
    //DriverManagerDataSource -> HikariDataSource 변경해도 상관없음.
    public MemberRepository(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    private Connection getConnection() throws SQLException {
        Connection conn = dataSource.getConnection();
        log.info("get connection={}, class={}", conn, conn.getClass());
        return conn;
    }

    private void close(Connection conn, Statement stmt, ResultSet rs) {
        JdbcUtils.closeResultSet(rs);
        JdbcUtils.closeStatement(stmt);
        JdbcUtils.closeConnection(conn);
    }
}
```
<br>

### DriverManagerDataSource 사용
```text
get connection=conn0: url=jdbc:h2:.. user=SA class=class
org.h2.jdbc.JdbcConnection
get connection=conn1: url=jdbc:h2:.. user=SA class=class
org.h2.jdbc.JdbcConnection
...
```
- `conn0~5` 번호로 항상 새로운 커넥션을 생성해서 사용

### HikariDataSource 사용
```text
get connection=HikariProxyConnection@xxxxxxxx1 wrapping conn0: url=jdbc:h2:...
user=SA
get connection=HikariProxyConnection@xxxxxxxx2 wrapping conn0: url=jdbc:h2:...
user=SA
...
```
- `conn0` 커넥션을 재사용
- 로직을 순서대로 시행하기 때문에 커넥션 사용과 반환을 반복함.
- 웹 애플리케이션에서 동시에 여러 요청이 들어오면 여러 스레드에서 커넥션 풀의 커넥션을 가져가는 상황을 확인할 수 있음.
- 히카리가 conn0을 사용하고 반환하고를 반복하지만, ProxyConnection의 주소값은 모두 다르다. wrapping의 과정을 거치기 때문.

<br>
<br>

참고: 인프런 "스프링 DB 1편 - 데이터 접근 핵심 원리" https://inf.run/D8N6