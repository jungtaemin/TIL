# JVM
자바 가상 머신으로 자바 바이트 코드(.class 파일)를 OS에 특화된 코드로 변환(인터프리터와 JIT컴파일러)하여 실행한다.

## 구조(클래스 로더가 읽어서 적절하게 메모리에 배치)
![jvm](https://user-images.githubusercontent.com/96284736/218015288-f89e8cf2-073d-463f-aa1a-1ad3b0fce3c5.PNG)
* 클래스 로더 시스템(자바바이트 코드 읽어들여서 메모리에 적절히 배치함)
  * 로딩(클래스 파일에서 바이트 코드를 읽어옴)
  * 링크(레퍼런스 연결하는 과정)
  * 초기화(static 값 초기화 및 변수에 할당)
* 메모리
  * 스택(쓰레드마다 런타임 스택을 만들고,그 안에다 스택 프레임을 쌓는다.쓰레드마다 하나씩 만들어짐)
  * PC(Program Counter)(쓰레드 마다 쓰레드 내 현재 실행할 스택 프레임을 가리키는 포인터 생성)
  * 네이티브 메소드 스택
  * 힙(객체)
  * 메소드(클래스이름,부모클래스이름,메소드,변수)  
* 실행엔진
  * 인터프리터(바이트 코드를 한줄 씩 실행)
  * JIT 컴파일러(반복된 코드를 모두 네이티브 코드로 바꿔둔다.)
  * GC(더이상 참조되지 않는 객체를 모아서 정리한다.)

### 참고:JDK
JRE+ 개발에 필요한 툴  
오라클은 자바 11부터는 JDK만 제공하며 JRE를 따로 제공하지 않는다.
### JRE
**JVM+라이브러리**  
자바 애플리케이션을 실행할 수 있도록 구성된 배포판.  
JVM과 핵심 라이브러리 및 자바 런타임 환경에서 사용하는 프로퍼티 세팅이나 리소스 파일을 가지고 있다.


### JVM 동작 과정 
우리가 자바 코딩을 함 ->컴파일함 -> 바이트코드만들어짐(사람이 쓰는 자바코드에서 컴퓨터가 읽는 기계어의 중단단계)  
이 바이트코드를 JVM에 갖다주면 바이트코드를 그때그떄 기계어로 통역해줌

JIT컴파일(Just In TIme컴파일):주어진 코드를 실행 시점에 그때그때 기계어로 통번역해주는 방식