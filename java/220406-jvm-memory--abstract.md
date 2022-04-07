# 목차

1. [자바 가상 머신의 메모리](#자바-가상-머신의-메모리)
2. [overriding과 가상 메서드의 원리](#overriding과-가상-메서드의-원리)
3. [추상 클래스](#추상-클래스(abstract-class))

<br>
<br>

## 자바 가상 머신의 메모리

**메소드 영역** -  메소드의 바이트 코드, static 변수 <br>
- 바이트 코드: JVM에 의해 실행 가능한 코드(소스파일을 컴파일할 시 생성)
- 바이트 코드가 메모리 공간에 존재해야 실행 가능
- 즉, 메소드 영역이란 특정 클래스의 정보가 메모리 공간에 로드되어 프로그램이 종료할 때까지 사용할 수 있는 영역

<br>

**힙 영역** - 인스턴스 <br>
- 인스턴스가 생성되는 영역
- 소멸은 가비지 컬렉션에 의해 이루어짐
    - 가비지 컬렉션의 빈번한 실행은 시스템에 부담 주기 때문에 별도의 알고리즘을 기반으로 계산되며, 계산 결과를 토대로 수행함

<br>

**스택 영역** - 지역변수, 매개변수 <br>
- 중괄호 내(block)에만 유효한 변수들

<br>
<br>

## overriding과 가상 메서드의 원리

### 메서드는 어떻게 호출되고 실행될까?

1. 바이트코드가 메소드 영역에 할당되고
2. 메소드가 호출되면 해당 주소를 찾음 (메소드 내 변수는 스택 영역에 할당됨)
3. 다른 인스턴스라도 메소드의 주소는 변함 없으므로 동일한 메소드가 호출됨
4. 인스턴스가 생성될 때마다 힙 메모리에 할당되지만, 메소드는 처음 한번만 로드됨


```java
public class Car {
	public void drive() {
		System.out.println("Drive Mode");
	}
}

public class Main {
	public static void main() {
		Car car1 = new Car();
		Car car2 = new Car();
		
		car1.drive();
		car2.drive();
	}
}
```

<img width="453" alt="image" src="https://user-images.githubusercontent.com/56334513/162231915-695098b6-689d-4def-98b0-aa7bd414ab45.png">

<br>

### 가상 메서드의 원리

- 가상 메서드 테이블은 메서드에 대한 주소를 가지고 있음
- 재정의된 경우는 재정의된 메서드의 주소를 가리킴

![image](https://user-images.githubusercontent.com/56334513/162229790-6d2d23b3-8822-4b82-9d99-90e40ec42aa1.png)


<br>
<br>

## 추상 클래스(abstract class)

- 메서드의 선언부만 있는 추상 메서드를 포함
- method declaration: 반환 타입, 메서드 이름, 매개 변수
- method definition: 구현부(body)를 가짐 - {}

<br>

### 응용: 템플릿 메서드 패턴

- 추상 메서드나 구현된 메서드를 활용하여 코드의 시나리오를 정의하는 패턴
- 시나리오 흐름을 위해 final로 선언 (재정의 x)
- 프레임워크에서 많이 사용되는 패턴

