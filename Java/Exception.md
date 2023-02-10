# Exception
 ## Exception Class 계층도
 
**Throwable** 
* Exceptions
  * [Check Exceptions](https://github.com/jungtaemin/TIL/blob/main/Java/Exception.md#Checked-Exception)
    * IOException
    * SQLExcetion
    * ClassNotFoundException
  * [Uncheck Exceptions](https://github.com/jungtaemin/TIL/blob/main/Java/Exception.md#Unchecked-Exception)
    * [NullPointerException](https://github.com/jungtaemin/TIL/blob/main/Java/Exception.md#NullPointerException)
    * IllegalArgumentException
    * ArithmenticException
    * IndexOutOfBoundsException

* Errors 
  * StackOverFlowError
  * VirtualMachineError
  * OutOfMemoryError

</br>

**참고**
[예외 처리](https://github.com/jungtaemin/TIL/blob/main/Java/Exception.md#예외-처리),
[컴파일](https://github.com/jungtaemin/TIL/blob/main/Java/Exception.md#컴파일),
[런타임](https://github.com/jungtaemin/TIL/blob/main/Java/Exception.md#런타임)


# Checked Exception
**확인 시점 : 컴파일단계**

복구가 가능한 상황일때 던짐.
</br>  
반드시 예외를 처리해야함  
Exception 처리코드를 compiler가 check
# Unchecked Exception
**확인시점 : 실행단계**  

복구가 불가능할때 던짐.
</br>
명시적인 처리를 강제하지 않음  
컴파일하는데는 문제없다.실행하면 문제가 발생함

# NullPointerException
객체 참조가 없는 상태,즉 null 값을 가지고 있는 참조 변수로 객체 접근 연산자인 도트(.)를 사용했을때 발생(객체를 사용하려 하였으나 객체가 없는 상태이기 때문에 발생하는 에러)
```java
public class NullPointerExceptionExample {
	public static void main(String[] args) {
    	String data = null;
        System.out.println(data.toString());
    }
 }
```
**에러 터질 시**
![화면 캡처 2023-02-02 135022](https://user-images.githubusercontent.com/96284736/216243360-c82e7e2a-4f0f-4bba-b2fd-9f7664503055.png)

예외 던질때 사용법:
``` java
public void updateDeliveryDate(LocalDate deliveryDate){
  if(deliveryDate == null){
    throw new NullPointerException("deliveryDate can't be null");
  }
  
}
```


# IllegalArgumentException

예외 던질때 사용법:예외를 던질때는 최소한 어떤 필드가 잘못되었는지 알려주는게 좋다.
``` java
public void updateDeliveryDate(LocalDate deliveryDate){
  if(deliveryDate.isBefore(LocalDate.now())){
    throw new IllegalArgumentException("deliveryDate can't be earlier than" + LocalDate.now());
  }
  
}
```
deliveryDate 잘못되었다고 알려줌.IllegalArgumentException 을 던질때 반드시 생성자에 충분한 정보를 전달해주면 좋다.
# 예외 처리
>try catch finally

**직접처리**  
다음 메소드에 던져줄 때,명시해줘야한다.  
```java
try{
///예외가 발생할 가능성이 있는 코드
.... ......... ;
.............. .. ; << 1. 여기서 예외가 발생했을 때
.............. .... ; << 2. 예외 발생한 곳 아래는 실행하지 않고,
}
catch(예외클래스 e) {
   예외 처리; << 3. catch문에서 예외처리를 한다.
}
finally {
///무슨 일이 있든 항상 실행
}
```
---
</br>

>throws  

**간접처리**  
이걸 명시해주지 않으면,예외가 있는지 없는지 모르니까 명시해 준다.
```java
void method1() throws Exception1, Exception2, ... ExceptionN {
    // 메서드의 내용
}
```

## 예외 발생시키기
>throw  

예외를 발생시킬 때는 new를 통해 예외를 만들고 throw로 만든 예외를 발생시킨다.
```java
Exception e = new Exception("고의적인 예외")  // 예외 객체 생성
throw e;  // 고의로 만든 예외 발생
```

## 컴파일
개발자가 작성한 소스코드를 컴파일하여 기계어로 변환하는 과정(번역하는과정)
## 런타임
컴파일을 마친 프로그램이 사용자에 의해 **실행**되어 동작되어지는 때.
