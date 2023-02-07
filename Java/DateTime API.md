# Data/Time API

## 자바 8 이전 날짜와 시간 API 문제
Date,GregorianCalender...
* 그전까지 사용하던 java.util.Date 클래스는 mutable 하기 때문에 thread safe하지 않다.
* 클래스 이름이 명확하지 않다.Date인데 시간까지 다룬다.
* 버그 발생한 여지가 많다.(타입 안정성이 없고,월이 0부터 시작한다거나..)
* 날짜 시간 처리가 복잡한 애플리케이션에서는 보통 **Joda Time**을 쓰곤했다.

## 자바8부터 생긴 새로운 날짜와 시간 API
### 기계적인 시간을 표현하는 방법
메소드 실행시간을 비교하거나 시간잴때 씀
```java
Instant now = Instant.now();
System.out.println(now);  // 기준시 UTC, GMT

ZoneId zone = ZoneId.systemDefault();
ZonedDateTime zonedDateTime = now.atZone(Zone);
System.out.println(zonedDateTime);
```
결과예시
>2020-06-21T23:53:33.110587Z  
2020-06-21T16:55:08.549881-07:00[America/Los_Angeles]

### 인류용 일시를 표현하는 방법
```java
LocalDateTime now = LocalDateTime.now();
System.out.println(now);  // 현재 시스템 로컬일시를 리턴한다. 
LocalDateTime birthDay =LocalDateTime.of(1982,Month.JULY,15,0,0,0) //만들수도있다
ZonedDateTime nowInKorea = ZonedDateTime.now(ZoneId.of("Asia/Seoul")); //다른나라의 시간 가져올때
System.out.println(nowInKorea);
```

### 기간을 표현하는 방법
**Period.between()**  
**인류용 일시** 기간을 날짜로 리턴받는법
```java
LocalDate today = LocalDate.now();
LocalDate thisYearBirthday = LocalDate.of(2020,JULY,15);

Period period = Period.between(today, thisYearBirthday);
system.out.println(period.getDay());  //결과:int ex)24

Period until = today.until(thisYearBirthday);
system.out.println(until.get(ChronoUnit.DAYS));  //결과 위와 같음 24
```
**Duration.beteen()**  
**기계적인 시간** 기간을 날짜로 리턴받는법

```java
Instant now = Instant.now();
Instant plus = now.plus(10,ChronoUnit.SECONDS);
Duration between = Duration.between(now, plus);
System.out.println(between.getSeconds());   // 결과 10
```


### 포맷팅
```java
LocalDateTime now = LocalDateTime.now();
DateTimeFormatter MMddyyyy = DateTimeFormatter.ofPattern("MM/dd/yyyy");
system.out.println(now.format(MMddyyyy)); //ex)02.07.2023
```
미리 정의된 포맷이 있기때문에 그걸쓰는것도 좋을것 같다.
