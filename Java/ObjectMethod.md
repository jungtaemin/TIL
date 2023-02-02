# ObjectMethod
* [toString()]()
* [equals()]()
* [clone()]()
* [hashCode()]()

참고 
>http://www.tcpschool.com/java/java_api_object


# toStrng()
toString() 메소드는 해당 인스턴스에 대한 정보를 문자열로 반환합니다.

이때 반환되는 문자열은 클래스 이름과 함께 구분자로 '@'가 사용되며, 그 뒤로 16진수 해시 코드(hash code)가 추가됩니다.

16진수 해시 코드 값은 인스턴스의 주소를 가리키는 값으로, 인스턴스마다 모두 다르게 반환됩니다.

```java
Car car01 = new Car();

Car car02 = new Car();

 

System.out.println(car01.toString());

System.out.println(car02.toString());
```
**실행결과**
>Car@15db9742  
Car@6d06d69c

# equals()
equals() 메소드는 해당 인스턴스를 매개변수로 전달받는 참조 변수와 비교하여, 그 결과를 반환합니다.

이때 참조 변수가 가리키는 값을 비교하므로, 서로 다른 두 객체는 언제나 false를 반환하게 됩니다.

```java
Car car01 = new Car();

Car car02 = new Car();

 

System.out.println(car01.equals(car02));

car01 = car02; // 두 참조 변수가 같은 주소를 가리킴.

System.out.println(car01.equals(car02));
```
**실행결과**
>false  
true

# clone()
clone() 메소드는 해당 인스턴스를 복제하여, 새로운 인스턴스를 생성해 반환합니다.

하지만 Object 클래스의 clone() 메소드는 단지 필드의 값만을 복사하므로, 필드의 값이 배열이나 인스턴스면 제대로 복제할 수 없습니다.

따라서 이러한 경우에는 해당 클래스에서 clone() 메소드를 오버라이딩하여, 복제가 제대로 이루어지도록 재정의해야 합니다.
이러한 clone() 메소드는 데이터의 보호를 이유로 Cloneable 인터페이스를 구현한 클래스의 인스턴스만이 사용할 수 있습니다.

 

다음 예제는 clone() 메소드를 이용하여 인스턴스를 복제하는 예제입니다.
```java
import java.util.*;

 

class Car implements Cloneable {

    private String modelName;

①  private ArrayList<String> owners = new ArrayList<String>();


    public String getModelName() { return this.modelName; }                    // modelName의 값을 반환함
 public void setModelName(String modelName) { this.modelName = modelName; } // modelName의 값을 설정함
 public ArrayList getOwners() { return this.owners; }                      // owners의 값을 반환함

    public void setOwners(String ownerName) { this.owners.add(ownerName); }   // owners의 값을 추가함

 

    public Object clone() {

        try {

②          Car clonedCar = (Car)super.clone();

③          // clonedCar.owners = (ArrayList)owners.clone();

            return clonedCar;

④      } catch (CloneNotSupportedException ex) {

            ex.printStackTrace();

            return null;

        }

    }

}

 

public class Object03 {

    public static void main(String[] args) {

⑤      Car car01 = new Car();

        car01.setModelName("아반떼");

        car01.setOwners("홍길동");

⑥      System.out.println("Car01 : " + car01.getModelName() + ", " + car01.getOwners() + "\n");

 

⑦      Car car02 = (Car)car01.clone();

⑧      car02.setOwners("이순신");

⑨      System.out.println("Car01 : " + car01.getModelName() + ", " + car01.getOwners());

⑩      System.out.println("Car02 : " + car02.getModelName() + ", " + car02.getOwners());

    }

}    
```
**실행결과**
>Car01 : 아반떼, [홍길동]  
Car02 : 아반떼, [홍길동, 이순신] 
Car02 : 아반떼, [홍길동, 이순신]


# hashcode()
해당 객체의 해시 코드값을 반환함.