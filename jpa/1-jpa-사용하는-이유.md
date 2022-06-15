## SQL 중심적인 개발에 대한 문제점

### 무한 반복, 지루한 코드
`CRUD`, 자바 객체 -> SQL로, SQL -> 자바 객체로

### CRUD를 작성할 때 나타나는 문제점
객체에 필드가 추가되면?
+ insert문을 작성할 때 추가된 필드를 넣어야 함..

<br>

## 객체와 관계형 데이터베이스의 차이

### 1. 상속

객체는 `상속` 관계로, 테이블은 `슈퍼타입-서브타입` 관계로 표현한다. <br>
만약 Item - Album 관계가 주어질 때 <br>
+ Album을 추가한다면?
  + `insert into item`, `insert into album` 으로 쿼리문을 작성해야 한다.
+ Album을 조회한다면?
  + 테이블을 조인하는 쿼리문, item, Album 객체를 생성해서 값 대입하기.. 등등

### 2. 연관관계

+ 객체는 **참조**(`member.getTeam()`)를 사용하지만, 테이블은 **외래키**(`JOIN ON m.team_id = t.team_id`)를 사용한다.
+ 테이블은 기본키와 외래키가 존재하므로 양방향으로 이동할 수 있지만, 객체는 한 곳에서만 참조하므로 참조한 쪽에서만 단방향으로 이동 가능하다.

테이블에 맞춘 객체의 모습
```java
class Member {
    String id;
    Long teamId;
    String username;
}

class Team {
    Long id;
    String name;
}
```
하지만 Team 객체를 참조할 수는 없다. 객체지향 다운 모델링은 다음과 같다.

```java
class Member {
    String id;
    Team team; // 참조로 연관관계를 맺은 모습
    String username;

    public Team getTeam() {
        return team;
    }
}

class Team {
    Long id;
    String name;
}
```

이 뿐만 아니라, 객체는 `그래프 탐색`에 자유로워야 한다. <br>

### 3. 엔티티 신뢰 문제
> **모든 객체를 미리 로딩할 수는 없다.**
> + `member.getTeam()` `member.getOrder().getDelivery()`
> + 위의 값을 가져야 한다면 관련된 쿼리문을 모두 작성해서 실행해야 한다.

그렇기 때문에 상황에 따른 메서드를 미리 생성해놓아야 한다.
+ `memberDAO.getMember();`
+ `memberDAO.getMemberWithTeam();`
+ `memberDAO.getMemberWithOrderWithDelivery();`

### 4. 데이터 식별 방법

데이터 접근 계층에서 특정 데이터를 조회하는 경우를 살펴보자.

```java
class MemberDAO {
    public Member getMember(String memberId) {
        //... query
        return new Member(memberId, ...)
    }
}
```
항상 새로운 객체를 생성하므로 동일한 memberId로 조회할지라도 참조 주소가 다르기 때문에 값이 다르다.

<br>

**위에 언급한 저장과 조회같은 경우를 `컬렉션`에 저장하고 조회하듯이 DB에 저장한다면 정말 간편할 것이다.** 