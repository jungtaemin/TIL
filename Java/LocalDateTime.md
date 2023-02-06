# LocalDateTime
자바8부터 생긴 새로운 날짜와 시간 API

## 자바 8 이전 날짜와 시간 API 문제
Date,GregorianCalender...
* 그전까지 사용하던 java.util.Date 클래스는 mutable 하기 때문에 thread safe하지 않다.
* 클래스 이름이 명확하지 않다.Date인데 시간까지 다룬다.
* 버그 발생한 여지가 많다.(타입 안정성이 없고,월이 0부터 시작한다거나..)
* 날짜 시간 처리가 복잡한 애플리케이션에서는 보통 **Joda Time**을 쓰곤했다.