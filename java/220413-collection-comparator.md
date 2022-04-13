# Collection Framework

#### 프로그램 구현에 필요한 자구조를 구현해놓은 JDK 라이브러리

- **왜 필요해?**
  - 개발에 소요되는 시간을 절약하면서 최적화된 알고리즘을 사용할 수 있기 때문

## 인터페이스 종류

![image](https://user-images.githubusercontent.com/56334513/163203059-4723b90d-94de-4880-9437-9a97708f3a23.png)

출처: https://techvidvan.com/tutorials/java-collection-framework/
<br>
<br>
<br>

## `List<E>`

- 인스턴스의 저장 순서를 유지한다.
- 동일한 인스턴스의 중복 저장을 허용한다.
- c.f. 참조변수를 인터페이스를 선언하는 이유는 **다형성** 때문! 

<br>

### ArrayList vs. LinkedList

|       |ArrayList|LinkedList|
|-------|------------|------------|
|저장공간 늘리기|시간이 비교적 많이 소요|단순|
|삭제|O(n)|O(1)|
|탐색|O(1)|O(n)|

<br>

#### 저장 공간

**ArrayList**: 필요 시 배열의 길이를 스스로 늘림 -> 새로운 배열로 교체
+ default capacity는 10이다. 10을 초과할 경우, 10을 더한 20으로 확장함
+ 성능에 신경써야 하면 capacity로 미리 공간 확보

**LinkedList**: 노드 하나만 추가 <br>
<br>

#### 탐색

**ArrayList**: index로 접근 <br>
**LinkedList**: head부터 순차적으로 접근 <br>
<br>

#### 삭제

**ArrayList**: 삭제된 위치를 비워두지 않기 위해 그 뒤의 저장돼있는 인스턴스들을 한 칸씩 앞으로 이동 <br>
**LinkedList**: 삭제하고자 하는 위치의 이전 노드와 다음 노드만 연결해주면 됨 <br>
<br>

c.f. ArrayList가 Array보다 좋은 이유?
- Array는 저장 공간을 미리 할당하는데, 만약 1000을 할당한 상태에서 2개의 공간만 차지한다면 공간 낭비

<br>
<br>

## 저장된 인스턴스의 순차적인 접근 방법

### 1. enhanced-for

#### 저장된 모든 인스턴스들을 대상으로 조회할 경우

+ Iterable 인터페이스를 상속한 Collection 인터페이스를 구현한 경우 Ok.
  + ArrayList, LinkedList, HashSet...
+ 컴파일 과정에서 iterator 방식으로 수정됨

<br>

### 2. Iterator

#### 모든 인스턴스를 순차적으로 접근 + `삭제 가능`

- 저장된 인스턴스를 가리킴 (지팡이에 비유)

<br>

### 3. ListIterator

#### 양방향으로 순차적으로 접근 가능

- **iterator 기능** + `hasPrevious(), previous(), add(E e), set(E e) ...`

<br>
<br>

## Set<E>

- 인스턴스의 저장 순서를 유지하지 않는다.
- 동일한 인스턴스의 중복 저장을 허용하지 않는다.
  - 사용자 정의 객체 이용시, 중복 여부 체크

<br>

### HashSet

- **사용자 정의 객체를 저장할 경우, 동일성 확인을 위해 equals()와 hashCode()를 override.**
- (IntelliJ에서는 자동 완성시켜줌)

```java
//Student: id, name

@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Student student = (Student) o;
    return id == student.id && Objects.equals(name, student.name);
}

@Override
public int hashCode() {
  return Objects.hash(id, name);
}
```

<br>

### TreeSet

#### 인스턴스의 정렬을 위해 사용되는 클래스

- 내부적으로 BST가 구현됨
- 특정 조건에 맞는 정렬을 원할 경우, `Comparable이나 Comparator 인터페이스` 사용

<br>
<br>

## Comparable과 Comparator 구현

### Comparable

- 사용자 정의 객체에 `Comparable<사용자 정의 객체>`를 구현
  - e.g. `public class Member implements Comparable<Member>`
- String, Integer 등 래퍼 클래스는 이미 구현돼있음
- `compareTo(Object o)`

<br>

### Comparator

- 사용자 정의 객체 `Comparator<사용자 정의 객체>`를 구현
- `compare(Object o1, Object o2)
- **compare 함수를 override해서 사용할 경우, 사용하고자 하는 컬렉션을 초기화할 때 내가 정의한 Comparator 객체를 생성자의 인자로 넣어준다**
  - e.g. `TreeSet<Member> set = new TreeSet<>(new MyComp())`

<br>
<br>

### 적절한 사용법

#### Comparable

**Comparable이 implements가 되지 않은 객체 (e.g. 사용자 정의 객체)** <br>

<br>

#### Comparator

**이미 Comparable이 구현됐고 Comparator로 비교하는 방식을 다시 구현 가능**
- (내용 더 추가할 것)

```java
public class Main {

    public static void main(String[] args) {

        Set<String> set = new TreeSet<>(new MyCompare());

        set.add("자바");
        set.add("파이썬");
        set.add("씨");
        set.add("씨플플");

        System.out.println(set);
    }
}

class MyCompare implements Comparator<String> {

    @Override
    public int compare(String o1, String o2) {
        return o1.compareTo(o2) * -1; //내림차순
    }
}
```

<br>
<br>

## `Map<K,V>`

#### key와 value로 구성된 데이터 구조(key 중복 허용X)

- (HashTable과 HashMap의 성능 차이)
- keySet: `Set<K>`
- values: `Collection<V>`

<br>

### TreeMap

#### key에 대한 정렬을 구현할 수 있는 클래스

key가 되는 클래스에 Comparable 또는 Comparator를 구현함으로써 key 값 기준으로 key-value 형태를 정렬

