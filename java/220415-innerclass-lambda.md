# inner 클래스

**클래스 내부에 선언한 클래스로 클래스 내에서만 사용됨** <br>
<br>

## 1. 인스턴스 내부 클래스

- 외부 클래스가 생성된 이후에 생성되는 클래스
- 평상적으로 `private`으로 선언

<br>

## 2. static 내부 클래스

- 외부 클래스 생성과 무관하게 사용 가능한 클래스
- static 변수 및 메서드 사용
- c.f. static > 인스턴스

<br>

## 3. local 내부 클래스

- 지역 변수처럼 메서드 내부에 정의해서 사용하는 클래스
- 메서드의 호출이 끝나게 되면 메서드에서 사용한 지역변수는 모두 반환됨
- Runnable 인터페이스 내의 run() 메서드에서는, **메서드 호출 이후에도 로컬 변수가 사용될 수 있으므로 final로 선언하여 data 영역에 저장**

```java
    Runnable getRunnable(int i) { //method
        int num = 10;

        // getRunnable 함수만 호출될 뿐 MyRunnable 클래스는 사용되지 않음 -> 익명 추천!
         class MyRunnable implements Runnable {

            int localNumber = 1000;

            @Override
            public void run () {
                System.out.println("outNumber = " + outNumber);
                System.out.println("outStaticNumber = " + outStaticNumber);

                //값을 사용할 수 있지만 상수화시켜줌
                System.out.println("i = " + i);
                System.out.println("num = " + num);
            }
        };
    }
```
<br>

## 익명 내부 클래스

- 말 그대로 이름이 없는 클래스 (클래스 이름을 생략)
- 인터페이스나 추상 클래스의 인스턴스를 초기화한 후에 구현

```java
//case1. 인터페이스 또는 추상 클래스의 자료형을 가지는 변수에 인스턴스 생성
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("hi");
        System.out.println("hoho");
    }
};

//case2. 메서드의 반환값으로도 익명 클래스가 사용될 수 있음 
Runnable getRunnable(int i) {
    int num = 100;

    return new Runnable() {
				
        @Override
        public void run() {
            //num = 200;   //에러 남
            //i = 10;      //에러 남
            System.out.println(i);
            System.out.println(num);
        }
    };
}
```
<br>
<br>

# 람다식 (Lambda Expression)

(Topic- OOP로 유지보수 하느냐, 함수형 프로그래밍으로 발전하는가(가독성의 문제)) <br>
<br>

#### 자바 8부터 함수형 프로그래밍 방식을 지원하는데, 함수형 프로그래밍 방식을 람다식이라고 함
#### 함수의 구현 및 호출만으로 프로그래밍이 수행되는 방식
<br>

    함수형 프로그래밍

`pure function`을 구현하고 호출함으로써 외부에 `side effect`를 건네주지 않도록 구현하는 방식

- pure function: 매개 변수만을 이용하여 만든 함수. 외부의 변수를 사용하지 않아 함수가 실행되더라도 전혀 외부에 영향을 끼치지 않음

입력받은 매개변수에 의해서만 실행되므로 동시에 수행되는 `병렬 처리`가 가능. `독립적`으로 실행되며 `동일한 자료형에 대해서는 동일한 결과를 보장`하고, 다양한 자료형이더라도 같은 기능을 수행할 수 있게 함
<br>
<br>

### 람다식 문법

- 익명 함수로 표현
- 구조: `(매개 변수) → (구현부)`

<br>

### 다양한 표현 방법

1. 기본 람다식
> `(int x, int y) -> {return x+y;}`

<br>

2. 매개 변수가 1개일 때 자료형과 소괄호 생략 가능

> `str -> {System.out.println(str);}`
>
3. 매개 변수가 2개 이상인 경우는 괄호 생략 불가능
<br> <br>

4. 실행문이 한 문장이라면 중괄호 생략 가능

> `str -> System.out.println(str);`

<br>

5. 실행문이 한 문장이더라도 return이 있으면 중괄호 생략 불가능

> `str -> return str.length(); //오류`

<br>

6. 실행문이 한 문장의 반환문인 경우에는 return과 중괄호를 모두 생략 가능

