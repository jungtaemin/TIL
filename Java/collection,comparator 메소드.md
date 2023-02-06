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

## 기존 메서드
### HashMap
* put(K key, V value)  
HashMap에 Key, Value을 삽입하는 메소드입니다.
```java
map.put("신논현", 1);
map.put("강남", 2);
map.put("혜화", 3);
map.put("안양", 4);
map.put("수원", 5);
```
각 Key, Value에 대한 타입에 맞게 선헌하면 map 객체에 저장됩니다.

* containsKey(Object Key)


```java
HashMap<String , Integer> map = new HashMap<String , Integer>();

    map.put("신논현", 1);
    map.put("강남", 2);
    map.put("혜화", 3);
    map.put("안양", 4);
    map.put("수원", 5);

    System.out.println(map.containsKey("혜화"));
```
만약 엘리먼트 내에 Key가 포함되어있다면 boolean 값으로 true, 아니라면 false가 리턴됩니다.
* containsValue(Object Value)

```java
HashMap<String , Integer> map = new HashMap<String , Integer>();

    map.put("신논현", 1);
    map.put("강남", 2);
    map.put("혜화", 3);
    map.put("안양", 4);
    map.put("수원", 5);

    System.out.println(map.containsValue(5));
```
containsKey와 마찬가지로 엘리먼트 내에 Value가 포함되어있다면 boolean 값으로 true, 아니라면 false가 리턴됩니다.