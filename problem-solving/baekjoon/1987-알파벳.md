# 1987. 알파벳

    - jdk version: java11
    - 알고리즘 분류: dfs, backtracking

출처: https://www.acmicpc.net/problem/1987
<br> <br>

**dfs**

<img width="855" alt="image" src="https://user-images.githubusercontent.com/56334513/166616066-d208c2bc-c902-4084-a140-a8ca9862a2d6.png">

<br>

## 접근 방법

- 전형적인 `dfs`를 활용한 그래프 문제
- 하지만 더 이상 수행할 필요가 없는 경우(이미 방문한 알파벳 또는 범위를 벗어남)를 체크 = `백트래킹`
- (백트래킹을 제대로 수행하지 못해서 시간이 많이 걸린 것 같음)

<br>

### 고찰

시간이 100 ms 대가 걸리는 코드들은 `비트마스킹 기법`을 곁들던데 비트마스킹 공부 후에 적용해볼 것. 

# Source Code

```java
import java.util.*;

public class Main {
    static final int[] dx = {1, -1, 0, 0};
    static final int[] dy = {0, 0, 1, -1};
    static char[][] g;
    static boolean[] alphabet;
    static int r, c, ans;

    private static void init(Scanner sc) {
        r = sc.nextInt();
        c = sc.nextInt();
        sc.nextLine();
        g = new char[r][c];
        alphabet = new boolean[26];
        ans = 1;
        for (int i = 0; i < r; i++) {
            g[i] = sc.nextLine().toCharArray();
        }
    }

    private static void dfs(int i, int j, int cnt) {
        alphabet[g[i][j]-'A'] = true;
        int closeCnt = 0;

        for (int k = 0; k < 4; k++) {
            int nx = i + dx[k];
            int ny = j + dy[k];

            if ((nx < 0 || nx >= r || ny < 0 || ny >= c) || alphabet[g[nx][ny]-'A']) {
                closeCnt++;
                continue;
            }
            dfs(nx, ny, cnt + 1);
        }

        if(closeCnt == 4)
            ans = Math.max(ans, cnt);

        alphabet[g[i][j]-'A'] = false;
    }

    public static void main(String[] args) {
        init(new Scanner(System.in));
        dfs(0,0,1);
        System.out.println(ans);
    }
}
```
