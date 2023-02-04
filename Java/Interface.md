# Interface

## 특징
인터페이스에 선언한 모든것들은 다 public으로 간주(priavate선언하면 사용가능하나 기본적으로는 public.public 생략가능. 앞에 아무것도 없으면 public으로 간주)
(!참고:클래스에서는 아무것도 안붙이면 패키지 private 레벨로 접근지시자로 간주)
### 필드
인터페이스는 private 필드를 가질 수 없다.  
그래서 인터페이스에 선언하는 필드는 public한 필드들이 된다.  
**보통 인터페이스에 정의할때는 상수만 정의한다.**

## 추가된 특징
### 메서드
### 자바 8
자바 8 이전에는 메서드를 정의하는게 불가능했다.선언은 가능했음.  

**선언**
```java 
    String hello();
```
**정의**
```java 
     String hi(){
        return "hi";
     }
```
자바 8부터 인터페이스에서는 정의 하려면 Default 키워드나 Static 키워드를 줘야한다.
>default  

인스턴스에서만 호출할 수 있는 메서드를 만들경우
```java
     default String hi(){
        return "hi";
     }    
```
>static

인스턴스를 만들지 않고도 호출할 수 있는 정적인 메서드를 정의 할경우
```java
     static String hi(){
        return "hi";
     }    
```
* **자바 8**부터는 인터페이스가 정적 메서드를 가질 수 없다는 제한이 풀림  
* **자바 8** 까지는 public 정적 멤버만 허용한다.  
### 자바 9
public 메서드 중간중간에 공통적인 작업들이 있어서 굳이 public하게 공개하고 싶지 않아서 
> static private
```java
public interface HelloService{
    String hello();

    static String hi1(){
        prepareMessage();
        return "hi";
    }
    static String hi2(){
        prepareMessage();
        return "hi";
    }
    static private void prepareMessage(){

    }    
}
```

* **자바 9** 부터는 private 정적 메서드까지 허용하지만,정적 필드와 정적 멤버 클래스는 여전히 public이어야 한다.
