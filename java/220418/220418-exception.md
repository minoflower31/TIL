# 예외 처리

## 프로그램에서의 오류

### 컴파일 오류

- 프로그램 코드 작성 중에 발생하는 문법적 오류
<br>

### 런타임 오류

- 프로그램을 실행할 때 의도하지 않은 동작(bug)이 발생하거나 프로그램이 중지되는 오류
- 비정상 종료 시에 시스템에 심각한 장애가 발생할 수 있음

<br>

## 예외 처리의 중요성

- 비정상 종료를 피하여 시스템이 원활히 실행되도록 함
- 오류 발생 시 `log`를 남겨 분석을 통해 원인 파악 및 bug 수정
  - (자바 패키지에 Logger 클래스 존재)

<br>

## Error 클래스, Exception 클래스

### Error

- 가상 머신에서 발생, 프로그래머가 처리할 수 없는 오류
- e.g. 힙 메모리가 없는 경우, 스택 메모리의 오버플로우 등

<br>

### Exception

- 프로그램에서 제어할 수 있는 오류
- e.g. I/O하기 위한 파일이 존재하지 않음, DB 연결 오류 등

<br>

## try-catch, try-catch-finally

finally는 보통 리소스를 해제하기 위해 사용 <br>
<br>

### try-with-resources

    리소스를 try() 괄호 안에 선언하면 자동으로 close()를 호출해 리소스를 해제시킴 <br>

```java
try(FileInputStream fis = new FileInputStream()) {
        ...
}
```
- 자바 7부터 제공
- 리소스 클래스는 `AutoCloseable` 인터페이스를 구현하고 있어야 사용 가능
- e.g. FileInputStream
- 자바 9부터는 try 외부에서 초기화하고 괄호 안에 참조변수 대입도 가능

<br>

## 예외처리 미루기

main(), loadFile() 메서드 존재. <br>
loadFile()에서 `throws` 키워드로 Exception을 설정하면 이를 호출한 main() 메서드로 던짐(미루기) <br>

## 여러 개의 Exception 다루기

```java
try {
    
} catch(FileNotFoundException | IOException e) {
    ...
}
```
<br>

## 사용자 정의 Exception 클래스

1. `Throwable` 을 상속받는 클래스 구현
   - String 매개변수가 담긴 생성자 작성(에러 구문 담기)
2. `throw new ClassName("message")`로 예외 발생

- **Regax 표현식 공부하기**