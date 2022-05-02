# 위상 정렬(Topological Sorting)

- 비순환 방향 그래프(DAG: Directed Acyclic Graph)를 순서대로 출력
- 순서에 추가되는 정점은 in-degree가 0일 때 표현가능
- BFS를 응용해서 구현 가능

<img width="568" alt="image" src="https://user-images.githubusercontent.com/56334513/166197225-8e777081-841a-47d9-bd96-f4fcebfaf958.png">

<br>

### 구현 방법

- **bfs**
  1. Queue, in-degree 1차원 배열
  2. in-degree가 0인 vertex를 queue에 삽입
  3. 큐에서 하나 꺼내어 해당 vertex가 가리키는 vertex를 찾기 위해 순차적으로 접근 (인접리스트)
  4. `ind[y]--`로 in-degree를 제거
  5. `if(ind[y]==0)`이라면 in-degree가 0이므로 queue에 삽입

<br>

- **dfs**: 인접 리스트를 반대로 저장(stack의 구조상 마지막 원소가 가장 처음에 위치하기 때문)
    1. check 1차원 배열
    2. `main`에서 check 배열로 방문하지 않았는지 확인
       - 아직 방문하지 않았다면 dfs 함수 호출
    3. 인접 리스트로 차례대로 접근하되, 아직 방문하지 않은 vertex일 때만 재귀
    4. _**주의**) 단방향이므로 for문이 끝난 이후에는 방문 체크를 false로 바꾸지 말 것!_

<br>

### 시간 복잡도

#### O(V + E)