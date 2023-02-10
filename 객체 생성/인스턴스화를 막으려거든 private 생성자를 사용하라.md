# 인스턴스화를 막으려거든 private 생성자를 사용하라
정적 메서드만 담은 유틸리티 클래스는 인스턴스로 만들어 쓰려고 설계한 클래스가 아니다.(static만 있는 클래스는 객체 생성 막자는 얘기다.)
```java
public class UtilityClass{
  //이런 static 만 있는 클래스는 객체 생성을 애초에 막자.
  public static String hello(){
    return "hello";
  }

  public static void main(String[] args){
    //static만 있는 클래스는 이것도 가능하지만
    String hello = UtilityClass = new UtilityClass();
    //이것도 가능하다.기본생성자가 기본으로 생기기때문.그래서 애초에 막아두는게 좋다.
    UtilityClass utilityClass = new UtilityClass();
    utilityClass.hello();
  }
}
```

## 방법1 abstract(안좋은 방법)
```java
public abstract class UtilityClass{
  public static String hello(){
    return "hello";
  }

  public static void main(String[] args){
    //생성시 에러남
    UtilityClass utilityClass = new UtilityClass();
    utilityClass.hello();
  }
}
```
생성시 에러가 난다.그러나 이 클래스를 상속받아서 class 만들면 생성이 가능해서 좋지않은 방법이다.
```java
public class DefaultUtilityClss extends UtilityClass{
  public static void main(String[] args){
    //생성됨
    DefaultUtilityClass utilityClass = new DefaultUtilityClass();
  }
}
```
## 방법2 private생성자 만들기(좋은 방법)
```java
public class UtilityClass{
  /**
   * 이 클래스는 인스턴스를 만들 수 없습니다.(왜 이렇게 막아놨을까 다른사람이 의아해할 수 있기때문에 주석넣어서 문서화)
   */
   //이러면 상속해서 다른클래스 만들어도 기본생성자로 생성못함
  private UtilityClass(){
    //내부에서는 생성되는데 그것도 막으려면 이 에러를 던진다.AssertinError은 exception이 아니라 에러 즉 터지면 무조건 잘못된거다 알려주는것
    throw new AssertinError();
  }
  public static String hello(){
    return "hello";
  }

  public static void main(String[] args){
    //외부에서 생성시 에러남 
    UtilityClass utilityClass = new UtilityClass();
    utilityClass.hello();
  }
}
```
