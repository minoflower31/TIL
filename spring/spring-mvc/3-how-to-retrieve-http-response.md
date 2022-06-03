# 스프링 mvc가 HTTP 응답을 만드는 방법

+ **정적 리소스**는 정적인 HTML, CSS, JS를 웹 브라우저에게 제공
+ **뷰 템플릿**은 동적인 HTML을 웹 브라우저에게 제공
+ **HTTP 메시지**는 HTTP 메시지 바디에 JSON과 같은 형식으로 실어 보냄

## 정적 리소스

스프링 부트는 classpath의 디렉토리를 통해 정적 리소스를 제공하는데, `src/main/resources`가 바로 classpath의 시작 경로이다.
+ `/static`, `/public` 등
+ 정적 리소스는 파일을 변경하지 않는다.

<br>

## 뷰 템플릿

주로 HTML을 동적으로 생성하는 용도다.

### 스트링으로 반환: View 아니면 HTTP 메시지
```java
/*
        @ResponseBody가 없는 경우: 뷰 리졸버를 통해 뷰를 렌더링
        @ResponseBody가 있는 경우: HTTP 메시지 바디를 직접 문자가 입력됨
 */
 */
public String response(Model model) {
    model.addAttribute();
    return "response/hello";
}
```
<br>

## HTTP API, 메시지 바디에 직접 입력

### String이 담긴 HTTP 메시지 반환
```java
// 방법1
public ResponseEntity<String> responseBody(){
    return new ResponseEntity<>("ok", HttpStatus.OK);
}
// 방법2
// @ResponseBody으로 어노테이션 설정
```

#### Json 형식이 담긴 HTTP 메시지 반환

```java
// 방법1
public ResponseEntity<Member> responseBody(){
    Member member = new Member(1L, "park", 20);
    return new ResponseEntity<>(member, HttpStatus.OK);
}

// 방법2
// @ResponseBody으로 어노테이션 설정
```
참고로 `@ResponseBody`는 상태 코드를 설정할 수 없기 때문에 `@ReponseStatus` 어노테이션을 이용. <br>
**하지만 컨트롤러 로직에 따라 상태 코드가 동적으로 변경된다면 `HttpEntity`를 사용하자.**

<br>

## `@RestController`

해당 컨트롤러에 있는 메서드에 `@ResponseBody`를 적용시킨다. 이는 모든 메서드는 뷰 템플릿을 이용하지 않고 HTTP 메시지 바디로 String 혹은 Json 형태로 응답을 보내겠다는 의미다. 



<br>
<br>

---
> 참고: 인프런 - 김영한님의 스프링 MVC 1편(백엔드 웹 개발 핵심 기술) https://inf.run/6La3