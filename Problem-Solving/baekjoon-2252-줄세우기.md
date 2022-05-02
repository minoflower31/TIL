# 9934. 완전 이진 트리

    - jdk version: java11
    - 알고리즘 분류: 그래프(인접리스트), 위상정렬

출처: https://www.acmicpc.net/problem/2252
<br> <br>
<img width="1038" alt="image" src="https://user-images.githubusercontent.com/56334513/166202949-c55b4303-2a68-40be-b8d8-51a9fbf4af37.png">

<br>
<br>

## 접근 방법

- 전형적인 **위상 정렬** 문제
  - `queue`를 활용한 `bfs`로 접근
  - **in-degree**가 0일 때만 queue에 삽입
  - 인접리스트가 가장 효율적. (u -> v 관계인 일차원 배열을 떠올려봤으나 전체를 순환해야 하는 단점을 발견.)

<br>
  
- **왜 위상 정렬으로 풀어야 하는가?**
  - 키의 순서대로 나열한다는 것은 비순환이어야만 하고 단방향이어야 함. 이는 **DAG**를 의미.
  - 부분적으로 키를 정렬하되, 정답이 여러 개면 아무거나 출력하면 되므로 문제에 주어지지 않는 관계는 모두 0으로 초기화가 되어서 상관없음.

<br>

### 시간을 단축하다

- MAX+1이 아닌 n+1.
  - 초기화해야 할 배열 또는 컬렉션이 많음. 또한, 그래프를 표현할 때 n이 매우 작다면 쓸데없는 공간 차지.
- `List<ArrayList<Integer>>` 대신 `ArrayList<Integer>[]` 로 초기화
  - 공간을 미리 할당할 수 있는 장점과 `get()` 함수로 접근하는 대신 배열의 접근 방식인 `[]`로 접근 가능
  - 코드 길이를 조금 줄일 수 있음

<br>

- **시간복잡도**는 `위상정렬 O(V+E)`
<br>

# Source Code

```java
import java.io.*;
import java.util.*;

class Main {

    static int n, m;
    static int[] ind;
    static ArrayList<Integer>[] g;
    static Queue<Integer> q;

    private static void init(BufferedReader br) throws IOException {
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");

        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());

        g = new ArrayList[n + 1];
        ind = new int[n + 1];
        q = new LinkedList<>();

        for (int i = 1; i <= n; i++) {
            g[i] = new ArrayList<>();
        }

        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            int u = Integer.parseInt(st.nextToken());
            int v = Integer.parseInt(st.nextToken());
            g[u].add(v);
            ind[v]++;
        }

        for (int i = 1; i <= n; i++) {
            if (ind[i] == 0) q.add(i);
        }
    }

    private static String bfs() {
        StringBuilder sb = new StringBuilder();

        while (!q.isEmpty()) {
            int x = q.poll();
            sb.append(x).append(" ");

            for (int i : g[x]) {
                ind[i]--;
                if (ind[i] == 0) q.add(i);
            }
        }
        return sb.toString();
    }

    public static void main(String[] args) throws IOException {
        init(new BufferedReader(new InputStreamReader(System.in)));

        System.out.println(bfs());
    }
}
```