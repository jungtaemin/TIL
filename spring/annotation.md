# annotation
자바 애너테이션을 정의하는 방법

## @Retention: 애노테이션의 정보를 얼마나 오래 유지할 것인가.
  * Runtime,Source,Class
 ```java
  @Retention(RetentionPolicy.RUNTIME)
  public @interface MyAnnotation{}
```
  런타임이 기본값.
  >RetentionPolicy.RUNTIME : 가장 폭이 넓다.이 애노테이션 사용한 코드를 런타임 중에도 이용할 수 있게끔  
  ```java
  @MyAnnotation
  public class MyClass{
    public static void main(String[] args){
      Arrays.stream(MyClass.class.getAnnotations()).forEach(System.out::println);  
    }
  }
  
  ```
  출력됨
  >RetentionPolicy.CLASS : 런타임에는 참조할 수없게.클래스까지만 유지하겠다.
   ```java
  @MyAnnotation
  public class MyClass{
    public static void main(String[] args){
      Arrays.stream(MyClass.class.getAnnotations()).forEach(System.out::println);  
    }
  }
  
  ```
  출력안됨
  >RetentionPolicy.SOURCE  

  클래스파일에서도 나는 이정보가 필요없다.소스 코드이해를돕기위한 주석같은거다.바이트코드에도 참조할수없음.
## @Target: 애노테이션을 사용할 수 있는 위치. 
  Type,Field,Method,Parameter,...
  > ElementType.TYPE : 가장 많이 쓰는 타입(클래스타입,인터페이스타입..)
  ```java
  @Retention(RetentionPolicy.RUNTIME)
  @Target(ElementType.TYPE)
  public @interface MyAnnotation{}
  ```

   ```java
  @MyAnnotation
  public class MyClass{
      
      public static void main(String[] args){
        Arrays.stream(MyClass.class.getAnnotations()).forEach(System.out::println);  
      }
  }
  
  ```
> ElementType.METHOD : 

  ```java
  @Retention(RetentionPolicy.RUNTIME)
  @Target({ElementType.TYPEm, ElementType.METHOD})
  public @interface MyAnnotation{}
  ```

  ```java
  @MyAnnotation
  public class MyClass{
      @MyAnnotation
      public static void main(String[] args){
        Arrays.stream(MyClass.class.getAnnotations()).forEach(System.out::println);  
      }
  }
  
  ```  
# annotation 종류
* **controller 에서 많이쓰는 annotation**
  * [@ModelAttribute/@RequestBody](https://github.com/jungtaemin/TIL/blob/main/spring/annotation.md#@ModelAttribute/@RequestBody)
  * @RequestParam
* [@Documented](https://github.com/jungtaemin/TIL/blob/main/spring/annotation.md#@Documented)
* @Override
## @ModelAttribute/@RequestBody
>공통점  

두 애노테이션 다 클라이언트 측에서 보낸 데이터를 Java 코드에서 사용할 수 있는 오브젝트로 만들어주는 애노테이션이다.
>차이점  

**@ModelAttribute**  

@ModelAttribute 애너테이션의 역할은 클라이언트가 보내는 HTTP 파라미터들을 특정 Java Object에 바인딩(맵핑) 하는 것입니다  
객체의 필드에 접근해 데이터를 바인딩할 수 있는 생성자 혹은 setter 메서드가 필요하다.  
Query String 및 Form 형식이 아닌 데이터는 처리할 수 없다.
**@RequestBody**

클라이언트가 보내는 HTTP 요청 본문(JSON 및 XML 등)을 Java 오브젝트로 변환하는 것입니다.  
HTTP 요청 본문 데이터는 Spring에서 제공하는 HttpMessageConverter를 통해 타입에 맞는 객체로 변환됩니다.    
@RequestBody를 사용할 객체는 필드를 바인딩할 생성자나 setter 메서드가 필요없다.
## @RequestParam
@RequestParam은 1개의 HTTP 요청 파라미터를 받기 위해서 사용한다
## @Documented  
```java
@Documented
public @interface MyAnnotation{}
```
이 애노테이션이 사용한 자바Doc을 만들때 이 애노테이션 정보가 자바Doc에 포함된다.
>사용하는 애너테이션 종류
* @Override

## @Override
컴파일타임에 실제로 이메서드가  인터페이스에 구현하는 메서드가 맞는지 시그니처를 확인해준다.
```java
@Override
public int getPrice(){

}
```