# 1655. 가운데를 말해요

    - jdk version: java 11
    - 분류: 자료구조, 힙(우선순위 큐)

<br>
<br>

최대 힙과 최소 힙을 이용한 방식은 동일하지만 장황한 코드가 나올 수 있고, 간결한 코드가 나올 수 있는 교훈을 뼈저리게 실감했다.
<br>
<br>

> Key point. <br>
> **최대 힙의 root node 값 M < 최소 힙의 root node 값 m**

## 풀이1

1. n=1일 때와 n=2 이상일 때를 분류
2. 내가 생각한 로직은 최대힙과 최소힙에 데이터가 주어진 경우에만 값을 비교할 수 있었기 때문에 분류해둠
3. 작은 값을 최대힙에, 큰 값을 최소힙에 넣음
4. 현재 넣어야 할 값이 최소힙으로부터 꺼낸 값보다 큰 경우 최소힙에 반영 (너무나도 단순했던 생각..)
5. 힙의 크기가 2 이상 차이날 경우, 크기가 더 큰 힙에서 `poll`하여 다른 힙에 `add`함
6. 힙의 크기에 따라 `peek`하는 방식을 나눔
  - if) 크기가 짝수 && 짝수 혹은 홀수 && 홀수일 경우, 각각의 힙에서 `peek`한 값 중에 최소값이 답
  - else) 크기가 더 큰 힙에서 `peek`

<br>

## 풀이2

1. 두 힙의 사이즈가 같으면 최대힙에 `add`
   - 정답은 최대힙의 `peek`한 값
2. if) 최소힙이 비어있지 않고, 최대힙의 `peek`한 값이 최소힙의 `peek`한 값보다 클 경우
   - 이유: 최대힙에 1, 6 // 최소힙에 5가 있다고 가정
     - Key point였던 최대 힙의 값이 최소 힙의 값보다 더 작아야 한다는 규칙 위배
   - 그러므로 최대힙의 값을 최소힙으로, 최소힙의 값을 최대힙으로 switch 
3. 최대힙에서 `peek`.

<br>


## 내가 작성한 코드 (풀이1)

```java
import java.io.*;
import java.util.*;

class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        //heapL: 최대 힙, heapR: 최소 힙
        PriorityQueue<Integer> heapL = new PriorityQueue<>();
        PriorityQueue<Integer> heapR = new PriorityQueue<>();

        int n = stoi(br.readLine());
        if (n == 1) {
            System.out.println(stoi(br.readLine()));
            return;
        }

        //n이 2 이상
        int a = stoi(br.readLine());
        int b = stoi(br.readLine());

        if (a < b) {
            heapL.add(a * -1);
            heapR.add(b);
        } else {
            heapL.add(b * -1);
            heapR.add(a);
        }

        bw.write(a + "\n");
        bw.write(Math.min(heapL.peek() * -1, heapR.peek()) + "\n");

        int ans=0;
        for (int i = 2; i < n; i++) {
            int e = stoi(br.readLine());

            if(!heapL.isEmpty() && !heapR.isEmpty()) {
                if (heapR.peek() < e) {
                    heapR.add(e);
                } else {
                    heapL.add(e * -1);
                }

                if(heapR.size() + 2 == heapL.size()) {
                    heapR.add(heapL.poll() * -1);
                } else if(heapL.size() + 2 == heapR.size()) {
                    heapL.add(heapR.poll() * -1);
                }

                if((heapL.size() % 2 == 0 && heapR.size() % 2 == 0) || (heapL.size() % 2 == 1 && heapR.size() % 2 == 1)) {
                    ans = Math.min(heapL.peek() * -1, heapR.peek());
                } else {
                    if(heapL.size() > heapR.size()) {
                        ans = heapL.peek() * -1;
                    } else {
                        ans = heapR.peek();
                    }
                }
            }
            bw.write(ans+"\n");
        }
        bw.close();
    }

    private static int stoi(String s) {
        return Integer.parseInt(s);
    }
}
```
<br>

## 강의 코드 (풀이2)

```java
import java.io.*;
import java.util.*;

class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        //heapMax: 최대 힙, heapMin: 최소 힙
        PriorityQueue<Integer> heapMax = new PriorityQueue<>();
        PriorityQueue<Integer> heapMin = new PriorityQueue<>();

        int n = stoi(br.readLine());

        for (int i = 0; i < n; i++) {
            int e = stoi(br.readLine());

            if ((heapMax.size() == heapMin.size())) {
                heapMax.add(e * -1);
            } else {
                heapMin.add(e);
            }

            if(!heapMin.isEmpty() && heapMax.peek() * -1> heapMin.peek()) {
                heapMax.add(heapMin.poll() * -1);
                heapMin.add(heapMax.poll() * -1);
            }
            bw.write((heapMax.peek() * -1)+ "\n");
        }
        bw.close();
    }

    private static int stoi(String s) {
        return Integer.parseInt(s);
    }
}
```