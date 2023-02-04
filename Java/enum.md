# enum
**Enum**eration  
## 특징
* 열거타입.자바5부터 지원하는 기능.
* 상수 목록을 담을 수 있는 데이터 타입.
* 특정한 변수가 가질 수 있는 값을 제한할 수 있다.**타입-세이프티**(Type-Safety)를 보장할 수있다.
* **싱글톤 패턴**을 구현할 때 사용하기도 한다.  

만약 상태변화에는 일정한 상태변화가 있으니까 정해진 상태변화가있다.
가령 주문의 상태변화면..
```java
public enum OrderStatus{
    PREPARING,SHIPPED,DELIBERING,DELIVERED
}
```
이렇게 enum은 어떠한 정해져있는 상수값들은 정할 수 있다.

## 장점

특정한 값을 제한할 수 있다.필드가 가질 수 있는 값을 특정한 값들로만 제한할 수 있다.(**타입 세이프티**)

```java
public class Order{
    private OrderStatus orderStatus;
}
```
enum이 없다면 상태를 표현할떄 int나 short 쓰거나 char String을 쓸거다.
이것들의 문제는 주석으로
```java
public class Order{
    // 0 - 주문 받음
    // 1 - 준비중
    // 2 - 배송중
    // 3 - 배송완료
   private int status;
   private final int PREPARING = 0;
   private final int SHIPPED = 2;

   public static Order primeOrder(Product product){
    if(tihs.status == 0){
        return Order;
    }
   }
   
}
```
이런식으로 값을 적어놓고 0,1,2,3 체크하거나 상수로 정의하는데 만약 status라는 값에 100이 들어가거나 200이 들어가면?? 이런경우가 실제로 있을 수 있다.  
**왜??** int에는 들어갈 수 있는 값이니까.
그래서 어떻게든 타입세이프를 보장해주는 코드가 (방어하는코드가) 어딘가에는 들어가있어야한다.

**!!**  
그런데 그런것들에 대한 필요가 enum을 쓰면 없어진다.  
왜나면 이건 컴파일러 차원에서 지원을 하니까.
enum에는 전혀 다른값이 들어갈 수 없다.