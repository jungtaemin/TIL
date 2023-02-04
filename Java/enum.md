# enum
**출처 [백기선-이펙티브 자바 완벽 공략 1부](https://www.inflearn.com/course/%EC%9D%B4%ED%8E%99%ED%8B%B0%EB%B8%8C-%EC%9E%90%EB%B0%94-1) 강의를 보고 정리한 포스팅입니다.**  

**Enum**eration
## 특징
* 열거타입.자바5부터 지원하는 기능.
* 상수 목록을 담을 수 있는 데이터 타입.  
(**상수란 변하지 않고, 항상 일정한 값을 갖는 수를 말한다.**)
* 특정한 변수가 가질 수 있는 값을 제한할 수 있다.**타입-세이프티**(Type-Safety)를 보장할 수있다.  
(**타입세이프티 - [장점참고](https://github.com/jungtaemin/TIL/blob/main/Java/enum.md#타입-세이프티)**)
* **싱글톤 패턴**을 구현할 때 사용하기도 한다. 
* enum을 비교할떄는 ==으로(자바 jvm레벨에서 하나의 인스턴스만 있음을 보장) 
**equals 보다 좋은 이유:equals 는 NullPointException 이 나지만 ==은 NullPointException 발생안함**   
(사용예시: if(Order.orderStatus == OrderStatus.DELIVERED))

만약 상태변화에는 일정한 상태변화가 있으니까 정해진 상태변화가있다.
가령 주문의 상태변화면..
```java
public enum OrderStatus{
    PREPARING,SHIPPED,DELIBERING,DELIVERED
}
```
이렇게 enum은 어떠한 정해져있는 상수값들은 정할 수 있다.

## 장점
### 타입 세이프티

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


## 메서드
> values()  

모든 enum 타입을 담고있다.들어있는 값을 다 가져오는 메서드.
```java
public static void main(String[] args) {
    Arrays.stream(OrderStatus.values()).forEach(System.out::println);
}
```
**결과**
>PREPARING  
SHIPPED  
DELIBERING  
DELIVERED  
---

## 구조
### 기본
```java
public enum OrderStatus{
    PREPARING,SHIPPED,DELIBERING,DELIVERED
}
```
### 상태값
```java
public enum OrderStatus{
    PREPARING(0),SHIPPED(1),DELIBERING(2),DELIVERED(3)

    private int number;

    OrderStatus(int number){
        this.number = number;
    }
}
```
가령 DB에 저장할때 문자열이 아니라 숫자로 저장해야된다고 할때 사용.  
문자보다 숫자가 더 편하다.이름들이 바뀌더라도 숫자를 그대로 유지하게끔.  
DB에다 매핑된 값들을 따로 정의를 하고싶다.

### 메서드

enum은 메서드도 가질 수 있다.