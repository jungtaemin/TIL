# Strong,Soft,Weak,Phantom
# 레퍼런스 종류 
> Strong,Soft,Weak,Phantom

**Strong** - 기본적으로 쓰는new,빌더..등 오브젝트 기본적으로 할당하면 Strong  
**Soft**  - new SoftReference<>(strong) 스트롱레퍼런스 넣어서만듬.스트롱레퍼런스가 없어졌을때 메모리가 부족할때만 GC로 없어짐.  
**Weak** - new WeakRefernce<>(strong) 스트롱레퍼런스 넣어서만듬.GC가 일어날때 무조건없어짐.메모리랑 상관없이.  
**Phantom** - 유령 레퍼런스.팬텀레퍼런스가 스트롱레퍼런스 대신에 남아있음.팬텀 레퍼런스는 큐가 있어야함

* soft,weak,phantom 은 쓰는 경우를 극히 드물게 볼것이다.거의 못본다.
```java
RefernceQueue<BigObject> rq = new ReferenceQueue<>();
PhantomReference<BigObject> phantom = new PhantomReference<>(strong, rq);
```
팬텀은 무조건 큐를 넘겨줘야한다.
## WeakHashMap
더이상 사용하지 않는 객체를 GC할 때 자동으로 삭제해주는 Map  
원래 기존의 map이나list는 안에 들어있는 엔트리들을 바로 없애주지 않는다.  
한번 들어가면 계속들어가 있는거다.누군가 꺼내서 비워주지 않으면.  
Key가 더이상 강하게 레퍼런스되는 곳이 없다면 해당 엔트리를 제거.
