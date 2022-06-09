# 상품을 등록하거나 결제할 때 주의해야 할 사항

다음과 같이 상품을 등록하는 코드가 있다.
```java
@PostMapping("/add")
public String addItem(@ModelAttribute Item item) {
    respository.save(item);
    return "basic/item";
}
```

### 새로고침을 하게 된다면?

`경고창`이 하나 뜨고, 계속 진행하면 상품이 중복으로 등록되는 것을 확인할 수 있다.

1. 클라이언트는 `상품 등록 폼 화면`에서 상품을 등록한다. (post)
2. view template을 호출해서 `상품 상세 화면`으로 이동하여 결과를 보여준다.

> **웹 브라우저에서 새로고침은 마지막에 서버에서 전송한 데이터를 다시 전송한다.**

2번에서 마지막으로 받은 요청은 `post`이다. 그렇기 때문에 새로고침을 실행하면 post로 하여금 상품을 계속 등록하는 셈이다.

<br>

## Post | Redirect | Get

<img width="420" alt="image" src="https://user-images.githubusercontent.com/56334513/172744587-7eb8eec9-bca9-4c34-80a6-bf0bc1d5d7ba.png">

+ 상품을 저장하면 뷰 템플릿을 호출하지 않고(item.html/post) 상품 상세 화면으로 리다이렉트를 호출(item.html/get)하면 된다.
+ 그렇게 되면 웹 브라우저는 리다이렉트에 의해 컨트롤러에 구현해놓은 상품 상세 화면으로 이동한다.
+ 이후에 새로고침을 하게 되면 상품 상세 화면으로만 이동하게 된다.

다음과 같이 코드를 수정하자.
```java
@PostMapping("/add")
public String addItem(@ModelAttribute Item item) {
    repository.save(item);
    return "redirect:/basic/items/" + item.getId();
}
```
+ 리다이렉트에 성공했다!
+ 하지만 반환으로 String의 덧셈 연산을 하게 되면 URL 인코딩이 불가능하다.
+ 해결 방법은 `RedirectAttributes`를 사용한다.

<br>

### RedirectAttributes

`RedirectAttributes`를 반영한 코드
```java
@PostMapping("/add")
public String addItem(@ModelAttribute Item item, RedirectAttributes redirectAttributes) {
    Item savedItem = repository.save(item);
    redirectAttributes.addAttribute("itemId", savedItem.getId());
    redirectAttributes.addAttribute("status", true); // 상품이 잘 저장했는지 사용자에게 알려주기 위한 용도
    return "redirect:/basic/items/{itemId}";
}
```

`http://localhost:8080/basic/items/1?status=true` <br>

#### 기능
+ URL 인코딩을 도와준다.
+ `@PathVariable`처럼 바인딩해준다.
+ 쿼리 파라미터로 처리해준다.

> 타임리프에서 사용하려면?

+ 타임리프에서 쿼리 파라미터를 편리하게 조회할 수 있게 `param`이라는 변수 제공
+ 원래는 컨트롤러에서 모델에 직접 담고 뷰 템플릿에서 값을 꺼내야 함..