# 1477. 휴게소 세우기

    - jdk version: Java11
    - 알고리즘 분류: 구현, 자료구조, 덱

출처: https://www.acmicpc.net/problem/3190
<br>

<img width="1035" alt="image" src="https://user-images.githubusercontent.com/56334513/168032291-ea5f721f-e742-480e-aa5d-de2ebc69747b.png">


<br>

## 접근 방법

`질문 검색` 탭에 보면 느낄 수 있듯이 문제의 표현이 명확하지 않아 이해하는 데 다소 오래 걸리는 문제 <br>
게임이 진행되는 순서는 다음과 같다.
1. 매 초마다 이동
   + 시간이 흐르고 이동하는 것이므로 while문 상단에 time++;
2. 뱀의 몸길이를 늘려 머리를 다음칸에 이동
   + 여기서 벽에 부딫히진 않는지, **머리가 몸통에 닿지 않았는지** check.
3. 이동칸에 사과가 있으면 사과를 먹었으므로 없애야 함
4. 이동칸에 사과가 없으면 꼬리를 제거

<br>

`Deque` 자료구조를 이용하면 뱀의 머리부터 꼬리까지 간단하게 표현 가능하다. <br>
단, 코드의 순서에 따라 결과값이 달라지므로 주의!

<br>

## 고찰
1. 90도로 왼쪽 회전, 오른쪽 회전이 나온다면 (이것 때문에 시간잡아먹지 말자)
```java
//0: 북, 1: 동, 2:남, 3:서
static int[] dx = {-1, 0, 1, 0};
static int[] dy = {0, 1, 0, -1};

//왼쪽으로 90도 회전
direct = (direct + 3) % 4;
//오른쪽으로 90도 회전
direct = (direct + 1) % 4;
```
2. 사용자 정의 객체로 `contain()` 함수를 사용한다면 `equals`, `hashcode`를 오버라이딩해야 한다.(이거때문에 시간을 얼마나 날려먹었는지...)

# Source Code
```java
import java.io.*;
import java.util.*;

public class Main {

    static int N, K, L;
    static Deque<Pos> deque = new ArrayDeque<>();
    static boolean[][] apple;
    static char[] rotate;
    static int[] dx = {-1, 0, 1, 0};
    static int[] dy = {0, 1, 0, -1};

    private static void init(BufferedReader br) throws IOException {
        N = stoi(br.readLine());
        K = stoi(br.readLine());
        apple = new boolean[N+1][N+1]; // T: 사과 o, F: 사과 x
        for (int i = 0; i < K; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            apple[stoi(st.nextToken())][stoi(st.nextToken())] = true;
        }

        rotate = new char[10001];
        L = stoi(br.readLine());
        for (int i = 0; i < L; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            rotate[stoi(st.nextToken())] = st.nextToken().charAt(0);
        }
    }

    private static int solve() {
        deque.add(new Pos(1, 1)); // 시작
        int ans = 0;
        int direct = 1; //북: 0, 동: 1, 남: 2, 서: 3
        int tx, ty; //tx, ty 초기화(1을 대입하면 check()에서 false)
        while (true) {
            Pos head = deque.peekLast();
            tx = head.x + dx[direct];
            ty = head.y + dy[direct];

            ans++;
            if(tx < 1 || tx > N || ty < 1 || ty > N) break;
            //if(deque.contains(new Pos(tx,ty))) break;
            if(isContain(tx,ty)) break;

            deque.addLast(new Pos(tx, ty));
            if (apple[tx][ty]) apple[tx][ty] = false;
            else deque.removeFirst();

            if (rotate[ans] == 'L' || rotate[ans] == 'D') {
                if (rotate[ans] == 'L') direct = (direct == 0) ? 3 : direct - 1;
                else direct = (direct + 1) % 4;
            }
        }
        return ans;
    }

    private static int stoi(String s) {
        return Integer.parseInt(s);
    }

    private static boolean isContain(int x, int y) {
        for (Pos pos : deque) {
            if(pos.x == x && pos.y == y) return true;
        }
        return false;
    }

    public static void main(String[] args) throws IOException {
        init(new BufferedReader(new InputStreamReader(System.in)));
        System.out.println(solve());
    }

    private static class Pos {
        int x;
        int y;

        public Pos(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
}

```