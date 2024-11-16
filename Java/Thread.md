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
> 동기화 메소드, 동기화 블록 내에서만 사용 가능하다.

