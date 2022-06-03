# HTTP 메시지 컨버터

+ HTTP 요청: @RequestBody, HttpEntity(RequestEntity)
+ HTTP 응답: @ResponseBody, HttpEntity(ResponseEntity)

<br>

### HTTP 메시지 컨버터의 인터페이스 내 메서드
+ `canRead()`, `canWrite()`: 클래스 타입과 미디어 타입을 지원하는지 체크
+ `read()`, `write()`: 메시지를 읽고 쓰는 기능이 담김

<br>

### 메시지 컨버터의 우선 순위

클래스 타입과 미디어 타입을 체크할 때 만약 만족하지 못하면 다음 메시지 컨버터로 우선 순위를 부여한다.

+ `ByteArrayHttpMessageConverter`: byte[] 처리
  + 쓰기 미디어 타입: `application/octet-stream`
+ `StringHttpMessageConverter`: String 문자 처리
  + 쓰기 미디어 타입: `text/plain`
+ `MappingJackson2HttpMessageConverter`: json 처리 (클래스 타입은 객체, HashMap 등)
  + 쓰기 미디어 타입: `application/json`

<br>

### HTTP 요청 데이터 읽기

1. 컨트롤러에서 HttpEntity 또는 @RequestBody 파라미터를 사용하고 있다.
2. Http 요청이 오면 메시지 컨버터가 메시지를 읽어들일 수 있는지 확인하기 위해 `canRead()`를 호출한다.
    + 이 때, 클래스 타입과 **Content-Type** 미디어 타입을 체크한다.
3. 조건을 만족하면 `read()`를 호출해서 객체를 생성 및 반환한다.

### HTTP 응답 데이터 쓰기

1. 컨트롤러에서 HttpEntity를 반환 타입으로 설정하거나 @ResponseBody를 설정하고 있다.
2. 값이 반환되고 메시지 컨버터는 `canWrite()`를 호출한다.
   + 이 때, 클래스 타입과 **Accept** 미디어 타입을 체크한다. (스프링 mvc에서는 @ReqeustMapping의 produces)
3. 조건을 만족하면 `write()`를 호출해서 메시지를 작성한다.

<br>

## 요청 매핑 핸들러 어댑터

HTTP 메시지 컨버터는 어디쯤에서 사용될까?

<img width="545" alt="image" src="https://user-images.githubusercontent.com/56334513/171877572-fd588b83-205a-494a-bcb0-878198cd6d82.png">

### ArgumentResolver

다양한 파라미터로 넘어오는 정보들을 **유연하게 처리할 수 있는 이유**는 `ArgumentResolver`!
+ 컨트롤러에게 필요한 파라미터의 값을 생성
  + `supportParameter()`로 파라미터를 검증하고, 이를 통과하면 `resolveArgument()`를 호출해서 객체를 생성
+ 이제 `RequestMappingHandlerAdapter`는 준비된 값을 컨트롤러에게 전달

### ReturnValueHandler

컨트롤러에게 응답 값을 전달받으면 이를 변환한 후에 처리하는 기능을 담당한다. <br>


<br>

## HTTP 메시지 컨버터 위치

결론: `ArgumentResolver`와 `ReturnValueHandler`에 존재
<br>
<br>

`ArgumentResolver`
+ @HttpEntity와 @RequestBody으로 파라미터가 넘어오면 HTTP 메시지 컨버터를 호출해서 필요한 객체를 생성한다.
`ReturnValueHandler`
+ @HttpEntity와 @ResponseBody 또한 HTTP 메시지 컨버터를 호출해서 처리한다.





