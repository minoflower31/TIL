# 9934. 완전 이진 트리

    - jdk version: java11
    - 알고리즘 분류: 그래프(인접리스트), dfs

출처: https://www.acmicpc.net/problem/13023
<br> <br>
<img width="939" alt="image" src="https://user-images.githubusercontent.com/56334513/166192597-0c398880-ac9d-458b-87f5-532d56db89cf.png">

<br>

## 접근 방법



<br>

### 시간을 단축하다

1. 재귀에서 답을 찾았을 경우, return해주는 것이 아닌 `System.exit(0)` 호출로 바로 종료하기
2. `split()` 함수 대신 `StringTokenizer` 사용.
   - 구분자가 공백밖에 없으므로 속도 및 안정성에서 더 효과적이기 때문.
   - **코드 라인이 길어지더라도 혹은 귀찮더라도, `StringTokenizer`를 사용하자!** 

<br>

### 다른 풀이 방법?

- java11 기준으로 1위를 달성한 코드는 `인접 리스트`를 `Node(int data, Node next)` 객체 배열로 품.

<br>

# Source Code

```java
import java.io.*;
import java.util.*;

class Main {
    static List<ArrayList<Integer>> gList = new ArrayList<>(2000);
    static boolean[] c = new boolean[2000];
    static int n, m;

    private static void dfs(int person, int cnt) {
        if (cnt == 4) {
            System.out.println(1);
            System.exit(0);
        }

        c[person] = true;
        for (int i : gList.get(person)) {
            if(c[i]) continue;
            dfs(i, cnt + 1);
        }
        c[person] = false;
    }


    private static void init(BufferedReader br) throws IOException {
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());

        for (int i = 0; i < n; i++) {
            gList.add(new ArrayList<>());
        }

        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            gList.get(a).add(b);
            gList.get(b).add(a);
        }
    }
    
    public static void main(String[] args) throws IOException {
        init(new BufferedReader(new InputStreamReader(System.in)));
        for (int i = 0; i < n; i++) {
            dfs(i, 0);
        }
        System.out.println(0);
    }
}
```