# Interface

**클래스들이 구현해야 하는 동작을 지정하는데 사용되는 추상 자료형** <br>
<br>

모든 메서드는 추상 메서드로 선언: `public abstract` <br>
모든 변수는 상수로 선언: `public static final` <br>

    자바 8 이전에는 모든 메서드가 구현부를 포함하지 않았지만, <br>
    자바 8 이후부터는 **default, static** 키워드가 달린 메서드에서는 구현부를 가질 수 있게 됨

<br>

### default method (java 8 이상)

- 구현부를 가지는 메서드
- 인터페이스를 구현한 클래스에서 공통으로 사용 가능
- 인스턴스를 생성한 후, 참조변수를 통해 사용

```java
default void description() {
    ...
}
```

<br>

### static method (java 8 이상)

- 인스턴스 생성 없이 인터페이스 타입으로 사용 가능

```java
static int total() {
    ...
}

//main
Calc.total();
```

<br>

### private method (java 9 이상)

- 인터페이스를 구현한 클래스에서 사용하거나 재정의 불가능
- 인터페이스 내부에서만 사용 가능
- 인터페이스 내 default 메서드나 static 메서드에서 사용

<br>
<br>

## 왜 인터페이스를 사용하는가?

- 일종의 클라이언트 코드와의 약속과 같으며, 클래스나 프로그램이 제공하는 명세(specification)
- 클라이언트는 구현 객체의 내부 기능을 알 필요가 없이 interface를 통해 기능만 가져다 씀
- 다형성

<br>
<br>

## 여러 인터페이스 구현

- 인터페이스에 구현 코드가 없기 때문에 하나의 클래스에서 여러 인터페이스를 구현할 수 있음
- default 메서드가 중복되면 구현하는 클래스에서 overriding 해야 함
  
<br>
<br>

## 인터페이스의 상속

- `extends` 키워드 사용
- 인터페이스는 다중 상속 가능, **타입 상속**이라고도 불린다.
- 클래스끼리 다중 상속이 안되는 이유는 `diamond problem` 이 발생하기 때문
  - 같은 이름을 가진 인스턴스 변수가 있으면 혼동을 야기함

<br>
<br>

# 자바의 유용한 클래스 (jdk)

## java.lang 패키지

프로그래밍 시 `import` 선언을 해주지 않아도 자동으로 import됨

<br>
<br>

## Obejct 클래스

- 모든 클래스의 최상위 클래스
- 컴파일 시점에 컴파일러가 **extends Object** 를 클래스에 추가

<br>

### equals() 메서드

- 두 인스턴스의 주소값을 비교하여 T/F 반환
- 인스턴스가 달라도 논리적으로 동일한 경우는 true로 반환하도록 overriding하기

<br>

### hashCode() 메서드

- 인스턴스의 저장 주소를 반환
- 힙 메모리에 인스턴스가 저장되는 방식이 hash 방식
- 두 인스턴스가 같다면 인스턴스의 주소가 같아야 하므로 equals() 메소드를 overriding했다? hashCode도 overriding하자
- e.g. hashCode의 return 값: memberId

<br>

### clone() 메서드

- 객체의 원본을 복제하는 데 사용하는 메서드
- clone() 사용 시 객체의 정보(private으로 선언된 멤버변수들,,) 또한 복제되므로 정보 은닉에 위배할 가능성 생김
- `Cloneable` 인터페이스를 implements 하면서 명시적으로 자바에게 알림

<br>
<br>

# String, StringBuilder, StringBuffer

```java
String str1 = new String("abc"); // 힙 메모리에 인스턴스로 생성됨
String str2 = "abc"; // 상수 풀에 있는 주소를 참조함
```
<br>

String을 연결하면 기존의 String에 연결하지 않고 새로운 문자열로 생성됨
- 메모리 낭비 발생

<br>

### 해결

> <br>
> **StringBuffer, StringBuilder** 사용
> 내부에 `char[]` 가 자료형인 멤버 변수를 사용해서 가변적으로 문자를 늘림
> 자바는 다음과 같이 권고한다. **StringBuilder는 싱글 스레드에서, StringBuffer는 멀티 쓰레드(동기화 보장)에서 사용하기**
> <br>

<br>
<br>

### text block 사용하기 (java 13 이상)

- 문자열을 `*** {내용} ***` 사이에 이어서 작성 가능
- html, json 문자열을 만드는데 유용함


