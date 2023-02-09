# Thread
## 기본적인 쓰레드 처리방법
### sleep()
```java
Thread thread = new Thread(() ->{
  try{
    Thread.sleep(1000L);
  }catch{
    e.printStackTrace();
  }
})
```
현재 쓰레드 멈춰두기.  
다른 쓰레드가 처리할수 있도록 기회를 주지만 그렇다고 락을 놔주진 않는다.(잘못하면 데드락 걸림)  
메서드 사용시 다른쓰레드한테 우선권이감.현재쓰레드는 자기때문에.
### interrupt()
InterruptedException 에러 터짐
### join()
```java
thread.join();
```
메서드를 호출한 해당 쓰레드가 다 끝날때까지 기다림.

### 문제점
쓰레드가 수십개,수백개인 프로그램은 관리가 사실상 힘들다.

## CompletableFuture(자바8)
자바에서 비동기 프로그래밍을 가능케하는 인터페이스.