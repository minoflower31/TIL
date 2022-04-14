## Switch문 (java14)

```java
int month = 3;

int day = switch(month) {
	case 1,3,5 -> {
		yield 31;	
	}
}
```

<br>
<br>

# Static

## Static 변수

#### 여러 인스턴스에서 공통으로 사용하는 변수

- 프로그램이 메모리에 상주할 때 크게 **code 영역, data(=static)  영역, heap 영역, stack 영역**으로 나뉜다.
     - 이 때, data 영역에 literal이나 static 변수가 포함된다.

<br>
<br>

## Static 메서드

- 인스턴스 변수(또는 인스턴스 메서드)를 사용할 수 없다.
     - 이유:  우선 순위가 `static > instance` 이기 때문. instance 메서드에서 static 변수 및 메서드는 사용 가능


|변수 유형|선언 위치|사용 범위|메모리|생성과 소멸|
|-----|--------|--------------|---|----------------|
|지역 변수(로컬 변수)|함수 내부|함수 내부|스택|함수가 호출될 때 생성, 반환하면 소멸함.|
|멤버 변수(인스턴스 변수)|클래스 멤버 변수|클래스 내부에서 사용|힙|인스턴스가 생성될 때 힙에 생성, 가비지 컬렉터가 메모리 수거할 시 소멸|
|static 변수(클래스 변수)|static 예약어 사용|클래스 내부 or 외부에서 사용|데이터|프로그램이 처음 시작할 때 상수와 함께 데이터 영역에 생성. 프로그램이 끝나고 메모리를 해제 시 소멸

<br>
<br>

## Static - singleton pattern

#### 인스턴스를 단 하나만 생성해야 할 경우
<br>

```java
public Singleton {
	private static Singleton instance = new Singleon();

	private Singleton() {}

	public Singleton getInstance() {
		return instance;
	}
}
```

<br>
<br>

## 배열

- 동일한 자료형의 순차적 자료구조
- 물리적 위치와 논리적 위치가 같음
     - 0번째 위치 옆에 1번째 위치(논리적 위치)가 실제로 메모리에 순서대로 상주됨(물리적 위치)
<br>
<br>

### 객체 배열 선언

- 기본 자료형 배열은 선언과 동시에 배열의 크기만큼 메모리에 할당
- 하지만 객체 배열은 객체의 주소(초기: null)가 들어갈(4byte or 8byte) 메모리만 할당되기 때문에 배열의 크기만큼 객체를 초기화해야 함.

<br>
<br>

### 객체 배열 복사하기 (얕은 복사)

`System.arraycopy(원본, 원본_위치, 바꿀 대상, 바꿀 대상_위치, 복사 크기);` <br>
`Arrays.copyOf()` <br> 

<br>
<br>

## 얕은 복사 vs. 깊은 복사 (객체)

#### 얕은 복사

- 복사 과정에서 참조 변수가 지닌 주소값이 그대로 복사(=동일한 참조값을 가리킴).
    - `a=b` 후, a의 인스턴스 변수를 변경하면 b도 변경됨

<br>

#### 깊은 복사

- 방법1: getter, setter 이용
     - 인스턴스 a, 인스턴스 b 존재할 때, b.setAge(a.getAge()) 로 가능

- **방법2: Cloneable 이용**
     - 윤성우의 열혈 자바프로그래밍 책 참조
     - 정리 링크: 

<br>
<br>

## 묵시적 타입 변환(Promotion)과 명시적 타입 변환(Casting)

#### 묵시적 타입 변환

- 타입이 자동으로 변환
- byte -> short, char -> int -> long(8byte) -> float(4byte) -> double

<br>

#### 명시적 타입 변환

- 큰 크기의 데이터 -> 작은 크기의 데이터로 변환
- e.g. `double b = 3.14;` `int a = (int) b`
- Casting

<br>

### 용어 정리

- **Type Conversion**: 형 변환(묵시적+명시적을 모두 아우르는 말)
- **Promotion**: 승격의 의미
- **Casting**: 배역 선정, 강제로 타입이 변환되는 것
