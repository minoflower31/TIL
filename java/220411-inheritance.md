# 상속 

**"상속은 연관된 일련의 클래스에 대해 공통적인 규약을 정의하는 개념"** - O <br>
**"상속은 코드의 재활용을 위한 문법이다"** - X <br>
<br>

상속**하는** 클래스: 상위 클래스
상속**받는** 클래스: 하위 클래스
<br>
<br>

## 자바는 `단일 상속`

### Diamond Problem

**모호성을 야기한다** <br>

```java
class Man {
    int age;
}

class Woman {
    int age;

}

class Student extends Man, Woman {
    /*
    *   어느 부모의 age를 들고 와야 하나?
    */
}
```
<br>

<img width="269" alt="image" src="https://user-images.githubusercontent.com/56334513/162876631-b672ede5-5a5d-481b-b261-f9806b80c5ba.png">

<br>
<br>
<br>

## 상속 과정에서 클래스 생성 과정

**하위 클래스의 인스턴스를 생성 시, 상위 클래스의 생성자가 먼저 호출됨** <br>

```java
public class A {
    ...
}

public class B extends A {
    
    public B() {
        A()// 상위 클래스 생성자가 호출되는 순간
    }
}
```
<br>

### super 키워드

- 하위 클래스가 갖는 상위 클래스의 **참조값**
- 상위 클래스의 기본 생성자를 호출
- 하위 클래스에서 명시적으로 상위 클래스 호출하지 않아도 super()는 자동 호출됨
  - 상위 클래스에 매개변수 1개 이상인 생성자 존재 시, 하위 클래스에서 명시적으로 super()에 매개변수 전달

```java
public A(int id, int name) {
    this.id = id;
    this.name = name;
}

class B extends A {
    public B(int id, int name) {
        spuer(id, name)
    }
}
```

<br>

### final 키워드

- final + 인스턴스 변수 = **상수**
- final + 메서드 = **overriding X**
- final + 클래스 = **상속 X**

<br>


### 상속에서 인스턴스 메모리 상태

힙 메모리 생성 순서는, <br>
1. 상위 클래스의 인스턴스 생성
2. 하위 클래스의 인스턴스 생성

<br>
<br>
<br>

## 상속의 논리적 포함관계

### IS - A 관계

    자식 클래스는 부모 클래스이다
    = 자식 클래스 is a 부모 클래스

하위 클래스는 상위 클래스의 모든 특성을 지닌다 <br>
하위 클래스는 자신만의 추가적인 특성을 더하게 된다. <br>
<br>

즉, 논리적으로 하위 클래스는 상위 클래스에 온전히 포함되어야 한다. <br>

예를 들면 **Sugar - Candy - 피타고라스의 정리** 의 상속 관계를 가질 때<br>
- 사탕은 설탕이야 (사탕 is a 설탕) - O
  - 사탕을 보면 설탕이 떠오른다
<br>

- 피타고라스의 정리는 사탕이야 (피타고라스의 정리 is a 사탕) - X
  - 🤔??? 

<br>
<br>

### HAS - A 관계

학문적으로는 소유의 관계이므로 상속을 뜻하는 개념이 될 수 있으나, **상속으로 보기 어렵다.** <br>

예를 들면 **자동차 - 슈퍼카** 는 is a 관계로 상속이지만, **자동차 - 바퀴** 는 단순히 자동차가 바퀴를 가지고 있는 개념이다. 그래서 **has-a 관계에서는 메서드로 분류하는 것이 좋다**

<br>
<br>
<br>

## 형 변환

인스턴스 초기화 시, 상위 클래스는 좌측 _ 하위 클래스는 우측 <br>

e.g. Customer c = new VIPCustomer(); <br>
<br>

**묵시적 타입 변환: 자식 객체를 부모 타입의 참조변수에 할당하는 것** <br>

그렇다면 메모리는 어디를 가리킬까? <br>
- 변수 타입을 따르므로 Customer의 인스턴스 변수 및 메서드에만 접근 가능

<br>
<br>

## Overriding

```java
Customer vc = new VIPCustomer();
```

변수 타입: Customer <br>
인스턴스 생성 타입: VIPCustomer <br>
<br>

자바에서 상속하고 있는 메서드를 overriding했다면, 인스턴스 생성 타입의 메서드가 호출됨 **(가상 메서드의 원리)** <br>
- vc.getPrice() // VIPCustmomer의 getPrice() 호출
- 자바는 모든 메서드가 가상 메서드