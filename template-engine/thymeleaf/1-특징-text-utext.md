## 타임리프의 특징

### 서버 사이드 렌더링(SSR)

백엔드 서버에서 HTML을 동적으로 렌더링하는 용도

### <font color="skyblue">네츄럴 템플릿</font>

#### 순수 HTML을 최대한 유지한다.

+ `웹 브라우저에서 파일을 직접 여는 상황`
  + thymeleaf는 순수 HTML으로 작성된 결과를 볼 수 있다.
  + JSP 등의 다른 템플릿들은 템플릿 소스 코드와 HTML이 합쳐진 형태를 볼 수 있다. (렌더링되어야 HTML만 보인다)
+ 서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과 확인 가능
  + JSP 등의 다른 템플릿들도 마찬가지.

### 스프링 통합 지원

스프링의 다양한 기능을 편리하게 사용하도록 지원함.

<br>

## 기본 표현식

### 간단한 표현
```thymeleafexpressions
+ 변수 표현식: ${...}
+ 선택 변수 표현식: *{...}
+ 메시지 표현식: #{...}
+ 링크 URL 표현식: @{...}
+ 조각 표현식: ~{...}
```
### 리터럴
```thymeleafexpressions
+ 텍스트: 'one text', 'Another one!',…
+ 숫자: 0, 34, 3.0, 12.3,…
+ 불린: true, false
+ 널: null
+ 리터럴 토큰: one, sometext, main,…
```

### 문자 연산
```thymeleafexpressions
+ 문자 합치기: +
+ 리터럴 대체: |The name is ${name}|
``` 

### 산술 연산
```thymeleafexpressions
+ Binary operators: +, -, *, /, %
+ Minus sign (unary operator): -
```

### 불린 연산
```thymeleafexpressions
+ Binary operators: and, or
+ Boolean negation (unary operator): !, not 
```

### 비교와 동등
```thymeleafexpressions
+ 비교: >, <, >=, <= (gt, lt, ge, le)
+ 동등 연산: ==, != (eq, ne)
```

### 조건 연산
```thymeleafexpressions
+ If-then: (if) ? (then)
+ If-then-else: (if) ? (then) : (else)
+ Default: (value) ?: (defaultvalue)
```

### 특별한 토큰
```thymeleafexpressions
+ No-Operation: _
```

## text, utext

+ 콘텐츠에 데이터 출력하기
```thymeleaftemplatesexpressions
<span th:text="${data}"></span>
```
+ 컨텐츠 안에서 직접 출력
```thymeleaftemplatesexpressions
<span>이 데이터는 [[${data}]]</span>
```

### Escape

웹 브라우저는 `<`를 HTML **태그의 시작**으로 인식하기 때문에 이를 문자로 표현하는 방법을 **HTML 엔티티**라고 함 <br>
Escape: HTML에서 사용되는 `<`, `>` 등과 같은 특수 문자를 HTML로 엔티티로 변경함
+ `th:text`, `[[...]]`는 이스케이프를 제공함

### Unescape

+ `th:utext`
+ `[(...)]`

> escape를 사용하지 않으면 HTML이 정상적으로 렌더링되지 않을 수 있음. 필요할 때만 unescape 사용할 것!