# Optional
비어있을 수도 있고 담고있을수도있는 컨테이너 인스턴스타입


## 자바 8 이전의 null처리
OnlineClass 의 getProgress 메서드 
```java
public Progress getProgress(){
    return progress;
}
```
null 처리 예시
```java
OnlineClass spring_boot = new OnlineClass(1,"spring boot",true);
progress progress = spring_boot.getProgress();
//progress가 null일 수 있기 때문에 참조하기전에 null 체크
if(progress != null){
    progress.getStudyDuration();
}
```
문제1:이코드는 에러를 만들기 좋은 코드다.우리는 기계가 아니라 사람이기 때문에 **null을 체크하는것을 깜박할 수 있기 때문이다.**  

문제2:null을 리턴하는것 자체가 문제이다.

## 자바 8 이후의 null처리
**참고:optional은 리턴타입만으로 쓰는것이 권장사항이다.(메소드 매개변수 타입,맵의 키 타입,인스턴스 필드 타입으로 쓰지말자)**  
OnlineClass 의 getProgress 메서드 
```java
public Optional<Progress>  getProgress(){
    return Optional.ofNullable(progress);
}
```
**참고:Optional.of()메서드는 뒤에오는값이 무조건 null이 아니라고가정.null을 넣을경우 NullPointException 에러가난다. ofNullable을 쓰자**

null 처리 예시
```java
OnlineClass spring_boot = new OnlineClass(1,"spring boot",true);
Optional<progress> progress = spring_boot.getProgress();
//progress가 null일 수 있기 때문에 참조하기전에 null 체크
progress.ifPresent(p -> p.getStudyDuration());
```



## Optional 프리미티브 타입
Optional은 숫자를 넣으면 안에서 박싱 언박싱이 벌어진다.그 때문에 성능이 안좋아진다.
```java
Optional.of(10);
```
권장
```java
OptionalInt.of(10);
```

## 주의
Collection,Map,Stream Array,Optional은 Optional로 감싸지 말 것  
이유:비어있다는 것 자체를 표현할 수 있는 타입들이다.그 자체로 이미 비어있는지 안비어있는지 판단할 수 있는 컨테이너 성격의 인스턴스들은 감쌀 이유가 없다.

## Optional 메소드

* Optional 만들기
    * **Optional.of()**  
      값넣기(null불가능)
    * **Optional.ofNullable()**  
      값넣기(null가능)
    * **Optional.empty()**  
    빈값넣기
* Optional에 값이 있는지 없는지 확인하기
    * **isPresent()**
    * **isEmpty() (java11부터 제공)**
* Optional에 있는 값이 있는 경우에 그 값을 가지고 ~~을 하라.  
(꼭 Optional값 꺼낼필요없고 있으면 함수실행하고싶을때)
    * **ifPresent(Consumer)**  
    안에 값이 있으면 파라미터에 Consumer람다써서 그 함수 실행
* Optional 값이 있으면 가져오고 없는 경우에 ~~를 하라.  
(Optional값 꺼내고싶을때)
    * **orElseGet(Supplier)**  
    null일때 Supplier 람다로 실행.값이 있으면 안에 있는 값꺼냄.
    * **orElseThrow()**  
    null일때 에러던짐.값이 있으면 안에 있는 값꺼냄.
    * **get()**  
    꺼내긴 하는데 null이면 에러.(내생각에는 쓸이유가없음)
* filter()
* map()
* flatMap()