# 스프링 mvc가 HTTP 요청을 조회하는 방법

## HTTP 요청 - 헤더 조회

```java
public String headers(HttpServletRequest request,
        HttpMethod httpMethod,
        ...){}
```

### 기본으로 조회할 수 있는 것들

HttpServletRequest, HttpServletResponse, HttpMethod, Locale ...

### 헤더를 조회하는 방법들

> 모든 HTTP 헤더 조회

`@RequestHeader MultiValueMap<String,String> headerMap`

+ 하나의 key에 여러 value를 조회할 수 있음 (e.g. keyA=valueA&keyA=valueA)

<br>

> 특정 HTTP 헤더 조회

`@RequestHeader("host") String host`

<br>

> 특정 쿠키 조회

`@CookieValue(value = "myCookie", required = false) String cookie`

+ required: 필수 값 여부

<br>

## HTTP 요청 파라미터: 쿼리 파라미터, HTML Form

### 1. HttpServletRequest, HttpServletResponse 이용하기

서블릿에서 했던 방식과 동일

<br>

### 2. @RequestParam 이용하기

```java
@ResponseBody
@RequestMapping()
public String requestParam(@RequestParam String username)
```

+ `@RequestParam`은 파라미터 이름으로 바인딩하는 기능
    + @RequestParam("username") == request.getParameter("username")
    + 기본 자료형(e.g. `int`)이나 참조 자료형(e.g. `Integer`)처럼 단순 타입일 때 `@RequestParam` 생략 가능! (비추천)

<br>

+ `@ResponseBody`는 View 조회를 하지 않고 HTTP 메시지 바디에 직접 작성하겠다는 기능

### 파라미터 필수 여부

`@RequestParam(required = false` (default=true)

+ 만약 `?username=`처럼 공백이 들어간다면? 빈문자로 통과됨
+ 이를 방지하기 위한 2가지 방법
    1. 속성으로 `defaultValue="guest"`와 같이 설정
    2. `int`형이 아닌 `Integer`를 사용하여 null을 받도록 함

### `Map`으로 파라미터 조회하기

`@RequestParam Map<String,Object> paramMap`

+ 특정 파라미터의 값이 2개 이상일 경우 `MultiValueMap`을 사용하긴 하나, 거의 사용할 일 없음

<br>

### 3. @ModelAttribute 이용하기

요청 파라미터를 받아서 필요한 객체를 생성한 후, 그 객체에 값을 넣어주는 로직을 자동화해주는 기능. 심지어 Model 객체의 `addAttribute` 기능을 수행함

`@ModelAttribute HelloData helloData`

+ 생략 가능(기본형argument resolver로 지정해둔 타입 외의 모든 것)
+ @ModelAttribute 동작 방식
    1. 객체를 생성
    2. 요청 파라미터들의 이름으로 객체의 프로퍼티를 찾음.
    3. 해당 프로퍼티의 `setter`를 호출해서 파라미터의 값을 입력(바인딩)함
        + 프로퍼티를 가진다는 뜻은, **멤버 변수에 대한 getXXX, setXXX가 존재하면 해당 멤버변수는 프로퍼티를 가진다고 볼 수 있다.**

<br>

`BindException`

+ 숫자가 들어갈 곳에 문자가 들어가면 발생하는 예외. 이는 **검증**으로 처리

## HTTP 요청 메시지: 단순 텍스트, JSON

## 단순텍스트

### 1. HttpEntity 사용 (RequestEntity, ResponseEntity)

+ 헤더와 바디 정보를 편리하게 조회한다. (요청 파라미터와 관계없음)
+ `HttpMessageConverter`에 의해 실행됨
+ `RequestEntity`: HttpMethod, url 정보
+ `ResponseEntity`: Http 상태 코드 설정, 헤더 정보 추가 가능
```java
@PostMapping
public HttpEntity<String> requestBodyString(HttpEntity<String> httpEntity){
        String messageBody = httpEntity.getBody();
        return new HttpEntity<>("ok");
}
```

### 2. @RequestBody 사용

```java
@ResponseBody
@PostMapping
public String requestBodyString(@RequestBody String messageBody) {
        return "ok";
}
```
+ 헤더 정보가 필요하다면 `HttpEntity`를 사용하거나, `@RequestHeader`를 이용함.

> 여기서 정리!
> + 요청 파라미터 조회: @RequestParam, @ModelAttribute
> + Http 메시지 바디 조회: @RequestBody, @HttpEntity 

<br>

## JSON

서블릿에서는 `ObjectMapper`를 이용했었다.

### @RequestBody 사용 (HttpEntity도 사용 가능)

```java
@ResponseBody
@PostMapping
public String requestBodyJson(@RequestBody HelloData data) {
        return "ok";
}

@ResponseBody
@PostMapping
public HelloData requestBodyJson(@RequestBody HelloData data) {
        return data; // 객체를 직접 실어서 보낼 수도 있음
}

```

+ HttpMessageConverter가 Http 메시지 바디의 내용을 String 혹은 Object로 변환해줌
  + 요청 헤더에 Content-Type으로 `application/json`이여야만 컨버터가 실행됨!


<br>
<br>

---
> 참고: 인프런 - 김영한님의 스프링 MVC 1편(백엔드 웹 개발 핵심 기술) https://inf.run/6La3