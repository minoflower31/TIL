# 스프링 mvc 구조

## DispatcherServlet

+ 스프링 MVC의 프론트 컨트롤러
+ 스프링 부트는 `DispacherSerlvet`을 서블릿으로 자동 등록하고 모든 경로에 대해서 매핑함 (urlPatterns="/")
+ 요청 흐름은 다음과 같다.
  + 서블릿이 호출되면 HttpServlet의 `service()` 호출
  + 상속하고 있는 FrameworkServlet를 통해 `DispacherServlet.doDispatch()`가 호출

## 동작 순서
<img width="418" alt="image" src="https://user-images.githubusercontent.com/56334513/173078154-f0474517-17f3-4ff6-95de-cd4eeb4dc6bd.png">

+ 1.`핸들러 조회`: 핸들러 매핑으로 요청한 URL에 매핑된 핸들러(컨트롤러)를 조회 (헤더와 함께 조회 가능)
+ 5.`ModelAndView 반환`: 핸들러 어댑터는 핸들러가 반환한 정보를 ModelAndView로 변환한 후에 반환
+ 7.`View 반환`: 뷰 리졸버가 뷰의 논리 이름을 물리 이름으로 변환한 후, 렌더링을 담당하는 뷰 객체를 반환
  + JSP는 InternalResourceView(JstlView)를 반환. 내부에 `forward()` 로직 존재

## HandlerMapping, HandlerAdapter

스프링 부트는 <u>우선 순위</u>를 적용해서 자동 등록하고, 순서대로 찾되 없으면 다음 순서로 넘어감.
+ HandlerMapping
  + **0** (`RequestMappingHandlerMapping`): 어노테이션 기반 컨트롤러에서 `@RequestMapping` 사용 시
  + **1** (BeanNameUrlHandlerMapping): 스프링 빈 이름으로 핸들러를 찾음.
+ HandlerAdapter
  + **0** (`RequestMappingHanlderAdapter`): 어노테이션 기반 컨트롤러에서 `@RequestMapping` 사용 시
  + **1** (HttpRequestHandlerAdapter): HttpRequestHandler(서블릿과 가장 유사한 형태) 처리
  + **2**: Controller 인터페이스

## ViewResolver

+ **1** (BeanNameViewResolver): 빈 이름으로 뷰를 찾아서 반환
+ **2** (InternalResourceViewResolver): JSP를 처리하는 뷰를 반환

#### viewResolver가 호출되면..
+ ModelAndView에 담은 뷰 이름으로 viewResolver를 순서대로 호출
+ BeanNameViewResolver가 호출됐을 때 뷰 이름을 가진 스프링 빈이 없으면 다음 ViewResolver를 호출

### c.f.

+ JSP를 제외한 나머지 뷰 템플릿들은 `forward()` 과정 없이 바로 렌더링된다.
+ Thymeleaf를 사용하면? 스프링 부트가 `ThymeleafViewResolver`를 자동으로 등록해줌

<br>

## @RequestMapping

> 실무에서 99% 이상 사용하는 컨트롤러 방식

+ 요청 정보(URL)를 매핑하는데, URL이 호출되면 `@RequestMapping`이 달린 메서드가 호출된다.
+ 클래스 레벨과 메서드 레벨을 분리할 수 있다.
```java
@Controller
@RequestMapping("/spring/v1/members")
public class SpringMemberControllerV1 {
    
    @GetMapping
    public String getList() {
        //...
    }

  @PostMapping("/add")
  public String save() {
    //...
  }
}
```
 

