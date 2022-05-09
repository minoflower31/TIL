# 예외 처리, 반복을 해결한다.

## 체크예외와 인터페이스

서비스 계층은 특정 구현 기술에 의존하지 않고, 순수하게 유지하는 것이 좋음. <br>
서비스가 `SQLException`을 처리하려면, 이 체크 예외를 런타임 체크 예외로 전환해서 서비스 계층에 던지는 것.

### 특정 기술에 종속되는 인터페이스

`SQLException`과 같은 특정 구현 기술에 종속적인 체크 예외를 사용하면, 인터페이스 또한 해당 예외를 포함해야 한다.

```java
public class RepositoryIml implements Repository {
    public Member save(Member member) throws SQLException {
        ...
    }
}

interface Repository {
    Member save(Member member) throws SQLException
}
```

이는 순수한 인터페이스가 아니기 때문에 향후에 다른 데이터 접근 기술을 이용하면 인터페이스 자체를 변경해야 함.<br>
런타임 예외는 따로 선언해두지 않아도 되기 때문에 **런타임 예외**를 적용해야 함. <br>
(RuntimeException을 상속받은 사용자 정의 예외 생성)

<br>

### 예외 변환

```java
catch(SQLException e) {
    throw new MyDbException(e);
}
```
기존 예외에 포함된 예외를 확인할 수 있기 때문에 꼭! 기존 예외를 인자로 전달해야 한다.

<br>

## 데이터 접근 예외를 직접 만들어보기

데이터베이스 오류에 따라 특정 예외는 복구하고 싶을 때
+ e.g. 회원 가입할 때 기존의 아이디가 있는 경우, 뒤에 임의의 숫자를 붙여 새로운 ID를 만들어야 할 때
+ 이 로직은 데이터베이스에서 오류 코드를 반환하고, JDBC 드라이버는 이 오류 코드를 받아 `SQLException`을 던진다.
+ `e.getErrorCode() == 23505 // H2 데이터베이스의 중복 오류 코드`
+ 하지만 이 오류 코드를 확인하기 위해서는 서비스 계층에서 SQLException을 의존해야 한다. 이를 해결하려면 런타임 예외로 감싸서 던지면 된다.
+ 이렇게 되면 JDBC 같은 데이터 접근 기술에 종속적임을 방지함. (향후 다른 기술로 바뀌더라도 전혀 무관함)

### (남은 문제)

SQL ErrorCode는 각 데이터베이스마다 다르므로, 데이터베이스를 변경할 때마다 ErrorCode를 모두 변경해야 함. <br>
키 중복 예외, SQL 문법 예외, LOCK 예외 등등을 직접 다 만들 수가 없을 뿐더러 데이터베이스마다 오류 코드를 반영시켜줘야 함

<br>

## 스프링 예외 추상화

<img width="479" alt="image" src="https://user-images.githubusercontent.com/56334513/167354875-0441bc8c-0982-455d-afe0-b35edb405a39.png">

스프링이 제공하는 예외 추상화를 통해, 서비스 계층에서도 이를 사용하면 된다. (각 데이터베이스에 맞는 예외를 스프링이 제공하는 예외로 변환해주는 역할 또한 스프링이 담당)
<br>

`DataAccessException`은 크게 2가지로 구분
+ `Transient`: 일시적. 하위 예외들은 동일한 SQL을 다시 시도했을 때 성공할 가능성 존재
+ `NonTransient`: 일시적이지 않음. SQL 문법 오류가 있는 SQL을 다시 시도하면 실패함.

<br>

### 스프링이 제공하는 예외 변환기

스프링은 데이터베이스에서 발생하는 오류 코드를 스프링이 정의한 예외로 자동 변환해주는 `변환기`를 제공
```java
SQLExceptionTranslator exTranslator = new SQLErrorCodeSQLExceptionTranslator(dataSource);
DataAccessException resultEx = exTranslator.translate("select", sql, e);
```
- translate의 첫 번째 파라미터: 설명 / 두 번째 파라미터: sql / 세 번째 파라미터: 예외
- 반환 타입이 `DataAccessException`이지만, 상황에 따른 예외를 반환해준다. (DataAccessException을 상속받고 있으므로)
- 오류 코드는 어떻게 해결했을까?
  + **sql-error-codes.xml** 파일에 error code를 대입해서 DB에 맞는 스프링 데이터 접근 예외를 찾음

스프링이 제공하는 예외를 사용하기 때문에 스프링에 대한 기술 종속성은 발생하지만, 순수하게 만드려면 예외를 모두 직접 정의할 뿐더러 예외 변환도 직접 다 작성해야 한다. <br>
그렇기 때문에 개발자는 `trade-off`를 잘 판단해야 한다.

```java
private final DataSource dataSource;
private final SQLExceptionTranslator exTranslator;

public class MemberRepositoryV4_2 implements MemberRepository {
    public MemberRepositoryV4_2(DataSource dataSource) {
        this.dataSource = dataSource;
        this.exTranslator = new SQLErrorCodeSQLExceptionTranslator(dataSource);
    }
    
    @Override
    public Member save(Member member) {
        //...
        
        try {
            
        } catch (SQLException e) {
            throw exTranslator.translate("save", sql, e);
        }
    }
}
```

<br>

### 서비스 계층의 순수성

1. 트랜잭션 추상화 + 트랜잭션 AOP로 순수성을 최대한 유지
2. 스프링이 제공하는 예외 추상화와 예외 변환기로 데이터 접근 기술이 변경되어도 예외 사용 가능
3. 리포지토리 인터페이스에 의존한 덕분에 다른 데이터 접근 기술로 변경되어도 순수하게 유지

<br>

## JDBC 반복 문제 해결 : JDBCTemplate

메서드마다 반복되는 코드

+ 커넥션 조회, 커넥션 동기화, PreparedStatement 생성 및 파라미터 바인딩
+ 쿼리 실행, 결과 바인딩, 예외 발생 시 스프링 예외 변환기 생성
+ 리소스 종료

```java
private final JdbcTemplate template;

public MemberRepositoryV5(DataSource dataSource) {
    template = new JdbcTemplate(dataSource);
}

@Override
public Member save(Member member) {
    String sql = "insert into member(member_id, money) values(?,?)";
    template.update(sql, member.getMemberId(), member.getMoney());
    return member;
}

@Override
public Member findById(String memberId) {
    String sql = "select * from member where member_id = ?";
    return template.queryForObject(sql, memberRowMapper(), memberId);
}

private RowMapper<Member> memberRowMapper() {
    return (rs, rowNum) -> {
        Member member = new Member();
        member.setMemberId(rs.getString("member_id"));
        member.setMoney(rs.getInt("money"));
        return member;
    };    
}
```

**반복 문제를 해결할 뿐만 아니라 트랜잭션을 위한 커넥션 동기화, 스프링 예외 변환기도 자동으로 실행해줌**