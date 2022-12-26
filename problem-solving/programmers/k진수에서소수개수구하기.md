# k진수에서 소수 개수 구하기

    언어: Java
    분류: 구현

<br>

### 다른 문제 풀이 방법

k진수로 변환한 후에 split("0") 함수로 분할시킬 수도 있다.

<br>

### 소스 코드
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int solution(int n, int k) {
        String number = Integer.toString(n, k);
        int answer = 0;

        List<Long> candidates = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < number.length(); i++) {
            char ch = number.charAt(i);
            if(ch > '0') {
                sb.append(ch);
            } else {
                if(sb.length() == 0) continue;
                candidates.add(Long.parseLong(sb.toString()));
                sb = new StringBuilder();
            }
        }

        if(sb.length() > 0) candidates.add(Long.parseLong(sb.toString()));

        for (Long candidate : candidates) {
            if(isPrime(candidate)) answer++;
        }

        return answer;
    }

    private boolean isPrime(long num) {
        if(num <= 1) return false;

        for (int i = 2; (long) i * i <= num; i++) {
            if(num % i == 0) return false;
        }
        return true;
    }
}
```