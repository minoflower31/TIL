##  트랜잭션 이해

해석 그대로 **거래**를 의미한다. 데이터베이스에서의 트랜잭션이란 하나의 거래를 안전하게 처리하도록 보장해주는 것. <br>

Situation. A -> B에게 5000원을 계좌이체
1. A의 잔고 = 5000원 감소
2. B의 잔고 = 5000원 증가

두 단계에 걸쳐서 진행되는 하나의 작업이다. 하지만 1번은 성공했으나 2번에서 전산 시스템 장애로 실패한다면? 1번의 돈만 줄어들게 된다. <br>
트랜잭션을 사용하게 되면 1,2번이 모두 성공해야 저장하고, 만약 도중에 실패 시 거래 이전의 상태로 돌아간다.<br>
모든 작업이 성공하면 `commit`으로 정상 반영하고, 도중에 실패 시 `rollback`으로 이전 상태로 되돌린다.<br>
<br>

### 트랜잭션 ACID

ACID를 모두 보장해야 한다.

- **원자성(Atomicity)**: 트랜잭션 내에서 실행되는 작업은 마치 하나의 작업처럼 모두 성공하거나 모두 실패해야 한다.
- **일관성(Consistency)**: 모든 트랜잭션은 일관성 있는 데이터베이스 상태를 유지해야 한다.
  - e.g. 데이터베이스에서 정한 무결성 제약 조건을 항상 만족해야 한다.
- **격리성(Isolation)**: 동시에 실행되는 트랜잭션이 서로에게 영향을 미치지 않도록 한다.
  - e.g. 동시에 같은 데이터를 수정하지 못하도록 해야 한다. 격리성은 동시성과 관련된 성능 이슈로 인해 트랜잭션 격리 수준(Isolation level)을 선택할 수 있다.
- **지속성(Durability)**: 트랜잭션을 성공적으로 끝내면 그 결과가 항상 기록되어야 한다. 중간에 시스템 문제가 발생해도 로그 등을 이용해서 성공한 트랜잭션 내용을 복구해야 한다.

트랜잭션은 원자성, 일관성, 지속성을 보장하지만 격리성은 트랜잭션을 거의 순서대로 실행하지 않은 이상 힘들다. 보장한다해도 동시 처리 성능이 매우 떨어진다. 그래서 트랜잭션 격리 수준을 정의했다. <br>
<br>

#### 트랜잭션 격리 수준

- READ UNCOMMITTED(커밋되지 않은 읽기)
- READ COMMITTED(커밋된 읽기)
- REPEATABLE READ(반복 가능한 읽기)
- SERIALIZABLE(직렬화 가능)

c.f. 아래로 갈수록 성능이 떨어진다. 보통은 READ COMMITTED를 사용한다. + JPA 책 참조 <br>

<br>

## 데이터베이스 연결 구조와 DB 세션

<img width="543" alt="image" src="https://user-images.githubusercontent.com/56334513/165317685-cbc15c0d-b275-4233-b5be-d0714d3dc399.png">

- 커넥션을 맺을 때 DB 서버는 내부에 세션을 만든다. 커넥션을 모든 요청은 세션을 통해 실행된다.
  - 개발자가 클라이언트를 통해 SQL 전달 -> 현재 커넥션에 연결된 세션이 SQL 실행
  - 트랜잭션이 종료되면 이후에 새로운 트랜잭션을 다시 시작할 수 있음
  - 사용자가 커넥션을 닫거나, DBA가 세션을 강제 종료하면 세션은 종료됨
  - (커넥션마다 하나의 세션이 만들어짐)

<br>

## 트랜잭션 - 전체 흐름

#### 트랜잭션 사용

- **커밋을 호출하기 전까지는 임시로 데이터를 저장**
- `commit`, `rollback`
- 사용자 A의 세션에서는 변경, 사용자 B의 세션에서는 변경 X

<img width="543" alt="image" src="https://user-images.githubusercontent.com/56334513/165318840-a41cff85-a2b0-4af4-a3d5-2e03bc872aae.png">

- 만약 커밋하지 않은 데이터를 다른 곳에서 조회한다면?
  - 세션1에서 `rollback`을 해버리면 추가한 데이터가 모두 사라지므로 **데이터 정합성**에 문제 발생.
  - `commit`을 수행하면 상태는 **임시 -> 완료**로 바뀌며 이제서야 세션2에서도 변경된 데이터 확인 가능
