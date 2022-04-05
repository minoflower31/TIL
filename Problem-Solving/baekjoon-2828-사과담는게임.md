## 2828. 사과 담기 게임

**jdk version: java 11**

출처: https://www.acmicpc.net/problem/2828

### Greedy

sp: 상자 시작점
ep: 상자 끝점
prevSp: 이전의 시작점
move: 이동 횟수

`if(sp < apple)`: 사과가 상자 시작점보다 앞에 있을 때
    - `sp = 사과의 위치` // 상자 시작점과 사과의 위치가 같아야 이동 최소화
    - `ep = sp + 상자의 길이 - 1`
<br>

`if(ep > apple)`: 사과가 상자 끝점보다 뒤에 있을 때
    - `ep = 사과의 위치` // 상자 끝점과 사과의 위치가 같아야 이동 최소화
    - `sp = ep - 상자의 길이 + 1`

<br>

`Math.abs(sp - prevSp)`: 이전의 시작점으로부터 이동한 시작점까지의 거리

<br>
<br>

## Souce Code

```java
import java.util.Scanner;

class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        int j = sc.nextInt();

        int sp = 1, ep = sp + m -1, prevSp = sp;
        int move = 0;

        for (int i = 0; i < j; i++) {
            int apple = sc.nextInt();

            if(sp > apple || ep < apple) {
                if(sp > apple) {
                    sp = apple;
                    ep = sp + m -1;
                } else {
                    ep = apple;
                    sp = ep - m + 1;
                }
                move += Math.abs(sp - prevSp);
                prevSp = sp;
            }
        }
        System.out.println(move);
    }
}
```
