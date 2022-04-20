# Thread Pool

- 배경: 스레드의 생성과 소멸은 그 자체로도 시스템에 부담을 줌. 필요할 때마다 스레드를 생성하게 되면 성능의 저하로 이어짐
- **미리 제한된 수의 스레드를 생성해두고 이를 재활용하는 기술**
- 멀티 스레드 프로그래밍에서 매우 중요!
- 직접 구현하려면 간단하지 않으므로, `java.util.conccurent` 패키지 활용해서 Thread Pool 생성 가능

```java
public class ThreadPoolTest {

    public static void main(String[] args) {
        Runnable runnable = () -> {
            int n1 = 10, n2 = 20;
            System.out.println(n1 + n2);
        };
        //스레드 풀 생성
        ExecutorService exr = Executors.newSingleThreadExecutor();
        //스레드 풀에 작업 전달
        exr.submit(runnable);

        System.out.println(Thread.currentThread().getName() + " end");
        //생성된 스레드 풀과 그 안에 존재하는 스레드 소멸
        exr.shutdown();

        
        //람다식으로도 표현 가능
        exr.submit(() -> System.out.println(Thread.currentThread().getName() + ": " + (100*100)));
    }
}
```
<br>

## 다양한 스레드 풀 생성 메서드

- `newSingleThreadExecutor`: 풀 안에 하나의 스레드만 생성하고 유지
  - 장점: 하나의 코어를 기준으로 코어의 활용도를 매우 높인 풀
  - 단점: 마지막으로 전달된 작업이 가장 늦게 처리됨
- `newFixedThreadPool`: 풀 안에 인자로 전달된 수의 스레드를 생성하고 유지
- `newCachedThreadPool`: 풀 안의 여러 스레드들을 작업의 수에 맞게 유동적으로 관리
  - 작업량에 따라 스레드를 생성하고 소멸하는 것이 장점처럼 보일 수 있지만, 작업량이 많다면 단점이 됨

<br>

# Callable과 Future

- 반환형이 void인 Runnable과 달리 `Callable`은 반환형이 제네릭 자료형으로 구성되어 원하는 값으로 반환 가능
- `Future`를 통해 thread의 반환값을 저장함

```java
public class Main {
    
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        Callable<Integer> task = () -> {
            int sum = 0;
            for (int i = 1; i <= 100; i++) {
                sum += i;
            }
            return sum;
        };

        ExecutorService exr = Executors.newSingleThreadExecutor();
        Future<Integer> future = exr.submit(task);

        Integer integer = future.get();
        System.out.println(integer);
        exr.shutdown();
    }
}
```