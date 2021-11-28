# Java

* ### asiterator()





* ### Iterator(반복자)?

  * `Collection` 

* ### Collection

  * List
    * LinkedList
    * Stack
    * Vector
    * ArrayList
  * Set
    * HashSet
    * SortedSet
      * TreeSet
  * Queue
    * 

* ### Map

  * Hashtable
  * HashMap
  * SortedMap

* ## default메소드

  * 자바 8의 등장으로 `interface` 에 대한 정의 몇개가 변경 되었다.

  * 인터페이스가 `default` 키워드로 선언되면 메소드가 구현될 수 있다. **또한 이를 구현하는 클래스는 `default` 메소드를 오버라이딩 할 수 있다.**

    ```java
    public interface Calculator {
      public int plus(int i, int j);
      public int multiple(int i, int j);
      default int exec(int i, int j){ //default로 선언함으로 메소드를 구현 할 수 있다. 
        return i + j;
      }
    }
    ```

    

  * 인터페이스가 변경이 되면, 인터페이스를 구현하는 모든 클래스들이 해당 메소드를 구현해야하는 문제가 있다. 따라서 인터페이스에 메소드를 구현해 놓을 수 있도록 하였다.

* ## static 메소드

  ```java
  public interface Calculator {
    public int plus(int i, int j);
    public int multiple(int i, int j);
    default int exec(int i, int j){
      return i+j;
    }
    public static int exec2(int i, int j){
      return i * j;
    }
  }
  
  
  public class MyCalculatorExam {
    public static void main(String[] args){
      Calculator cal = new MyCalculator();
      int value = cal.exec(5, 10);
      System.out.println(value);
      
      int value2 = Calculator.exec2(5, 10);
      System.out.println(value2);
    }
  }
  ```

  * 인터페이스에 `static` 메소드를 선엄함으로써, 인터페이스를 이용해 간단한 기능을 갖는 유틸리티성 인터페이스를 만들수 있다.

* ## Optional

  * `Optional<T>` 클래스는 `Integer` 나 `Double` 클래스처럼 `T` 타입의 객체를 포장해주는 래퍼 클래스이다.

  * **Optional 을 사용하면 예상하지 못한 `NullPointerException` 을 회피할 수 있다.**

    * 원래는 복잡한 조건문 등으로 처리를 해야하지만 이를 잘 해결할 수 있다.

  * 생성

    * `of()` 나 `ofNullable()` 메소드를 사용하여 `Optional` 객체를 생성 할 수 있다.

      * `of()` 메소드를 통해 생성된 `Optional` 객체에 `null` 이 저장되면 `NullPointerException` 예외가 발생한다.
      * `ofNullable()` 메소드는 명시된 값이 `null` 이 아니면 명시된 값을 가지는 `Optional` 객체를 반환하며, 명시된 값이 `null` 이면 비어있는 `Optional` 객체를 반환한다.

      ```java
      Optional<String> opt = Optional.ofNullable("자바 Optional 객체");
      System.out.println(opt.get());
      
      ---output---
      자바 Optional 객체
      ```

    * `empty()` 

      * 아무런 값도 가지지 않는 비어있는 `Optional` 객체를 반환함

  * 접근 

    * `get()` 메소드를 사용하면 `Optional` 객체에 저장된 값에 접근할 수 있다.

      * 단, `Optional` 객체에 저장된 값이 `null` 이면, `NoSuchElementException` 예외가 발생한다.
      * 따라서 `get()` 을 호출하기 전에 `isPresent()` 를 사용하여 `Optional` 객체에 저장된 값이 `null` 인지 아닌지를 확인한 후 호출하는것이 더 좋다.

      ```java
      Optional<String> opt = Optional.ofNullable("자바 Optional 객체");
      
      if(opt.isPresent()) {
        System.out.println(opt.get());
      }
      ```

    * 아래와 같은 메소드를 통해 `null` 대신에 대체할 값을 지정할 수도 있다.

      * https://homoefficio.github.io/2019/10/03/Java-Optional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%93%B0%EA%B8%B0/
      * `orElse()` : 저장된 값이 존재하면 그 값을 반환하고, 값이 존재하지 않으면 인수로 전달된 값을 지정
      * `orElseGet()` : 저장된 값이 존재하면 그 값을 반환하고, 값이 존재하지 않으면 인수로 전달된 **람다 표현식** 의 결과값을 반환함
      * `orElseThrow()` : 저장된 값이 존재하면 그 값을 반환하고, 값이 존재하지 않으면 인수로 전달된 예외를 발생시킴 

* ## StringUtils

  * 문자열에 작업하는 관련기능들을 모아놓은 라이브러리
  * `StringUtils.defaultIfBlank(문자열, 반환할 문자열)` 
    * 문자열이 띄어쓰기, 공백, null 일 경우 반환할 문자열로 대체한다.

* ## Stream

  * ### 장점
    * 간단하게 병렬처리가 가능하다.
    
  * ### 과정

    1. 생성하기
    2. 가공하기
    3. 결과 만들기

  * ### 생성하기

    * #### 배열스트림

      * 스트림은 배열 또는 컬렉션 인스턴스를 이용해서 생성할 수 있다.

      ```java
      String[] arr = new String[]{"a", "b", "c"};
      Stream<String> stream = Arrays.stream(arr);
      Stream<String> streamOfArrayPart = Arrays.stream(arr, 1, 3);
      ```
      
    * #### 컬렉션 스트림
    
      * 컬렉션 타입(Collection, List, Set) 의 경우 인터페이스 추가된 디폴드 메소드 `stream` 을 사용해서 스트림을 만들 수 있다.
    
    



* ## 익명 클래스

  * 말 그대로 이름이 없는 클래스를 의미한다.

    ```java
    public class Test{
      private int num = 1;
      public int getNum(){
        return this.num;
      }
      
      public void setNum(int num){
        this.num = num;
      }
    }
    
    // 익명 클래스가 아닌 클래스
    Test t1 = new Test();
    
    // 익명 클래스
            Test t1 = new Test(){
                public int num = 10;
                @Override
                public int getNum(){
                    return this.num;
                }
            };
        }
    ```

  * 보기에도 알 수 있지만 새롭게 정의된 Test를 볼 수 있다 오히려 상속받은거라고도 말할 수 있겠다.



* ## 값 타입 복사

  ```java
  //자바의 경우 주의 해야할 점이 있다 바로 객체 타입에 값을 대입 하면 항상 참조 값을 전달한다는 문제이다. 
  
  int a = 10;
  int b = 10;
  
  b = a;
  a = 4;
  
  // 이때 a는 4 b는 10을 가지게 된다. 하지만 객체 타입의 경우 그렇지 않다. 
  
  City cityA = new City("seoul");
  City cityB = new City("new york"); 
  
  cityB = cityA;
  cityA.setName("paris");
  
  // 이 경우는 cityB도 paris가 되고 cityA도 paris가 된다.
  ```

  

* ## 얕은복사와 깊은복사

  * ### 얕은복사

    * 한쪽에서 수정이 발생하면 다른쪽에도 영향을 끼쳐 값이 같아지게 된다.
    * 객체의 참조값 을 복사한다.
    * 대표적으로 `=` 는 얕은복사에 해당한다.

  * ### 깊은복사

    * 주소값을 참조하는 것이 아닌, 새로운 메모리 공간에 값을 복사하는 것이기 때문에 원본 배열이 변경되어도 복사된 배열엔 상관없다.
    * 객체의 실제값을 복사한다.
    * 한쪽 값을 수정해도 다른 배열에 영향을 끼치지 않는다.
