# Class 클래스

- 자바의 모든 클래스와 인터페이스는 컴파일 후 class 파일로 생성됨
- 컴파일된 class 파일을 로드하여 객체를 **동적 로딩**하고 정보를 가져오는 메소드 제공
- `forName()` 메서드로 클래스를 동적으로 로드함

<br>
<br>

## 동적 로딩

**컴파일 시에 데이터 타입이 binding되는 것이 아닌, 실행(runtime) 중에 데이터 타입이 binding되는 방법** <br>

<br>

**장점**
+ 런타임 시에 원하는 클래스를 로딩하여 binding 가능

**단점**
+ 컴파일 시에 타입이 정해지지 않았으므로 클래스 정보를 잘못 입력 시 `ClassNotFoundException` 발생

<br>
<br>

## Reflection

**Class 클래스로 클래스나 인터페이스를 동적 로딩하여 객체를 생성하거나 변수를 변경, 혹은 메서드를 호출할 수 있는 기능** <br>
<br>

다음과 같은 정보를 불러옴
+ Class
+ Constructor
+ method
+ field

<br>

### 유용한 점은?

+ private으로 설정된 변수 값을 임의로 변경시킬 수 있음
+ 필요한 정보만 골라서 사용할 수 있음

<br>
<br>
<br>

# Generic

**자료형을 일반화시키는 행위**
<br>

**특징**
+ 타입 매개변수는 인스턴스 생성 시 결정됨
+ 실제 사용되는 자료형의 반환은 컴파일러에 의해 검증되므로 안정적임
+ 컬렉션 프레임워크에서 주로 사용
+ 타입 매개변수는 사용하려는 의도가 분명하게 지을 것
  + **E: element, K: key, V: value, N: number, T: type**
<br>

만약 제네릭이 아닌 Object로 타입 지정시, **명시적 형변환**을 거쳐야 함 <br>
```java
class ObjectType {
    Object car;

    /*
    * getter, setter
    */
}

class Car {

}

class Main {
    //main
    ObjectType obj = new ObjectType();
    Car car = new Car();
    obj.setObjectType(car);

    Car temp = (Car) obj.getObjectType();
}
```
<br>
<br>

## 용어 정리

```java
class Box<T> {...}

class Main {
    public static void main() {
        Box<String> box = new Box<>();
    }
}
```

- 타입 매개변수: `Box<T>에서 T`
- 타입 인자: `Box<String>에서 String`
- 제네릭 타입: `Box<String` (= 매개변수화 타입)

<br>
<br>

## `<T extends Class>` 사용하기

### 상위 클래스의 필요성

- T 자료형의 범위를 제한할 수 있음
- 상위 클래스로부터 추상이나 정의된 메서드를 활용할 수 있음
- 상속 받지 않는다면, T는 Object로 변환하여 Object 클래스가 기본으로 제공하는 메서드를 사용 가능

<br>
<br>

### T extends

- 부모 클래스를 생성하고 제네릭 타입을 `<T extends 부모클래스>`로 설정
- **이제 부모 클래스를 상속 받은 자식 클래스만이 제네릭 타입으로 올 수 있음**
- 평상적으로 부모 클래스는 추상 클래스로 만들고 자식 클래스가 상속받는 구조

<br>
<br>

## 제네릭 메서드 활용

**제네릭 클래스가 아니어도 일부 메서드 또한 제네릭으로 정의하는 것이 가능한 메서드** <br>
+ static 유무와 상관없이 정의 가능
+ 형태: `public <T> Box<T> makeBox(T box) {...}`
  + 앞쪽의 `<T>`는 컴파일러에게 T가 타입 매개변수임을 알리는 표시
+ 제네릭 메서드는 메서드 호출 시 자료형이 결정됨

<br>
