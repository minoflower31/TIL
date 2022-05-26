# 11060 점프 점프

   - jdk version: java11
   - 분류: 다이나믹 프로그래밍


<br>
<br>

# 해결 방식

이 문제의 핵심은 2가지! <br>

1. 점프 뛰지 못하는 구간을 고려하자.
2. 맨 오른쪽 끝 칸에만 도달하면 되므로 마지막 칸의 점프는 고려하지 않는다.


# Source Code

```java
import java.io.*;

class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = stoi(br.readLine());
        String[] s = br.readLine().split(" ");
        int[] dp = new int[n + 1];
        int[] a = new int[n + 1];
        for (int i = 1; i <=n ; i++) {
            dp[i] = -1;
            a[i] = stoi(s[i-1]);
        }

        dp[1] = 0;
        for (int i = 1; i <= n; i++) {
            if(dp[i] == -1) continue;

            for (int j = 1; j <= a[i]; j++) {
                if(i+j > n) break;

                if(dp[i+j] == -1 || dp[i+j] > dp[i]+1)
                    dp[i+j] = dp[i] + 1;
            }
        }
        System.out.println(dp[n]);
    }

    private static int stoi(String s) {
        return Integer.parseInt(s);
    }
}
```
