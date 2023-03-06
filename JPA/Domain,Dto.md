# Domain,Dto
> 분류  
* 프론트에서 등록기능-Controller에 데이터 담는 FormDto로 받음
* @Entity로 만든 클래스 도메인
* 리스트 뿌릴때 데이터 찾음 - 프론트로 보낼때 Dto로 담아서 보냄


## Entity
Entity를 직접 노출하면 문제가 화면을 처리하기위한 기능을 추가하게 되고 화면 종속적인 기능이 생기게 된다.  
그러면 화면기능때문에 엔티티가 지저분해지고 유지보수하기 어려워진다.(엔티티를 바꾸면 API스펙도 다바뀌고 난리가난다.)  
JPA를 쓸때 Entity를 **최대한 순수하게 유지해야한다**.핵심 비즈니스 로직에만 dependency가 있도록 설계하는게 중요하다.    그렇게해야 애플리케이션이 커져도 유지보수성이 높아진다.  
> 결론:실무에서는 엔티티는 핵심비즈니스로직만 가지고있고 화면을 위한 로직은 없어야한다.

>등록,수정시(수정시에는 entity생성안하고 그냥 파라미터로 값 넘기기): controller에서 dto로 받아서 new 해서 entity생성후 service로 보냄  

>수정폼시:controller에서 service.find 해서 entity꺼내서 formDto에 다 데이터 옮겨서 프론트에 보냄

---
양방향 연관관계까지 걸리면 엔티티 직접 노출시 한곳을 @JsonIgnore처리 하지않으면 양쪽을 서로 호출하면서 무한 루프가 걸린다.  
**연관관계가 있는 list데이터 dto로 바꾸기**

```java
List<Order> orders = orderRepository.findAllByString(new OrderSearch());
List<simpleOrderDto> = orders.stream()
  .map(o -> new simpleOrderDto(o))
  .collect(Collectors.toList());
```

```java
public simpleOrderDto(Order order){
orderId = order.getId();
name = order.getMember().getName();//연관관계매핑의경우
orderDate = order.getOrderDate();
...
}
```
name 값처럼 연관관계 매핑의 경우 전부 Lazy기 때문에 저런 필드가 많을수록 n+1문제(각각 전부 select날림)로 쿼리가 어마어마하게 늘어난다.그래서 연관관계 된것을 가져올 경우 패치조인이나 EntityGraph로 풀어야한다.