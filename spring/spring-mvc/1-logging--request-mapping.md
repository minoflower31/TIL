> 패키징 설정
> + `Jar`를 선택해야 하는 이유
>   + 내장 서버를 사용하는데 최적화되어 있는데, 톰캣 서버를 내장으로 사용하며 webapp 경로를 사용하지 않는 특징이 있음.
>   + `War` 또한 내장 서버를 사용할 수 있지만 **외부 서버에 배포하는 목적**이 큼

<br>

## Logging

### 로깅 라이브러리

+ 스프링 부트 라이브러리를 사용하게 되면 `spring-boot-starter-logging`이 포함되어 로그 기능을 제공받는다.
+ `Logback, Log4J, Log4J2` 등 수많은 로그 라이브러리가 존재하는데, 이러한 로그들을 편리하게 사용할 수 있도록 인터페이스로 설정한 `Slf4j` 라이브러리가 있다.
+ 스프링 부트는 인터페이스인 `Slf4j`를 제공하고 대부분 `Logback`을 구현체로 사용한다.

<br>

> 로그를 선언하는 방법
+ `private Logger log = LoggerFactory.getLogger(getClass)`
+ (Lombok 사용 시) `@Slf4j`

<br>

> 로그 테스트

<img width="925" alt="image" src="https://user-images.githubusercontent.com/56334513/170638435-2531d65d-e053-4844-9359-ba1c1ef3c4ed.png">

+ 시간, 로그 레벨, 프로세스 ID, 쓰레드 명, 클래스 명, 로그 메시지가 담겨있음.
+ 레벨은 `TRACE > DEBUG > INFO > WARN > ERROR` 순이다.
  + 개발 서버에서는 `debug`부터 출력하고, 운영 서버에서는 `info`부터 출력한다.

<br>

> 로그 레벨 설정

+ `application.properties`에 설정
  + `logging.level.{companyName}.{pakageName}=debug` // 해당 패키지는 debug부터 출력한다는 의미
  + `logging.level.root=debug` // 전체적으로 debug부터 출력(default는 info)

<br>

> 올바른 로그 사용법
+ `log.debug("data={}", data)`
  + 만약 `"data="+data`로 찍는다면 새로운 String 객체를 생성해 더한 값을 할당하게 되므로 심한 메모리 낭비 

### 로그의 장점

1. `쓰레드 정보나 클래스 이름` 같은 정보를 확인할 수 있다.
2. 로그 레벨에 따라서 개발 서버에서는 개발에 필요한 정보를 출력하고, 운영 서버로 전환하면 개발 정보 로그를 출력하지 못하게 막을 수 있다.
3. 시스템 콘솔 뿐만 아니라, 파일 형태로도 저장 가능하다. (심지어 날짜별, 파일 크기에 따른 분할도 가능)
4. `System.out.println()` 사용할 때 발생하는  내부 버퍼링이나 멀티 쓰레드 문제에 대해 대처할 수 있다.

<br>

## 요청 매핑

### `RequestMapping`을 상속받은 매핑을 사용하자.

#### @GetMapping, @PostMapping, @PutMapping, @DeleteMapping, @PatchMapping

### 특정 경로를 설정할 때는 `@PathVariable`

```java
@GetMapping("/users/{userId}")
public String findUser(@PathVariable String userId) {
    //...
}
```
+ 경로에 식별자를 넣을 수 있는 이유
  + `@RequestMapping`은 URL 경로를 템플릿화하는데, `@PathVariable`로 매칭되는 부분을 설정할 수 있음

+ 다중으로도 사용 가능!
  + `@GetMapping("/users/{userId}/orders/{orderId}")` 

### 특정 헤더 조건 매핑

### 미디어 타입 조건 매핑

#### HTTP 요청 Content-Type. `consume`
+ `@PostMapping(value = "...", consumes = "application/json")`
+ Content-Type 헤더 정보를 기반으로 매핑

#### HTTP 요청 Accept. `produce`
+ `@PostMapping(value = "...", consumes = "application/json")`
+ Accept 헤더 정보를 기반으로 매핑
