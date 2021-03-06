## HTML

**정적 리소스**: 정적인 HTML, CSS, JS, 이미지, 영상 등을 제공하며 주로 웹브라우저를 통해 응답함. <br>
**동적인 HTML 페이지 생성**: 직접 HTML를 작성하거나 JSP, 타임리프 같은 템플릿 엔진으로 웹 브라우저에게 응답함

## HTTP API

- HTML이 아닌 데이터(JSON 형식)를 전달.
- 다양한 시스템에서 호출함
  - 웹 클라이언트(React, Vue.js)
  - 웹 브라우저에서 HTTP API 호출 (Ajax)
  - 앱 클라이언트(아이폰, 안드로이드, pc 앱)
  - 서버 to 서버 [WAS끼리]

## SSR (Server Side Rendering)

- HTML 최종 결과를 서버에서 만들어 웹 브라우저에 전달
- 주로 정적인 화면일 때
- `JSP`, `타임리프`

>  웹 브라우저로부터 HTML Form으로 요청이 올 경우, 서버는 DB에서 조회한 후에 동적으로 HTML을 생성해 응답한다.

## CSR

- HTML 최종 결과를 자바스크립트로 웹 브라우저에서 동적으로 생성해서 적용시킴
- 주로 동적인 화면일 때. 부분적으로 변경이 가능하다.
- `React`, `Vue.js`

c.f. 꼭 SSR과 CSR의 명확한 분리는 없다. SSR를 사용하면서 자바 스크립트를 통해 동적으로 변경 가능.
<br>

> html, css, js 리소스를 서버에서 불러오거나, HTTP API로 JSON 형태의 데이터를 요청할 때 

<br>

## 자바 웹 기술의 역사

+ 서블릿(1997): HTTP 요청 메시지를 편하게 꺼내 사용하고, HTTP 응답 메시지를 편하게 작성할 수 있지만 HTML 생성이 쉽지 않다.
+ JSP(1999): HTML 생성이 편리해졌지만 비즈니스 로직이 포함되어 코드가 복잡하고 많은 역할을 담당하게 됨
+ MVC 패턴[서블릿+JSP]: Model, View, Controller
+ MVC 프레임워크: MVC 패턴을 자동화하고 복잡한 웹 기술을 편리하게 사용함
  + 하지만,, 당시 스프링 MVC는 효율이 좋지 않아 개발자들은 스트럿츠 같은 기술을 대신 사용

<br>

+ 애노테이션 기반의 스프링 MVC: `@Controller`
+ Spring Boot: 톰캣 서버를 내장하고 있으며 Jar 파일에 WAS 서버를 포함해 빌드 배포를 단순화함.
  + 원래는 WAS를 직접 설치하는 데다 소스는 War 파일을 만들어서 WAS에 배포함 -> 번거롭다.

<br>

## 자바 뷰 템플릿의 역사

`JSP`는 속도가 느린 데다 기능이 부족하여 `프리마커`나 `벨로시티`의 등장으로 JSP 문제를 해결했다. <br>
하지만 `타임리프`는 내추럴 템플릿으로 스프링 MVC에 특화돼있음.

<br>
<br>

> 출처: 인프런 - 김영한님의 스프링 MVC 1편(백엔드 웹 개발 핵심 기술) https://inf.run/6La3