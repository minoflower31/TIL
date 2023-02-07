# GINI야 도와줘

[문제 바로가기](https://softeer.ai/practice/info.do?idx=1&eid=395&sw_prbl_sbms_sn=137484)

    sdk: Java11
    분류: bfs

## 풀이 방법

코드도 코드지만 어처구니 없는 실수로 많은 시간을 잡아 먹은 문제이다. 다음과 같은 실수를 범했다.
- 지도의 입력으로 빈 칸이 주어지지 않는 문자열이였지만, 테스트케이스를 작성할 때 중간마다 한 칸씩 띄어서 입력값을 넣었다.
- 출발 위치를 W가 아닌 H로 설정했다.

문제 풀이는 다음과 같다.
- 소나기를 한 번 이동시킨 후 태범이를 이동하게 했다.
- while문의 조건은 taebum이 더 이상 이동할 위치가 없을 때까지로 설정했다.
  - 만약 소나기를 기준으로 할 경우, 소나기가 모두 확산할 때까지 반복이 형성되기 때문이다.

## 시간복잡도

시간복잡도는 `O(N^2)`이다.
  - 시작 위치와 집의 위치가 각각 끝지점에 위치해있다면 N^2만큼 이동해야 하기 때문이다.

## 소스 코드
```java
import java.util.*;
import java.io.*;


public class Main {

  private static char[][] g;
  private static final Queue<Integer> rainQueue = new LinkedList<>();
  private static boolean[][] rainVisited;
  private static int R, C;
  private static int[] dx = {-1,1,0,0};
  private static int[] dy = {0,0,-1,1};

  public static void main(String args[]) throws IOException {
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    StringTokenizer st = new StringTokenizer(br.readLine());
    R = Integer.parseInt(st.nextToken());
    C = Integer.parseInt(st.nextToken());

    g = new char[R][C];
    rainVisited = new boolean[R][C];
    List<Integer> rainList = new ArrayList<>();
    int sx = 0, sy = 0;
    for(int i = 0; i < R; i++) {
      String input = br.readLine();
      for(int j = 0; j < C; j++) {
        g[i][j] = input.charAt(j);
        if(g[i][j] == 'W') {
          sx = i;
          sy = j;
        } else if(g[i][j] == '*') {
          rainList.add(i);
          rainList.add(j);
        }
      }
    }

    for(int i = 0; i < rainList.size(); i+=2) {
      int x = rainList.get(i);
      int y = rainList.get(i+1);
      rainQueue.add(x);
      rainQueue.add(y);
      rainVisited[x][y] = true;
    }

    int answer = bfs(sx, sy);
    System.out.print((answer > -1) ? answer : "FAIL");
  }

  private static int bfs(int sx, int sy) {
    Queue<Integer> taebum = new LinkedList<>();
    boolean[][] taebumVisited = new boolean[R][C];
    taebum.add(sx);
    taebum.add(sy);
    taebumVisited[sx][sy] = true;

    int cnt = 0;
    while(!taebum.isEmpty()) {
      int rainSize = rainQueue.size() / 2;
      //소나기 한 번 이동
      for(int i = 0; i < rainSize; i++) {
        int x = rainQueue.poll();
        int y = rainQueue.poll();

        for(int k = 0; k < 4; k++) {
          int nx = x + dx[k];
          int ny = y + dy[k];

          if(nx < 0 || ny < 0 || nx >= R || ny >= C) continue;
          if(rainVisited[nx][ny] || g[nx][ny] == 'H' || g[nx][ny] == 'X') continue;
          rainQueue.add(nx);
          rainQueue.add(ny);
          rainVisited[nx][ny] = true;
        }
      }

      int taebumSize = taebum.size() / 2;
      //태범이 한 번 이동
      for(int i = 0; i < taebumSize; i++) {
        int x = taebum.poll();
        int y = taebum.poll();

        if(g[x][y] == 'H') {
          return cnt;
        }

        for(int k = 0; k < 4; k++) {
          int nx = x + dx[k];
          int ny = y + dy[k];

          if(nx < 0 || ny < 0 || nx >= R || ny >= C) continue;
          if(taebumVisited[nx][ny] || rainVisited[nx][ny] || g[nx][ny] == 'X') continue;
          taebum.add(nx);
          taebum.add(ny);
          taebumVisited[nx][ny] = true;
        }
      }
      cnt++;
    }
    return -1;
  }
}
```