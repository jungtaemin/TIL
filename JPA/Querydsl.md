# Querydsl

## 검색 조건
where() 안에 들어감
```java
member.username.eq("member1") // username = 'member1'
member.username.ne("member1") // username != 'member1'
member.username.eq("member1").not() // username != 'member1'

member.username.isNotNull() // 이름이 is not null

member.age.in(10,20) // age in (10,20)
member.age.notIn(10,20) // age not in (10,20)
member.age.between(10,30) //between 10, 30

member.age.goe(30) // age >= 30
member.age.gt(30) // age > 30
member.age.loe(30) // age <= 30
member.age.lt(30) // age < 30

member.username.like("member%") // like 검색
member.username.contains("member") // like '%member%' 검색
member.username.startWith("member") // like 'member%' 검색
```

## 결과조회
마지막에 들어감
```java
.fetch();
```
리스트조회,데이터 없으면 빈 리스트 반환
```java
.fetchoOne();
```
단건조회 결과 없으면 null 둘이상이면 NonUniqueResultException
```java
.fetchFirst();
```
limit(1).fetchOne() 
```java
.fetchResults();
```
페이징용. getTotal도 제공(totalc count)
```java
.fetchCount();
```
count 쿼리로 변경해서 count 수 조회

## 정렬
```java
List<Member> result = queryFactory
  .selectFrom(member)
  .where(member.age.eq(100))
  .orderBy(member.age.desc(),member.username.asc().nullsLast())
  .fetch();
```
nullsLast() : null은 마지막에 출력

## 페이징
```java
List<Member> result = queryFactory
  .selectFrom(member)
  .orderBy(member.username.desc())
  .offset(1)
  .limit(2)
  .fetch();
```
전체조회count는 .fetchResults()로 바꾸면됨

## 집합 
select 절에 들어감
```java
List<Tuple> result = queryFactory
  .select(
    member.count(),
    member.age.sum(),
    member.age.avg(),
    member.age.max(),
    member.age.min()
  )
  .from(member)
  .fetch();
```

groupBy 예시(팀의 이름과 각 팀의 평균 연력 구하는 쿼리)
```java
List<Tuple> result = queryFactory
  .select(team.name, member.age.avg())
  .from(member)
  .join(member.team, team)
  .groupBy(team.name)
  .fetch();
```
having() 예시
```java
.groupBy(item.price)
.having(item.price.gt(1000))
```
## 조인
* 기본 조인  
조인의 기본 문법은 첫 번째 파라미터에 조인 대상을 지정하고,두 번째 파라미터에 별칭으로 사용할 Q타입을 지정하면 된다.
```java
join(조인 대상, 별칭으로 사용할 Q타입)
```
기본 조인 예시 (팀 A에 속한 모든 팀원)
```java
List<Member> result = queryFactory
  .selectFrom(member)
  .join(member.team,team)
  .where(team.name.eq("teamA"))
  .fetch();
```
lerfjoin,rightjoin도 가능

* on절
```java
List<Member> result = queryFactory
  .select(memberm, team)
  .from(member)
  .leftJoin(member.team,team).on(team.name.eq("teamA"))
  .fetch();
```
* 페치조인(기본조인과 똑같은데 join뒤에 fetchJoin을 적어주면된다.)

```java
List<Member> result = queryFactory
  .selectFrom(member)
  .join(member.team,team).fetchJoin()
  .where(team.name.eq("teamA"))
  .fetch();
```
* case 문
```java
List<Member> result = queryFactory
  .select(member.age
          .when(10).then("열살")
          .when(20).then("스무살")
          .otherwise("기타"))
  .from(member)        
  .fetch();
```

## 프로젝션 결과 반환 
프로젝션 : select 대상 지정 
* 프로젝션 대상이 하나 
```java
List<String> result = queryFactory
  .select(member.username)
  .from(member)
  .fetch();
```
프로젝션 대상이 하나면 타입을 명확하게 지정할 수 있음  
프로젝션 대상이 둘 이상이면 튜플이나 DTO로 조회
* 튜플 조회
```java
List<Tuple> result = queryFactory
  .select(member.username,member.age)
  .from(member)
  .fetch();
```
* DTO로 조회
> Setter로 생성해서 조회
```java
queryFactory
  .select(Projections.bean(MemberDto.class,
           member.username,
           member.age))
  .from(member)
  .fetch();        
```
>field로 바로 넣어서 조회

```java
queryFactory
  .select(Projections.fields(MemberDto.class,
           member.username,
           member.age))
  .from(member)
  .fetch();        
```

>생성자로 넣어서 조회

```java
queryFactory
  .select(Projections.constructor(MemberDto.class,
           member.username,
           member.age))
  .from(member)
  .fetch();        
```
>@QueryProjection 사용하기 
사용할 dto 생성자 위에 @QueryProjection을 붙인다.
```java
queryFactory
  .select(new QMemberDto(member.username,member.age))
  .from(member)
  .fetch();        
```

## 동적쿼리 
* BooleanBuilder
```java
BooleanBuilder builder = new BooleanBuilder();
if(usernameCond != null){
    builder.and(member.username.eq(usernameCond));
}
if(ageCond != null){
  builder.and(member.age.eq(ageCond));
}
return queryFactory
        .selectFrom(member)
        .where(builder)
        .fetch();
```
* where 다중 파라미터 사용(장점 재사용가능)
where 에 null 이 들어가면 무시가된다.
```java
return queryFactory
        .selectFrom(member)
        .where(usernameEq(usernameCond), ageEq(ageCond))
        .fetch();

private BooleanExpression usernameEq(String usernameCond){
  return usernameCond != null ? member.username.eq(usernameCond) : null;
}  
private BooleanExpression ageEq(Integer ageCond){
  return ageCond != null ? member.age.eq(ageCond) : null;
}      
```