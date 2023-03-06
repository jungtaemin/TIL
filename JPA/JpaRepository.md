# JpaRepository
**어떻게 인터페이스만으로 동작할까?**  
인터페이스를보고 스프링데이터JPA가 구현클래스를 만들어서 꽂아높은거다.(프록시)

* JpaRepository extends하기

```java
public interface MemberRepository extends JpaRepository<Member, Long>{

}
```

인터페이스를 만들고 JpaRepository<타입,pk로 매핑된 타입> 인터페이스를 extends 하면된다.(인터페이스끼리는 extends)

## 주요 메서드
> save(entity)

새로운 엔티티는 저장하고 이미 있는 엔티티는 병합한다.
> delete(entity)

엔티티 하나를 삭제한다.내부에서'EntityManager.remove()'호출

> findById(ID)

엔티티 하나를 조회한다.내부에서'EntityManager.find()'호출
> getOne(ID)

엔티티를 프록시로 조회한다.내부에서'EntityManager.getReference()'호출
> findAll()

모든 엔티티를 조회한다.정렬(Sort)이나 페이징(pageable)조건을 파라미터로 제공할 수 있다.
## 쿼리 메소드 기능(간단한것 할때)
**메소드 이름으로 쿼리 생성**
find...by  등 by앞에는 아무거나 적어도됨  
반환값 기본:List<?>  단건일경우:그 타입,Optional<타입>
* **조회**: findBy...
* **COUNT** : countBy...  반환타입 : long
* **EXISTS** : existsBy...   반환타입 : boolean
* **삭제** : deleteBy...   반환타입 : long
* **DISTINCT** : findDistinctBy,findDistinct
* **LIMIT** : findTop3By

By 뒤는 where 조건문 
> List<?> findByLastname(parameter)

...where x.Lastname = ?   
By뒤에 필드에 해당하는 파라미터값이 일치하는 데이터 들고옴

* 필드값 두개이상 조건으로 할때 
> List<?> findByLastnameAndFirstname(parameter,parameter) 

...where x.lastname = ?1 and x.firstname = ?2

> List<?> findByLastnameOrFirstname(parameter,parameter)  

...where x.lastname = ?1 or x.firstname = ?2

> !이 기능은 엔티티의 필드명이 변경되면 인터페이스에 정의한 메서드 이름도 꼭 함께 변경해야 한다.그렇지 안흥면 애플리케이션을 시작하는 시점에 오류가 발생한다.이렇게 애플리케이션 로딩 시점에 오류를 인지할 수 있는 것이 스프링 데이터 JPA의 매우 큰 장점이다.


## @Query(복잡한것 할때)
**리포지토리 메소드에 쿼리 정의하기**   
JPQL을 바로 적을 수 있다  
반환값 기본:List<?>  단건일경우:그 타입,Optional<타입>  
```java
@Query("select m from Member m where m.username = :username and m.age = :age")
List<Member> findUser(@Param("username") String username,@Param("age") int age);
```

* 단순한 값을 가져오고 싶을 때
```java
@Query("select m.username from Member m")
List<String> findUsernameList();
```
* 컬렉션으로 in 절 
```java
@Query("select m from Member m where m.username in :names")
List<Member> findByNames(@Param("names") Collection<String> names);
```
## QueryDSL(동적쿼리)
## 자주쓰는 annotation

### 패치조인,EntityGraph

> **@EntityGraph(간단한 조인일 경우)**
```java
@Override
@EntityGraph(attributePaths ={"team"})
List<Member> findAll();
```
* EntityGraph은 패치조인을 쉽게해주는 애노테이션이다. 지연로딩이라서 조인된 entity를 가짜 프록시객체로 참조하다가 불러왔을때 다시 select하는데 EntityGraph를 사용하면 조인해서 같이 가져온다.

> **패치조인**(복잡한 조인일 경우)
```java
"select o from Order o join fetch o.member m"
```
> **QueryHints**(생각보다 쓸일 없다.)
```java
@QueryHints(value = @QueryHint(name = "org.hibernate.readOnly", value ="true"))
Member findReadOnlyByUsername(String username);
```
원래는 find만 하면 변경감지를 사용하기위해 스냅샷도 하는데 QueryHints readOnly를 쓰면 스냅샷을 만들지않는다.(성능최적화인데 생각보다 성능최적화가 크지는않다.)

> **@Lock**
```java
@Lock(LockModeType.PESSIMISTIC_WRITE)
List<Member> findLockByUsername(String username);
```

### 권장 순서
엔티티 조회 방식으로 우선 접근 ->  
페치조인으로 쿼리 수를 최적화(manytoone관계) ->  
컬렉션은..(OneToMany) ->  
페이징 필요: hibernate.default_batch_fetch_size ,@BatchSize로 최적화  
페이징 필요 x : 페치 조인 사용  ->  
엔티티 조회 방식으로 해결이 안되면 DTO 조회 방식 사용->  
DTO 조회 방식으로 해결이 안되면 NativeSQL or 스프링 JdbcTemplate