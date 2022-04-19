# 17829. 영단어 암기는 괴로워


    - jdk version: java11 
    - 알고리즘 분류: 자료구조, 트리, 해시

<br>
<br>

# 문제 해결

1. `HashMap`을 이용하여 Key-Value로 String과 Integer를 설정한다.
   - 이유?
     - 자주 나오는 단어일수록 앞에 배치해야 되기 때문. 만약 이 조건이 없었다면 `TreeSet`으로 해결 가능했을 것

<br>

2. `TreeSet`으로 정렬하되, `Word` 클래스에서 `Comparable 인터페이스`를 구현

3. `HashMap`의 key-value를 `TreeSet(new Word(key,value))` 형태로 저장
4. BST를 기반으로 한 트리 구조이므로 `compareTo`를 오버라이딩함에 따라 **정렬**됨

<br>
<br>

# 소스 코드

```java
import java.io.*;
import java.util.*;

class Main {

    static class Word implements Comparable<Word>{
        private final String str;
        private final Integer count;

        public Word(String str, Integer count) {
            this.str = str;
            this.count = count;
        }

        @Override
        public int compareTo(Word o) {
            if(!this.count.equals(o.count)) return o.count.compareTo(this.count);
            else if(this.str.length() != o.str.length()) return Integer.compare(o.str.length(), this.str.length());
            else return this.str.compareTo(o.str);
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String[] s = br.readLine().split(" ");
        int n = Integer.parseInt(s[0]);
        int m = Integer.parseInt(s[1]);

        Map<String, Integer> hashMap = new HashMap<>();
        Set<Word> treeSet = new TreeSet<>();

        for (int i = 0; i < n; i++) {
            String str = br.readLine();
            if(str.length() < m) continue;

            hashMap.merge(str, 1, Integer::sum);
        }

        for (String s1 : hashMap.keySet()) {
            treeSet.add(new Word(s1, hashMap.get(s1)));
        }

        for (Word word : treeSet) {
            bw.write(word.str +"\n");
        }
        bw.flush();
    }
}
```