- 추가 뿐만 아니라 수정, 삭제 또한 `rollback` 시에 시작 직전의 상태로 복구됨

<br>

## 트랜잭션 - 자동 커밋, 수동 커밋

### 자동 커밋

쿼리 실행 직후에 자동으로 커밋을 호출. 따라서 커밋이나 롤백을 직접 호출하지 않아도 되는 장점이 있지만, 쿼리 하나하나 실행할 시 자동으로 커밋되어 완전한 트랜잭션 수행이 불가능.
```h2
set autocommit true;
insert into member(id, money) values ('memberA',10000); // commit
insert into member(id, money) values ('memberB',10000); // commit
```

### 수동 커밋

**수동 커밋으로 설정하는 것을 트랜잭션을 시작한다고 표현.** 보통은 autocommit이 true로 설정돼있기 때문.
```h2
set autocommit false;
insert into member(id, money) values ('memberA',10000);
insert into member(id, money) values ('memberB',10000);
commit;
```

- `autocommit`을 설정하면 세션에서 계속 유지됨. 중간에 변경도 가능

<br>

## h2 데이터베이스를 활용한 트랜잭션

- 강의 자료 참고하기

<br>
<br>

## DB Lock 이해

Situation. 세션1이 트랜잭션을 시작하고 데이터를 수정할 때(커밋X) 세션2에서 같은 데이터를 수정한다면?
- 트랜잭션의 원자성, 격리성 위배.

**세션이 트랜잭션을 시작하고 데이터를 수정하는 동안에는 `commit`이나 `rollback` 전까지 다른 세션에서 해당 데이터를 수정할 수 없게 한다.** <br>
<br>

<img width="546" alt="image" src="https://user-images.githubusercontent.com/56334513/165323233-2ffd43b6-aed2-40da-a9c6-4c412e9f1414.png">

- 4에서 5로 넘어갈 때 lock 획득 시도는 대기 상태가 됨. 비로소 세션1에서 `commit`하면 대기 상태에서 획득 상태로 변경되서 이후 수행.

<br>

### Lock Timeout

```java
SET LOCK_TIMEOUT 10000;
set autocommit false;
update member set money=1000 where id = 'memberA';
```
- `SET LOCK_TIMEOUT <millisecends>`: 락 타임아웃 시간 설정
- 10000은 10초. 이렇게 설정했을 때 10초동안 대기해도 lock을 얻지 못하면 락 타임아웃 오류 발생.
- lock을 획득한 세션이 `commit`이나 `rollback`을 수행할 때까지 대기.

<br>

### 조회

- **일반적인 조회는 락을 사용하지 않음**. 단, DB마다 다를 수 있음.
- 조회하면서 lock을 획득하고 싶다면 `select {sql} for update` 구문 사용
  - 언제 사용해?
    - 트랜잭션 종료 시점까지 해당 데이터를 다른 곳에서 변경하지 못하도록 강제로 막아야 할 때
      - e.g. 조회하고 나서 중요한 계산을 위해 데이터 변경을 막아놓음

<br>
<br>

## 트랜잭션 적용

- 강의 자료 참고

### 비즈니스 로직과 트랜잭션

- 트랜잭션은 비즈니스 로직이 있는 서비스 계층에서 시작. 비즈니스 로직에 문제가 생기면 해당 비즈니스 로직에 비롯된 문제들을 함께 Rollback해야 함.
- 서비스 계층에서 커넥션을 만들고 트랜잭션 커밋 이후에 커넥션 종료해야 함.
- 즉, **트랜잭션을 사용하는 동안 같은 커넥션을 유지**
- 비즈니스 로직에 사용되는 함수들의 인자로 Connection 전달.

```java
    Connection con = dataSource.getConnection();
    try {
    con.setAutoCommit(false); //트랜잭션 시작
    bizLogic(con, fromId, toId, money); //비즈니스 로직
    con.commit(); //성공시 커밋
    } catch (Exception e) {
        con.rollback(); //실패시 롤백
        throw new IllegalStateException(e);
    } finally {
        release(con); //커넥션 종료
    }
```

### 문제점

애플리케이션 내에 DB 트랜잭션 적용 시 서비스 계층의 코드가 매우 복잡해짐. 뿐만 아니라 커넥션을 유지하기 위해 코드를 변경하는 것도 복잡함. <br>
> 스프링으로 해결!

<br>
<br>

참고: 인프런 "스프링 DB 1편 - 데이터 접근 핵심 원리" https://inf.run/D8N6