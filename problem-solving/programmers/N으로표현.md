# N으로 표현

    언어: Java
    분류: DP

<br>

### 왜 Set을 이용해야 하는가?
dp의 index가 의미하는 것은 N이 사용된 개수를 뜻함. 그래서 사용된 개수에 따라 도출된 답을 쉽게 찾을 수 있음. <br>

또한 index가 1부터 8까지 순차적으로 탐색하므로 최적의 답을 찾는 과정에 최소를 찾을 수 있음

<br>

### 소스 코드
```java
import java.util.*;

class Solution {

    public int solution(int N, int number) {
        if(N == number) return 1;

        List<Set<Integer>> dp = new ArrayList<>();
        for (int i = 0; i < 9; i++) {
            dp.add(new HashSet<>());
        }

        int seqN = 0;
        for (int i = 1; i < 9; i++) {
            seqN = seqN * 10 + N;
            dp.get(i).add(seqN);

            for (int j = 1; j < i; j++) {
                for (int operand1 : dp.get(j)) {
                    for(int operand2 : dp.get(i-j)) {
                        dp.get(i).add(operand1 + operand2);
                        dp.get(i).add(operand1 - operand2);
                        dp.get(i).add(operand1 * operand2);

                        if(operand2 != 0)
                            dp.get(i).add(operand1 / operand2);

                        if(dp.get(i).contains(number)) return i;
                    }
                }
            }
        }
        return -1;
    }
}
```