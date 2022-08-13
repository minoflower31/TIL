# 16985. Maaaaaaaaaze

    - jdk version: java 11
    - 분류: 구현

출처: https://www.acmicpc.net/problem/16927
<br>

<img width="855" alt="image" src="https://user-images.githubusercontent.com/56334513/171090251-42cf6052-aa2a-44fb-bcf7-4c5cba5be76e.png">

뿌듯..😆

<img width="848" alt="image" src="https://user-images.githubusercontent.com/56334513/171090306-5010e912-2c24-4b70-b871-24a5dfbf6935.png">

<br>
<br>


## 소스 코드

```java
import java.io.*;
import java.util.*;

class Main {
    static int[][][] arr;
    static int[][][] userArr;
    static boolean[] c;
    static int[] a;
    static int n = 5;
    static int ans;
    static int[] dx = {-1, 1, 0, 0, 0, 0};
    static int[] dy = {0, 0, -1, 1, 0, 0};
    static int[] dz = {0, 0, 0, 0, -1, 1};


    private static class Pos {
        int x, y, z;

        public Pos(int x, int y, int z) {
            this.x = x;
            this.y = y;
            this.z = z;
        }
    }

    public static void main(String[] args) throws IOException {
        initVariables();
        order(0);
        System.out.println(ans);
    }

    private static void initVariables() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        arr = new int[n][n][n];
        a = new int[n];
        c = new boolean[5];

        ans = -1;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                st = new StringTokenizer(br.readLine());
                for (int k = 0; k < n; k++) {
                    arr[i][j][k] = Integer.parseInt(st.nextToken());
                }
            }
        }
    }

    private static void order(int idx) {
        if (idx == n) {
            userArr = new int[n][n][n];
            //순서에 맞게
            for (int i = 0; i < n; i++) {
                userArr[a[i]] = arr[i];
            }

            performBfsAndRotate();
            if(ans == 12) {
                System.out.println(ans);
                System.exit(0);
            }
            return;
        }

        for (int i = 0; i < n; i++) {
            if (c[i]) continue;
            c[i] = true;
            a[idx] = i;
            order(idx + 1);
            c[i] = false;
        }
    }

    private static void performBfsAndRotate() {
        for (int i = 0; i < n-1; i++) {
            userArr[0] = rotate(userArr[0]);
            if(userArr[0][0][0] == 0) continue;
            for (int j = 0; j < n-1; j++) {
                userArr[1] = rotate(userArr[1]);
                for (int k = 0; k < n-1; k++) {
                    userArr[2] = rotate(userArr[2]);
                    for (int l = 0; l < n-1; l++) {
                        userArr[3] = rotate(userArr[3]);
                        for (int m = 0; m < n-1; m++) {
                            userArr[4] = rotate(userArr[4]);
                            if(userArr[4][4][4] == 0) continue;

                            // bfs 수행
                            int temp = bfs();
                            if(temp != -1) {
                                if (ans == -1 || ans > temp)
                                    ans = temp;
                            }
                        }
                    }
                }
            }
        }
    }

    private static int bfs() {
        Queue<Pos> q = new LinkedList<>();
        int[][][] dist = new int[n][n][n];

        q.add(new Pos(0, 0, 0));
        dist[0][0][0] = 1;

        while (!q.isEmpty()) {
            Pos pos = q.poll();
            int x = pos.x;
            int y = pos.y;
            int z = pos.z;

            for (int i = 0; i < 6; i++) {
                int nx = x + dx[i];
                int ny = y + dy[i];
                int nz = z + dz[i];

                if (nx < 0 || ny < 0 || nz < 0 || nx >= n || ny >= n || nz >= n) continue;
                if (dist[nx][ny][nz] != 0 || userArr[nx][ny][nz] == 0) continue;

                dist[nx][ny][nz] = dist[x][y][z] + 1;
                q.add(new Pos(nx, ny, nz));
            }
        }
        return dist[n-1][n-1][n-1]-1;
    }

    private static int[][] rotate(int[][] descArr) {
        int[][] newArr = new int[n][n];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                newArr[i][j] = descArr[n - j - 1][i];
            }
        }
        return newArr;
    }
}
```