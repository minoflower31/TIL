# 목차

- [Hash Table](#hash-table)
  - [충돌(Collision) 해결 알고리즘](#충돌(collision)-해결-알고리즘)
    - [Chaining 기법](#chaining-기법)
    - [Linear Proving 기법](#linear-proving-기법)
  
  - [시간 복잡도](#시간-복잡도)
<br>

- [목차](#목차)
- [Hash Table](#hash-table)
  - [충돌(Collision) 해결 알고리즘](#충돌collision-해결-알고리즘)
      - [Chaining 기법](#chaining-기법)
      - [Linear Probing 기법](#linear-probing-기법)
    - [빈번한 충돌을 개선하는 방법](#빈번한-충돌을-개선하는-방법)
    - [자바의 HashMap](#자바의-hashmap)
  - [시간 복잡도](#시간-복잡도)
- [Tree](#tree)
  - [이진 트리와 이진 탐색 트리](#이진-트리와-이진-탐색-트리)
  - [노드 삭제](#노드-삭제)
      - [Leaf Node 제거](#leaf-node-제거)
      - [Child Node가 하나인 Node 제거](#child-node가-하나인-node-제거)
      - [Child Node가 두 개인 Node 제거](#child-node가-두-개인-node-제거)
  - [시간 복잡도](#시간-복잡도-1)

<br>


# Hash Table

**키 값의 연산에 의해 데이터에 직접적으로 매핑하는 데이터 구조** <br>
- `Hashing function`을 통해서 키에 매핑되는 데이터를 배열에 저장하는 주소(=인덱스)를 계산 
- 미리 배열을 할당하여 키가 가리키는 슬롯에 데이터 저장 및 탐색 <br>

<br>

**용어** <br>
- Hashing Fuction: 임의의 데이터를 고정된 길이 값으로 반환하는 함수
- Slot: 해쉬 테이블 내부에 데이터를 저장하는 한 공간

<br>

**Simple Hashing Function** <br>
- Key가 문자열이라면 맨 앞 문자를 숫자로 변환 후, Division 기법을 사용하여 key에 대한 주소를 사용
     - Division 기법: 나머지 연산을 이용한 기법

<br>

**장점** <br>
- 데이터 저장/읽기 속도가 빠름
- 데이터 중복 확인이 쉬움
     - 배열은 첫 번째 인덱스부터 마지막까지 순회해서 데이터 중복을 찾는 것과 비교됨

<br>

**단점** <br>
- 공간 복잡도가 커짐
     - 객체 배열을 초기화할 때 미리 공간을 할당해야 함
<br>
- 여러 키에 해당하는 주소가 동일하다면 충돌을 해결하는 별도의 자료구조 필요
     - e.g. ParkHo 키에 값 저장, Park 키에 값 저장 후 ParkHo 키가 가진 값을 불러오면 Park 값이 나타나는 충돌 현상

<br>

**용도** <br>

- 주로 검색에 쓰임
- 저장, 삭제, 읽기가 빈번할 때
- ache 구현 시 사용


<br>
<br>

## 충돌(Collision) 해결 알고리즘

#### Chaining 기법 ####

    - Open Hashing 기법 중 하나로, 해쉬 테이블 저장공간 외의 공간을 활용하는 기법
    - 충돌이 일어난 경우 LinkedList를 이용해서 해당 키 값에 추가로 연결하는 기법

<br>

#### Linear Probing 기법 ####
    - Close Hashing 기법 중 하나로, 해쉬 테이블 내부에서 충돌을 해결하는 기법
    - 충돌이 일어난 경우 hash address의 다음 address가 비었을 경우 저장함

<br>

### 빈번한 충돌을 개선하는 방법

해쉬 테이블 저장공간을 확대하거나 해쉬 함수를 재정의함 <br>


e.g. `return key % 8` -> `return key % 200` <br>

<br>

### 자바의 HashMap
해쉬 테이블 구조를 활용하여 구현된 Java Collection 중 하나 <br>

<br>
<br>

## 시간 복잡도

- 충돌이 없는 경우: O(1)
- 총돌이 모두 발생한 경우(최악): O(N)
    - 배열 전체를 돌려야 하기 때문
- Linear Probing, Chaining 기법 둘 다 동일함

<br>
<br>
<br>

# Tree

**Node와 Branch를 이용하고 여러 노드가 한 노드를 가리킬 수 없는, 즉 사이클이 존재하지 않는 데이터 구조** <br>

- 응용: 이진 탐색 트리 

<br>

**용어** <br>
- Node: 트리에 데이터를 저장하는 요소(data, branch)
- Root Node: 최상위 노드
- Level: Root Node를 level 0으로 설정하여 최하위 노드까지 층으로 나눈 것
- Parent Node, Child Node, Leaf Node
- Sibling: 동일한 Parent Node를 가진 노드들
- Depth: 트리에서 노드가 가질 수 있는 최대 Level

<br>
<br>

## 이진 트리와 이진 탐색 트리

- 이진 트리: 노드의 최대 branch가 2개인 트리
- **이진 탐색 트리(Binary Search Tree, BST)**: 하나의 특정 노드가 존재하면 왼쪽에는 해당 노드보다 작고, 오른쪽에는 해당 노드보다 큰 트리

<br>

> <br>
> 
> BST에서 삽입 시
> <br>
> 
<br>

![](https://blog.penjee.com/wp-content/uploads/2015/11/binary-search-tree-insertion-animation.gif)

<br>

> <br>
> BST와 Sorted Array 탐색 비교
> <br>
> 
<br>

![](https://blog.penjee.com/wp-content/uploads/2015/11/binary-search-tree-sorted-array-animation.gif)

<br>
<br>

## 노드 삭제

#### Leaf Node 제거

> <br>
> 
> 제거할 Node의 Parent Node가 제거 Node를 가리키지 않도록
> <br>

<br>

#### Child Node가 하나인 Node 제거

> <br>
> 
> 제거할 Node의 Parent Node가 제거 Node의 Child Node를 가리키도록
> <br>

<br>

#### Child Node가 두 개인 Node 제거

> <br>
> 
> - 삭제할 노드의 오른쪽 자식 노드 중에서 가장 왼쪽 노드와 change
>   1. 삭제 노드의 오른쪽 자식 노드 선택
>   2. 오른쪽 자식 노드가 가진 맨 왼쪽의 노드(=change할 노드)를 선택
>   3. 삭제 노드가 가진 부모 노드의 왼쪽 branch가 change할 노드를 가리키게 함 
>   4. change한 노드가 삭제 노드의 왼쪽 자식 노드를 가리킴
>   5. change한 노드가 삭제 노드의 오른쪽 자식 노드를 가리킴
>   <br>
>   -  단, change할 노드가 오른쪽 자식 노드를 가질 경우, 오른쪽 자식 노드를 change할 노드로 삼고    3~5 단계 진행
> <br>
> - 삭제할 노드의 왼쪽 자식 노드 중에서 가장 오른쪽 노드와 chagne
> <br>

- 만약 변경할 노드의 우측 노드에 자식 노드들이 존재해도 알고리즘 기반에 의해 트리에 영향 X


<br>
<br>

## 시간 복잡도

- depth가 h일 때 O(h)
- n개의 노드를 가진다면 h = logN 에 가까우므로 시간 복잡도는 O(logN)
  - 한 번 실행할 때마다 절반의 명령만 수행한다는 의미. 즉 실행 시간을 절반 단축시킴

- **단점**
  - 균형 잡힌 트리는 O(logN)의 시간 복잡도를 가지지만, 최악의 경우에는 LinkedList와 같은 구조를 가지게 돼 O(N) 시간복잡도가 된다.