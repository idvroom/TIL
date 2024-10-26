# Thread

- run: 싱글 스레드
- start: 멀티 스레드

```java
class TMain {
    public void tStart() {
        Thread t = new Thread(() -> {
            try {
                Thread.sleep(3000);
                System.out.println("tStart");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });        
        t.start(); 
        /* start 하면 멀티스레드라서 2초 지나고 rStart -> 1초 지나고 tStart 순서
         * run 하면 싱글스레드라이라 3초 대기 tStart -> 2초 대기 -> rStart
         */
    }
}

class RMain implements Runnable {
    @Override
    public void run() {
        try {
            Thread.sleep(2000);
            System.out.println("rStart");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class ThreadTestMain {   
    public static void main(String[] args) {
        TMain tMain = new TMain();
        Thread t = new Thread(new RMain());
        //RMain rMain = new RMain(); //이렇게 해서 run하는건 TMain run가 동일하다.
        tMain.tStart();
        t.start();
    }
}
```