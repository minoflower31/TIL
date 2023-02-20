# 5525. IOIOI

    - jdk version: java 11
    - 분류: 구현

출처: https://www.acmicpc.net/problem/5525
<br>

<img width="958" alt="image" src="https://user-images.githubusercontent.com/56334513/220020292-d7b90d34-419f-4f6a-93e7-685855b7d9f9.png">

<br>
<br>

## 접근 방법

- N, M이 최대 백만이기 때문에 O(N * M)보다 O(N) 또는 O(M)의 시간복잡도가 나와야 함
- 브루트포스로 해결함
  - i를 기준으로 i 위치에서 I, i+1 위치에서 O, i+2 위치에서 I이라면 문제 조건에 따라 N = 1을 충족하므로 카운팅함. 그리고 다음 문자열 탐색을 위해 i+=2를 해줌
  - 만약 아니라면 현재까지 카운팅한 수는 무의미해지므로 0으로 초기화함. 그리고 i는 1 증가하게 함.
  - 현재까지 카운팅한 수와 N이 동일하면 정답에 하나를 더하고, 카운팅에 하나를 빼줌
    - 빼는 이유는 다음 i의 위치에서도 계속 카운팅한다면 누적되기 때문임
    - 예를 들어, N = 2이고 cur = 2를 만족하면 answer를 하나 카운팅할 때, 다음 위치에서 cur = 3이 되어 N = 2인 조건을 벗어남. 그러므로 cur에 1을 차감해서 N과 같도록 함 

<br>

시간 복잡도는 O(M - 2) = `O(M)`이다. <br>
- 최악의 경우, i가 1씩만 증가하기 때문.
- M의 최대 크기가 백만이므로 충분히 문제를 풀 수 있음.

## 소스 코드
```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int M = Integer.parseInt(br.readLine());
        String s = br.readLine();

        int i = 0;
        int cur = 0;
        int answer = 0;
        while (i < M - 2) {
            if(s.charAt(i) == 'I' && s.charAt(i+1) == 'O' && s.charAt(i+2) == 'I') {
                cur++;
                i+=2;
            } else {
                cur = 0;
                i++;
            }

            if(cur == N) {
                answer++;
                cur--;
            }
        }
        System.out.print(answer);
    }
}
```