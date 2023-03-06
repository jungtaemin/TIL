# service

```java
@Service
@Transactional(readOnly = true)
public class MemberService{

@Transactional
public Long join(Member member){
    validateDuplicateMember(member);//validation
    memberRepository.save(member);
} 

}  
```
JPA의 모든 데이터 변경이나 로직은 가급적 transaction 안에서 실행되어야한다.(그래야 lazy로딩등이 된다.)
>transactional은 두개가 있다   
>Transactional (org.springframework.transaction)
>Transactional (javax.transaction) 

org.springframework.transaction을 권장!  
스프링을 쓰고있기도 하고 쓸 수 있는게 더 많다.

> @Transactional(readOnly = true)
* JPA가 조회하는곳에서는 성능을 더 최적화 한다.(**플러시**, 더티체킹안함 등)
* 읽기에는 readOnly하자.
* 읽기가 많으면 위에처럼 설정 위에 readOnly설정하고 아닌것만 그냥 transaction해놓자.