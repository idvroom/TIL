
#### 참고 서적
- [이것이 자바다 3판 (신용권, 임경균. 이것이 자바다. 한빛미디어, 2024.)](https://product.kyobobook.co.kr/detail/S000212853100) <br>
내용과 소스는 내가 보기 편하게 작성
--- 
# Thread

### 생성
- 익명 방식을 많이 사용
- main(String[] args) 도 1개의 Thread이다.

```java
public class ThreadTestMain {   
    public static void main(String[] args) {
        //main Thread

        //익명
        Thread t = new Thread(new Runnable() {            
            @Override
            public void run() {
            }
        });
        
        //익명
        Thread t2 = new Thread() {            
            @Override
            public void run() {
            }
        };
    }
        
    t.start();
    t2.start();
    
    ExtendsThread ex = new ExtendsThread();
    ex.start();
    
    Thread rt = new Thread(new RunnableThread());
    rt.start();
}

class extendsThread extends Thread {
    @Override
    public void run() {        
    }
}

class RunnableThread implements Runnable {
    @Override
    public void run() {        
    }    
}
```

### 상태
- 생성 -> start() -> 실행 대기(runnable) -> 실행 -> 종료(terminated)
- 실행 -> 일시 정지 -> 실행 대기로도 갈 수 있다.
- 일시정지로 보내는: sleep, join, wait
- 일시정지에서 벗어나는: interrupt, notify, notifyAll
- 실행 대기로: yield

1. join()
- 일시 정지로 보내는 메소드
- 다른 쓰레드가 종료 될 떄까지 대기
```java
public static void main(String[] args) {
    Thread t1 = new Thread(new Runnable() {            
        @Override
        public void run() {
            String name = Thread.currentThread().getName();
            for (int i = 0; i < 5; i++) {
                System.out.println(name + ": " + i);
            }
        }
    });
    
    Thread t2 = new Thread() {            
        @Override
        public void run() {
            try {
                t1.join(); // <- t1이 끝날 때까지 대기하겠다.
            } catch (InterruptedException e) {
            }
            
            String name = Thread.currentThread().getName();
            for (int i = 0; i < 5; i++) {
                System.out.println(name + ": " + i);
            }
        }
    };
    
    t1.start();
    t2.start();
}

join 사용 결과
Thread-0: 0
Thread-0: 1
Thread-0: 2
Thread-0: 3
Thread-0: 4
Thread-1: 0
Thread-1: 1
Thread-1: 2
Thread-1: 3
Thread-1: 4

join 미사용 결과
Thread-0: 0
Thread-1: 0
Thread-1: 1
Thread-1: 2
Thread-0: 1
Thread-0: 2
Thread-0: 3
Thread-1: 3
Thread-1: 4
Thread-0: 4
```

2. yield()
- 실행대기로 보내는 메소드
- 다른 쓰레드에게 실행 양보
- t1 Thread가 일정 시간 뒤에 false가 되고 yield() 메소드를 호출하여 t2에게 많은 실행기회를 준다. 
- console을 잘 보고 확인해야한다.
```java
class MainThread extends Thread {
    public boolean mainBool = true;
    @Override
    public void run() {
        String name = Thread.currentThread().getName();
        
        while (true) {
            if (mainBool) {
                System.out.println(name + ": 작업 중");
            } else {
                //System.out.println(name + ": yield");
                Thread.yield();
            }
        }
        
    }
}

public class ThreadTest {    
    public static void main(String[] args) {
        MainThread t1 = new MainThread();
        MainThread t2 = new MainThread();

        t1.start();        
        t2.start();
        
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
        }
        t1.mainBool = false;
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        t1.mainBool = true;
    }
}
```

3. 스레드 동기화
- synchronized 사용
  - t1에서 값을 변경 -> t2에서도 변경 -> 1초 대기 후 값 찍기
  - t2의 값이 마지막으로 저장되어 t1에서도 t2의 값이 표시
  - synchronized를 사용하여 t1이 종료될 때까지 잠금하여 t2가 접근하지 못 하게 한다.
```java
class MainThread extends Thread {
    private Calc calc;
    private int add;
    
    public MainThread(Calc calc, int v) {
        this.calc = calc;
        this.add = v;
    }
    
    @Override
    public void run() {
        calc.setSum(add);
    }
}

class Calc {
    private int sum = 0;
    
    //메소드 영역 동기화
    public synchronized void setSum(int v) {
        //일부 영역만 동기화
//        synchronized (this) {
//           code.. 
//        }
        this.sum = v;
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        System.out.println(Thread.currentThread().getName() + ": " + this.sum);
    }
    
    public int getSum(int v) {
        return this.sum;
    }
}

public class ThreadTest {    
    public static void main(String[] args) {
        Calc calc = new Calc();
        MainThread t1 = new MainThread(calc, 10);
        MainThread t2 = new MainThread(calc, 100);
        t1.start();
        t2.start();
    }
}
```
- wait(), notify() 사용
  - t1 작업 완료 -> notify() 호출하여 일시 정지 되어 있는 다른 Thread를 실행 대기 상태로 -> t1은 wait()을 호출하여 일시정지로 상태로
  - 동기화 메소드, 동기화 블록 내에서만 사용 가능하다.

추가 확인!!
왜 synchronized를 해야하는가??
다른 스레드를 실행 대기로는 랜덤인가??

```java
//Thread가 번갈아가면서 실행하는 코드
class Work {
    public synchronized void methodA() {
        System.out.println(Thread.currentThread().getName() + ": 실행");
        notify(); //다른 스레드를 실행 대기로
        try {
            wait(); //자신은 일시 정지로
        } catch (InterruptedException e) {
            
        }
    }

    public synchronized void methodB() {
        System.out.println(Thread.currentThread().getName() + ": 실행");
        notify(); //다른 스레드를 실행 대기로
        try {
            wait(); //자신은 일시 정지로
        } catch (InterruptedException e) {
            
        }
    }
}
class MainThread1 extends Thread {
    private Work work;
    
    public MainThread1(Work work) {
        this.work = work;
    }
    
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            work.methodA();
        }
    }
}

class MainThread2 extends Thread {
    private Work work;
    
    public MainThread2(Work work) {
        this.work = work;
    }
    
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            work.methodB();
        }
    }
}

public class ThreadTest {    
    public static void main(String[] args) {
        Work work = new Work();
        MainThread1 main1 = new MainThread1(work);
        MainThread2 main2 = new MainThread2(work);
        
        main1.start();
        main2.start();
    }
}

Thread-0: 실행
Thread-1: 실행
Thread-0: 실행
Thread-1: 실행
....
```

4. Thread 종료
- flag 사용
```java
class StopThread extends Thread {
    private boolean stop;
    
    @Override
    public void run() {
        while (!stop) {
            System.out.println("동작 중");
        }
        System.out.println("정리 중지 됨");
    }
    
    public void setStop(boolean b) {
        this.stop = b;
    }
}

public class ThreadTest {    
    public static void main(String[] args) {
        StopThread t = new StopThread();
        t.start();
        
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        
        t.setStop(true);
    }
}
```
- interrupt()
  - 실행 대기/실행 상태일 때는 interrupt()가 호출 되어도 InterruptedException 미발생
  - 어떤 이유로 일시 정지 상태가 되면 InterruptedException 발생
  <br>
  추가 확인!! 뭐가 좋은거지.//
```java
//sleep() 사용 (일시 정지로 만든다)
class StopThread extends Thread {    
    @Override
    public void run() {
        try {
            while (true) {
                System.out.println("동작 중");
                Thread.sleep(1); //일시 정지로 만들 수 있게 호출해야함
            }                
        } catch (InterruptedException e) {
        }
        
        System.out.println("정리 중지 됨");
    }
}

public class ThreadTest {    
    public static void main(String[] args) {
        StopThread t = new StopThread();
        t.start();
        
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
        }
        
        t.interrupt();
    }
}
```

``` java
//일시 정지를 만들지 않고 사용하는 법
class StopThread extends Thread {        
    @Override
    public void run() {
        while (true) {
            System.out.println("동작 중");

            ////interrupt 가 호출 되었는지 확인
            //정적 메소드
            if (Thread.interrupted()) {
                break;
            }
            //인스턴스 메소드
            if (Thread.currentThread().isInterrupted()) {
                break;
            }
            ////
        }     
        
        System.out.println("정리 중지 됨");
    }
}

public class ThreadTest {    
    public static void main(String[] args) {
        StopThread t = new StopThread();
        t.start();
        
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
        }
        
        t.interrupt();
    }
}
```

5. daemon 쓰레드
- 자동 저장 되다가 main이 종료되면 DaemonThreadTest도 같이 종료 된다.

확인
thread2개를 실행해서.. 자식thread로?

```java
class DaemonThreadTest extends Thread {    
    public void save() {
        System.out.println(Thread.currentThread().getName() + ": 자동 저장!!");
    }
    
    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                break;
            }
            save();
        }
    }
}

public class ThreadTest {    
    public static void main(String[] args) {
        DaemonThreadTest t = new DaemonThreadTest();
        t.setDaemon(true); // 빼면 main이 종료되어도 DaemonThreadTest는 계속 실행
        t.start();
        
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
        }
        
        System.out.println(Thread.currentThread().getName() + ": main 종료");
    }
}
```

6. 스레드풀

생성, 종료
```java
//=========생성
//코어 스레드: 스레드가 증가되고 사용하지 않는 스레드에서 제거할 때 유지하는 최소 쓰레스
ExecutorService executorService1 = Executors.newCachedThreadPool();
//초기 스레드, 코어 0, 작업 개수가 많아지면 새 스레드 생성, 60초 동안 작업을 하지 않으면 풀에서 제거, 최대 int max

ExecutorService executorService2 = Executors.newFixedThreadPool(5);
//초기 스레드 0, 작업 개수가 많아지면 최대 5개까지, 생성된 스레드를 제거하지 않는다.

ExecutorService pool = new ThreadPoolExecutor(
        5,  //코어 스레드수
        10, //최대 스레드수
        100L, //놀고 있는 시간
        TimeUnit.SECONDS, //시간 단위
        new SynchronousQueue<Runnable>() //작업큐
);


//========= 종료
//남아있는 작업을 마무리하고 종료(작업 큐도 포함)
executorService1.shutdown();
//현재 작업 중지 + 스레드풀 종료, 리턴값은 작업큐에 미처리된 작업
List<Runnable> list = executorService1.shutdownNow();
```

실제 작업 예시
