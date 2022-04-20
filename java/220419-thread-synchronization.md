# Thread

#### 프로그램이 OS로부터 메모리를 할당받아 프로세스가 되는데, 하나의 프로세스는 하나 이상의 thread를 가지게 됨
#### thread란 실행 중인 프로그램 내에 또 다른 실행의 흐름을 형성하는 주체

<br>

## Multi-Thread

- 여러 thread가 동시에 수행되는 프로그래밍, 동시성 또는 병렬성에 의해 수행
- **동시성**: 멀티작업을 위해 **하나의 코어**에서 **멀티 스레드가 번갈아가면서** 실행하는 성질
- **병렬성**: 멀티작업을 위해 **멀티 코어**에서 **개별 스레드를 동시에** 실행하는 성질

<br>

- 각 thread 사이에 공유하는 자원이 존재할 수 있음 (자바는 static)
- 여러 thread가 공유 자원에 대해 동시에 접근할 때, 서로 자원을 차지하려는 `race condition` 발생
- 동시에 접근해서는 안되는 공유 자원을 접근하는 코드의 일부를 `critical section` 이라 함
- critical section에 대해 `세마포어` 같은 동기화 메커니즘으로 접근해야 함. (차례로 접근)

<br>

## Thread Status

<img width="520" alt="image" src="https://user-images.githubusercontent.com/56334513/163926389-21ac0369-4e28-438e-901f-1f50bb025f12.png"><br>
출처: https://www.geeksforgeeks.org/lifecycle-and-states-of-a-thread-in-java/
<br>

- **new**: 새 스레드가 생성된 상태. 아직 실행되지 않은 상태
- **runnable**: 실제로 실행 중이거나 언제든지 실행할 준비가 되어 있는 상태
  - 실행할 시간을 주는 것은 스레드 스케줄러의 책임
  - 다중 스레드 프로그램은 각 개별 스레드에 고정된 시간을 할당하는데, 각각의 모든 스레드는 잠시 동안 실행된 다음, 다른 스레드가 실행할 수 있도록 CPU를 다른 스레드에 양도함
- **blocked, waiting**
- **time waiting**: time-out 매개변수가 있는 메서드를 호출한 상태
  - `sleep()`이나 `wait()` 메서드를 호출할 때 이 상태로 이동
- **terminated**
  1. 정상적으로 종료
  2. 비정상적인 오류 이벤트 발생(interrupt)
     <br>

**Runnable로 돌아오는 조건**
> sleep() -> time-out 시 <br>
> wait() -> notify() <br>
> join() -> other thread exits <br>

<br>

## Thread 만드는 2가지 방법

### 1. Thread 클래스 상속

```java
class MyThread extends Thread {
    
    @Override
    public void run() {
        //...
    }
}
public class Main {
    
    public static void main(String[] args) {
        MyThread thread1 = new MyThread();
        thread1.start();

        MyThread thread2 = new MyThread();
        thread2.start();
    }
}
```
- `Thread.currentThread()`: 현재 스레드 상태 확인

<br>

### 2. Runnable 인터페이스 구현

- 자바는 다중 상속이 허용되지 않으므로, 이미 상속을 받은 경우 implements로 `Runnable` 인터페이스를 구현
- Runnable 인터페이스는
  1. 익명 함수로 변수에 할당 가능
  2. `@FunctionalInterface` 어노테이션이 붙어있으므로 람다표현식 가능

```java
class MyThread extends Object implements Runnable {

    @Override
    public void run() {
        //...
    }
}
public class Main {

    public static void main(String[] args) {
        //1. Runnable을 구현한 인스턴스로 Thread 생성
        MyThread runnable = new MyThread();
        
        //2. 익명 함수로 runnable 생성
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                //...
            }
        };
        
        //3. 람다 표현식으로 생성
        Runnable runnable = () -> System.out.println("hi");

        Thread thread = new Thread(runnable);
        thread.start();
    }
}
```
<br>

## Priority

- Thread.MIN_PRIORITY(=1) ~ Thread.MAX_PRIORITY(=10)
- default: Thread.NORM_PRIORITY(=5)
- 우선 순위가 높을수록 먼저 CPU를 할당받음
- `setPriority()`로 우선 순위 등록
- (보충하기)

<br>

## join()

- 동시에 두 개 이상의 thread가 실행될 때 특정 thread가 우선적으로 실행되어야 할 때
- **Waits for this thread to die.** (java api)
  - e.g. main thread에서 다른 스레드의 join() 호출 시에 다른 스레드가 종료할 때까지 기다림(=non-runnable) 
```java
public class JoinTest extends Thread {

    public static void main(String[] args) {
        System.out.println(Thread.currentThread() + " start");
        JoinTest test1 = new JoinTest(1, 50);
        JoinTest test2 = new JoinTest(51, 100);

        test1.start();
        test2.start();
        try {
            test1.join();
            test2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        int res = test1.total + test2.total;
        System.out.println("test1.total = " + test1.total);
        System.out.println("test2.total = " + test2.total);
        System.out.println(res);
    }
}
```
<br>

## Thread 종료하는 법

- `run()` 메서드에서 무한 반복으로 실행할 경우, `flag` boolean 변수를 이용해 종료 알림

<br>

## 멀티 Thread 프로그래밍에서의 동기화

### critical section과 semaphore

- critical section은 두 개 이상의 thread가 공유 자원에 접근하는 코드의 일부이고 이를 해결하기 위해 semaphore 사용.
- semaphore는 시스템 객체이며 get/release 두 개의 기능 존재
- 오직 하나의 thread만이 semaphore를 얻고, 나머지 thread는 `blocked` 상태가 된다.
- semaphore를 얻은 thread만 critical section에 접근할 수 있다.

<img width="647" alt="image" src="https://user-images.githubusercontent.com/56334513/163956789-7e35e3c9-5ba4-4b07-baca-bf40aadc6da8.png">

<br>

## 동기화

- critical section에 접근한 경우 공유 자원을 `lock`하여 다른 thread의 접근을 제어함
- 하지만 잘못 구현하면 **deadlock** 발생


### 자바는 `synchronized block` 또는 `synchronized method` 사용

- 한순간에 하나의 쓰레드의 접근만을 허용하겠다는 의미
- `synchronized method`는 method를 선언할 때 붙임
  - `synchronized void methodName()`
  - 불필요한 부분까지도 동기화된다는 단점 존재

- `synchronized block`은 method 안에 선언
  - `synchronized(Object) {...}`
  

**주의사항**
- 자바는 **deadlock을** 방지하는 기술이 제공되지 않으므로 synchronized 메서드 안에 synchronized 메서드를 호출하지 않도록 하자!

<br>

## wait() / notify() 메서드 사용

- 접근하려는 리소스가 유효한 상태가 아닌 경우 Thread는 **waiting** 상태가 됨 `wait()`
- 유효한 자원이 나타날 때까지 기다렸다가 생기면 `notify()`를 통해 **waiting** 상태인 여러 thread 중에 무작위로 **runnable** 상태로 돌아오게 함
- `notifyAll()`은 waiting 상태의 모든 thread를 runnable 상태로 **(java에서 권장하는 스펙)**
- 자원에 접근하지 못한 thread들은 다시 waiting 상태로 돌려보냄

e.g. 한정된 소켓을 사용할 경우