> `(x, y) -> x + y;` <br>
> `str -> str.length();`

<br>
<br>

## 함수형 인터페이스 선언

### `@FunctionalInterface`
- **람다 표현식만 사용하겠다는 의미를 가진 어노테이션**
- 람다 표현식이나 인터페이스가 가진 메서드를 직접 구현해서 사용 가능
- **메서드는 하나만 선언해야 함**
  - 일반 인터페이스도 추상 메서드가 하나라면 람다 표현식이 가능하지만, 메서드가 2개 이상일 때 람다 표현식을 사용하게 되면 어떤 메서드로 표현하는지 모르기 때문에 컴파일 에러 발생

**코드**
```java
//MyMath interface
@FuncionalInteface
public interface MyMath {
    
    int getMax(int n1, int n2);
    int getMax2(int n1, int n2); // 어노테이션에서 컴파일 에러 발생
}

// Main
public static void main() {
    MyMath max = (x, y) -> (x > y) ? x : y;
}
```
<br>
<br>

## OOP vs Lambda Expression

### OOP (익명사용 X)

1. 인터페이스 생성
```java
public interface Add {

    public int add(int n1, int n2);
}
```
<br>

2. 인터페이스를 구현하는 클래스 생성
```java
public class AddImpl implements Add {
    
    @Override
    public int add(int n1, int n2) {
        return n1+n2;
    }
}
```
<br>

3. 인스턴스 생성 후 메서드 호출
```java
public class Main {
    public static void main() {
        int n1 =1, n2 = 5;
        AddImpl addImpl = new AddImpl();
        addImpl.add(n1,n2);

        //람다식 구현
        Add math = (n1,n2) -> n1+n2;
        int res = math.add(n1,n2);
    }
}
```
<br>

### Lambda Expression

자바는 객체가 없으면 객체 내 메서드를 호출할 수 없는데, 람다표현식을 구현하면 **익명 내부 클래스**가 만들어지고 이를 통해 익명 객체가 생성된다.<br>
<br>

람다 표현식이 생성되는 과정
```java

Add math = new Add() {
    
    @Override
    public int add(int n1, int n2) {
        return n1+n2;
    }
}
math.add(1,5);
```
- 익명 내부 클래스이기 때문에 외부에 있는 로컬 변수의 값을 변경하면 오류 발생

<br>
<br>

# Stream

#### 긴 파이프의 한 쪽에서 다른 한 쪽으로 물을 흘러보내듯이, 데이터의 흐름을 처리하는 것은 Stream이라 한다.

<br>

### 특징
- 배열, 컬렉션을 대상으로 어느 자료형이든 여러 가지 연산을 수행할 수 있음
- 한번 생성하고 사용한 스트림은 재사용할 수 없음
- 다른 연산을 수행하려면 새로운 스트림을 다시 생성해야 함
- 스트림 연산으로 배열이나 컬렉션 값이 변경되지 않음
  - 별도의 메모리 공간을 생성하여 연산을 수행함
- **중간 연산과 최종 연산**
  - 중간 연산: 마지막이 아닌 위치에서 진행되어야 하는 연산
  - 최종 연산: 마지막에 진행되어야 하는 연산
- 지연 연산
    - 최종 연산이 존재해야 중간 연산을 비로소 수행함. 따라서 연산 중에는 중간 연산에 대한 결과를 알 수 없음
<br>

```java
Arrays.stream(arr);
```
<br>

### `중간 연산` 종류

> filter(), map(), sorted() ...

<br>

### `최종 연산` 종류

> forEach(), count(), sum() ...

<br>


## reduce() 연산

정의된 연산이 아닌 프로그래머가 직접 구현한 연산을 적용

> T reduce(T indentify, BinaryOperator<T> accmulator)

<br>

e.g. 배열의 모든 요소의 합을 reduce() 연산으로 표현한 코드
```java
Arrays.stream(arr).reduce(0, (a,b) -> a + b);
```

- 첫 번째 요소는 초기값, 두 번째 요소는 람다식으로 원하는 기능 수행
- 람다식을 직접 구현하려면 `BinaryOperator` 인터페이스를 구현

