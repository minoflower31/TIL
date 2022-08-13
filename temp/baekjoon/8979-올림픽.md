# 8979. 올림픽

    - jdk version: java11
    - 알고리즘 분류: 구현, 정렬

출처: https://www.acmicpc.net/problem/8979
<br> <br>
<img width="957" alt="image" src="https://user-images.githubusercontent.com/56334513/166445919-0fbfbd64-8838-4ce8-a927-2a59f40ffb07.png">

<br>

## 접근 방법

- `Country 객체 배열`을 생성하고 k인 국가를 인스턴스에 따로 저장
- `Comparable<Country>` 인터페이스를 구현해서 금,은,동 비교
  - 맨 앞에 1등이 자리할 수 있도록 `o`를 앞에, `this`를 뒤로 해서 비교
- `Arrays.sort()`로 Comparable에 기반한 정렬 수행
- `Country 객체 배열`을 순회
  - 따로 저장한 k 국가와 코드가 맞으면 break
  - 나머지는 k 국가보다 앞에 있으므로 rank++;

<br>

### 고찰

2차원 배열로 저장하면 시간단축시킬 수 있었으나 Comparable이 먼저 떠올랐음. <br>
간단한 로직인데 의외로 시간을 많이 잡아먹음. 종이에 틀을 끄적이지 않고 바로바로 코드를 작성한 탓! <br>

**자만하지 말고 종이를 아낌없이 사용하자✊🏻**

# Source Code

```java
import java.io.*;
import java.util.*;

public class Main {

    private static int stoi(String s) {
        return Integer.parseInt(s);
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = stoi(st.nextToken());
        int k = stoi(st.nextToken());
        Country[] countries = new Country[n];
        Country find = new Country();
        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(br.readLine());
            int code = stoi(st.nextToken());
            countries[i] = new Country(code, stoi(st.nextToken()), stoi(st.nextToken()), stoi(st.nextToken()));
            if(code == k) {
                find = countries[i];
            }
        }
        Arrays.sort(countries);

        int rank = 1;
        for (Country country : countries) {
            if(country.code == find.code) {
                break;
            }
            if(country.compareTo(find) < 0) {
                rank++;
            }
        }
        System.out.println(rank);
    }

    static class Country implements Comparable<Country> {
        int code, gold, silver, bronze;

        public Country() {}

        public Country(int code, int gold, int silver, int bronze) {
            this.code = code;
            this.gold = gold;
            this.silver = silver;
            this.bronze = bronze;
        }

        @Override
        public int compareTo(Country o) {
            if (this.gold != o.gold) {
                return Integer.compare(o.gold, this.gold);
            } else if (this.silver != o.silver) {
                return Integer.compare(o.silver, this.silver);
            } else {
                return Integer.compare(o.bronze, this.bronze);
            }
        }
    }
}

```