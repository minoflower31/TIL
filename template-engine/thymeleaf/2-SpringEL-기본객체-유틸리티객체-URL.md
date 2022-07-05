## SpringEL

+ Object, List, Map 등 다양하게 표현식을 사용할 수 있도록 제공

### Object
+ `user.username`
+ `user['username']` (동적으로 호출하기 좋음)
+ `user.getUsername()`

### List
+ `users[0].username`
+ `users[0]['username']`
+ `users[0].getUsername()`

### Map
+ `userMap['userA'].username`
+ `userMap['userA]['username']`
+ `userMap['userA'].getUsername()`

<br>

## 기본 객체

`${#request}`, `${#response}`, `${#session}`, `${#servletContext}`, `${#locale}` 같은 기본 객체를 제공함

### 편의를 위한 객체 제공

+ Http 요청 파라미터: param
  + `${param.paramData}`
+ Http 세션: session
+ 스프링 빈 접근: @
  + `${@helloBean.hello('Spring')}`

<br>

## 유틸리티 객체와 날짜

### 종류 모음
https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#expression-utilityobjects

### 예시
https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-b-expressionutility-objects

### 자바8 - 날짜 `#temporals`
`LocalTime`, `LocalDateTime`, `Instant`를 사용하려면 추가 라이브러리가 필요한데, 스프링 부트로 타임리프를 추가하면 자동으로 해당 라이브러리를 추가한다.

```java
...
model.addAttribute("localDateTime", LocalDateTime.now());
...
```
포맷 형식도 지원!
```thymeleafexpressions
<span th:text="{#temporals.format(localDateTime, 'yyyy-MM-dd HH:mm:ss')"></span>
```

<br>

## URL 링크

`@{...}`

### Query Parameter
+ `@{/hello(param1=${param1}, param2=${param2})}`
+ 경로 내에 괄호가 있으면 쿼리 파라미터로 처리

### Path Variable
+ `@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}`
+ 경로에 중괄호로 표현돼있으면 이후의 괄호는 path variable로 처리
### Path Variable  + Query Parameter 
+ `@{/hello/{param1}(param1=${param1}, param2=${param2})}`
