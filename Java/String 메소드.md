# String 메소드
## 반환값 boolean
>equals  

두개의 String에 값만을 비교해서 같으면 true, 다르면 false를 반환한다.(대소비교)
```java
String str1 = "java";
String str2 = "java";
boolean equals = str1.equals(str2);
System.out.println("equals: " + equals);
```
결과값:true
>startWith  

 문자열이 지정한 문자로 시작하는지 판단 같으면 true반환 아니면 false를 반환한다.(대소문자구별)
 ```java
String str = "apple";
boolean startsWith = str.startsWith("a");
System.out.println("startsWith: " + startsWith);
 ```
 결과값:true
>endWith  

문자열 마지막에 지정한 문자가 있는지를 판단후 있으면 true, 없으면 false를 반환한다.(대소문자구별)
```java
String str = "test";
boolean endsWith = str.endsWith("t");
System.out.println("endsWith: " + endsWith);
```
결과값:true
>contains  

두개의 String을 비교해서 비교대상 String을 포함하고 있으면true, 다르면 false를 반환한다.
```java
String str1 = "abcd";
String str2 = "c";
boolean contains = str1.contains(str2);
System.out.println("contains: " + contains);
```
결과값:true