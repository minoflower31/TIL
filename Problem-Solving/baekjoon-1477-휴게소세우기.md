# 1477. 휴게소 세우기

    - jdk version: Java11
    - 알고리즘 분류: 이분탐색

출처: https://www.acmicpc.net/problem/1477
<br>

<img width="1035" alt="image" src="https://user-images.githubusercontent.com/56334513/167601848-c42ac351-8d8b-4ed5-87f2-33e7eb617560.png">


<br>

## 접근 방법

+ **1 ≤ 휴게소의 위치 ≤ L-1 조건**과 **고속도로 길이 L**이 주어지므로 휴게소와 휴게소 사이(gap)를 구하기 위해 `휴게소의 위치`, `0`, `L`을 포함해야 한다.
  + 이분 탐색을 위해 배열을 정렬해줄 것!
+ 휴게소가 없는 구간의 최댓값의 최솟값 = 우리가 구해야 하는 답 = 이분탐색의 mid
  + 각 구간마다 `mid`를 나눈 몫에 휴게소를 세울 수 있다.
+ **모든 위치가 중복되지 않는다.** 만약 `gap`을 `mid`로 모듈러 연산을 할 때 0이라면 휴게소의 위치가 중복될 수 있다.
  + e.g. 200 ~ 400 구간에 휴게소를 세울 때 만약 `mid`가 100이라면?
    + gap은 200이고 mid가 100이므로 휴게소를 2개 세울 수 있는데, 하나는 300(200+100), 다른 하나는 400(300+100)에 세운다는 말이다. 하지만 휴게소의 위치는 중복될 수 없으므로 400을 제외시켜야 한다. 즉, `gap / mid - 1`의 식을 세울 수 있다.


+ **low = 0, high = L인 이유**
  + while문 조건으로 `low + 1 < high`를 해주었는데, 이는 항상 `low < mid < high` 를 만족하게 되며 low가 0이므로 mid는 항상 1 이상의 값을 가지게 된다.
  + `gap / mid`에서 runtime error(by zero)를 방지할 수 있다.
  + (만약 while문 조건이 `low <= high` 였다면 low = 1 로 설정해야 함)

<br>

### 고찰
while문 조건을 `low + 1 < high`를 적용해보니 아직까지는 헷갈리는 구석이 있지만 몇 문제 풀어보면 감이 익힐 것 같다. 앞으로의 이분 탐색은 저 틀에 벗어나지 않고 문제를 풀 계획이다.

# Source Code

```java
import java.util.*;

public class Main {
    static int N,M,L;
    static int[] rest;

    private static void init(Scanner sc) {
        N = sc.nextInt();
        M = sc.nextInt();
        L = sc.nextInt();
        rest = new int[N + 2];
        rest[0] = 0;
        rest[N+1] = L;
        for (int i = 1; i <= N; i++) {
            rest[i] = sc.nextInt();
        }
        Arrays.sort(rest);
    }

    private static boolean check(int mid) {
        int cnt = 0;
        for (int i = 1; i < N+2; i++) {
            int gap = rest[i] - rest[i-1];
            cnt += (gap % mid == 0) ? gap / mid -1 : gap / mid;
        }
        return cnt > M;
    }

    public static void main(String[] args) {
        init(new Scanner(System.in));

        int low = 0;
        int high = L;

        while (low + 1 < high) {
            int mid = (low+high)/2;
            if(check(mid)) low = mid;
            else high = mid;
        }
        System.out.println(high);
    }
}

```