# 9934. 완전 이진 트리

    - jdk version: java11
    - 알고리즘 분류: 자료구조, 트리, 재귀

문제 출처: https://www.acmicpc.net/problem/9934

<br>

문제 조건은 중위 순서(in-order)를 뜻한다. <br>

주어진 입력값 자체를 중위 순서로 배열에 저장. <br>
<br>

#### another solution

이분탐색처럼 start, end로 재귀를 돌리는 풀이가 있던데, 현재 코드에서 걸리는 시간보다 개선된다면 코드 추가하기.

# Source Code

```java
import java.util.*;

class Main {

    static Scanner sc = new Scanner(System.in);
    static int[] a;
    static boolean[] v;

    private static void infix(int idx) {
        if(idx >= a.length) return;
        if(v[idx]) return;
        infix(idx * 2);
        a[idx] = sc.nextInt();
        v[idx] = true;
        infix(idx *2 + 1);
    }

    public static void main(String[] args) {
        int n = (int) Math.pow(2,sc.nextInt())-1;
        a = new int[n+1];
        v = new boolean[n + 1];
        infix(1);

        StringBuilder sb = new StringBuilder();
        int pow = 2;
        for (int i = 1; i <= n; i++) {
            if(i == pow) {
                sb.append("\n");
                pow *= 2;
            }
            sb.append(a[i]).append(" ");
        }
        System.out.println(sb);
    }
}
```