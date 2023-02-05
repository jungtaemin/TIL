# collection,comparator 메소드

## 자바 8 에서 추가된 메소드
### Iterable
* forEach()
```java
List<String> name = new ArrayList<>();
name.forEach(System.out::println);
```
파라미터 consumer 타입
* spliterator()
```java
List<String> name = new ArrayList<>();
Spliterator<String> spliterator = name.spliterator();
Spliterator<String> spliterator1 = spliterator.trySplit();
while (spliterator.tryAdvance(system.out::println));
while (spliterator1.tryAdvance(system.out::println));
```  
파라미터 consumer 타입
### Collection
* stream() / parallelStream()
* removeIf(Predicate)
```java
List<String> name = new ArrayList<>();
name.removeIf(s ->s.startsWith("k"));
``` 
이러면 k로 시작하는 list값 없어짐
* spliterator()
### comparator(함수형 인터페이스)
* reversed()
* thenComparing()
* static reverseOrder() / naturalOrder()
* static nullFirst() / nullsLast()
* static comparing()
