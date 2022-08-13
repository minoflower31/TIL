# 11660. 구간 합 구하기 5

    - jdk version: java 11
    - 분류: 누적합, DP

출처: https://www.acmicpc.net/problem/11660
<br>

<img width="1038" alt="image" src="https://user-images.githubusercontent.com/56334513/169986461-48c8c8bb-b7ba-4a4b-a306-b079a9e821a4.png">

<br>
<br>

## 접근 방법

+ N이 1024이고 M이 100,000까지므로, M번 수행할 때마다 N*N 배열에서 값을 찾으면 시간 초과가 발생할 것을 예상하여 미리 배열에 문제 조건에 맞는 값을 대입해야 함.
  + `다이나믹 프로그래밍` 으로 해결
+ `dp[i][j] += dp[i][j - 1] + dp[i - 1][j] - dp[i - 1][j - 1];`
  + 현재 좌표를 기준으로 왼쪽 값과 오른쪽 값은 (i-1, j-1)를 둘 다 포함하고 있으므로 한 번 빼줘야 함.
+ `dp[x2][y2] - dp[x2][y1 - 1] - dp[x1 - 1][y2] + dp[x1 - 1][y1 - 1]`
  + `dp[x2][y2]`: 최종적으로 더해진 값
  + 하나의 사각형을 생각해봤을 때,
  + `dp[x2][y1-1]`: 좌측의 `x좌표`는 항상 `x2`여야 맨 아래를 가리킬 수 있고, `y좌표`는 y1일 때 맨 왼쪽을 가리킨다. 즉, 대각선 아래 왼쪽을 가리킨다. `y1-1`인 이유는 (x2,y1)에서 (x2,y1-1)에 있던 값을 빼주기 위함.
  + `dp[x1-1][y2]`: 좌측의 `x좌표`는 항상 `x1`이어야 맨 위를 가리킬 수 있고, `y좌표`는 y2일 때 맨 오른쪽을 가리킨다. 즉, 대각선 위 오른쪽을 가리킨다.

<br>

## 고찰

+ dp문제는 아직까지 점화식을 세우는 데 시간이 많이 걸린다.
+ 예제를 기반으로 2~3개의 테스트 케이스를 거쳐 빠르게 점화식을 세우도록 연습하자!

## 소스 코드

```java
import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int N = stoi(st.nextToken());
        int M = stoi(st.nextToken());

        int[][] dp = new int[N + 1][N + 1];
        for (int i = 1; i <= N; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            for (int j = 1; j <= N; j++) {
                dp[i][j] = stoi(st.nextToken());
                dp[i][j] += dp[i][j - 1] + dp[i - 1][j] - dp[i - 1][j - 1];
            }
        }

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            int x1 = stoi(st.nextToken());
            int y1 = stoi(st.nextToken());
            int x2 = stoi(st.nextToken());
            int y2 = stoi(st.nextToken());


            sb.append(dp[x2][y2] - dp[x2][y1 - 1] - dp[x1 - 1][y2] + dp[x1 - 1][y1 - 1]).append("\n");
        }
        System.out.println(sb);
    }

    private static int stoi(String s) {
        return Integer.parseInt(s);
    }
}
```