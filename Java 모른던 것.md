# Java 모른던 것

1. `...`

   * 자바에서 `...` 은 해당 타입을 여러 개 넘길 수 있는 조건을 가졌다.

   * ```java
     public static Order createOrder(Member member, OrderItem... orderItems) {
       // 로직들
       for (OrderItem orderItem : orderItems) {
         // 로직들
       }
     }
     
     // 호출할 때
     order.createOrder(member, delivery, orderItem1, orderItem2);
     ```

   

2. `entrySet()`

   * `Map` 에 값을 전체 출력하기 위해서는 `entrySet()`, `keySet()` 메소드를 사용하면 된다.

     * `entrySet()` 메서드는 key 와 value의 값이 모두 필요한 경우에 사용하고, `keySet()` 메서도는 `Key` 값만 필요한 경우 사용한다.

     * ```java
       Map<String, String> map = new HashMap<String, String>();
       map.put("key01", "value01"); 
       map.put("key02", "value02");
       map.put("key03", "value03");
       map.put("key04", "value04");
       map.put("key05", "value05");
       
       // 방법 01 : entrySet()
       for (Map.Entry<String, String> entry : map.entrySet()) {
       	System.out.println("[key]: " + entry.getKey() + ", [value]: "+entry.getValue());
       }
       
       // 방법 02 : keySet()
       for (String key : map.keySet()) {
         String value = map.get(key);
         System.out.println("[key]: "+ key + ", [value]: " + value);
       }
       ```

3. `StringUtils`

   * 문자열등에서 여러가지를 체크해줌

   * ```java
     StringUtils.replace("문자열", "찾을문자", "변경문자", 변경횟수);
     StringUtils.replace("문자열", "찾을문자", "변경문자");
     ```



4. 메서드 참조

   * 특정 람다 표현식을 축약한 것

   * 예를들어 `Apple::getWeight` 는 `Apple` 클래스에 정의된 `getWeight` 의 메소드 참조이다.

   * 실제로 메소드는 호출한 것이 아니므로 괄호가 필요없다. 

   * 결과적으로 메소드 참조는 람다 표현식 `(Apple a) -> a.getWeight() ` 를 축약한 것이다. 

     | 람다                                     | 메서드 참조 단축 표현 |
     | ---------------------------------------- | --------------------- |
     | (Apple apple) -> apple.getWeight()       |                       |
     | () -> Thread.currentThread().dumpStack() |                       |
     | (str. i) -> str.substring(i)             |                       |
     | (String s) -> System.out.println(s)      |                       |
     |                                          |                       |

     

5. `Collections.shuffle`

   * java에서 collection 을 섞는 방법이다. 

   * 사용방법 

     ```java
     List<String> stringList = Arrays.asList("Foo", "Bar", "Baz", "Qux");
     Collections.shuffle(stringList);sl
     ```

     

6. `List` 에 특정 값이 포함되어 있는지 확인하는 방법

   * ```
     1. contains()
     2. indexOf()
     3. Stream API
     ```

     1. `contains()`

        ```java
        public class ListValueCheck {
          public static void main(String... args) {
            List<String> list = new ArrayList<>(Arrays.asList("Hello", "Hi"));
            
            // 포함 여부 체크
            // 여기서 포함여부는 해당 객체 내의 equals() 로 정의된다. 여기선 String 이니까 String의 equals로 재정의 되는 것
            boolean isContainsHello = list.contains("Hello"); // true
            boolean isContainsBye = list.contains("Bye"); // false
            
        
          }
        }
        ```

        

     2. `indexOf()`

        ```java
        List 의 indexOf() 메소드는 같은 객체가 List 안에 있으면 해당 객체가 나타나는 첫번째 index 값을 리턴한다. 만약 List에 같은 값이 없으면 -1을 리턴한다. 이때도 contains() 메소드와 같이, 같은 객체를 찾을 때 equals() 메소드를 활용한다.
          
        public class ListValueCheck {
          public static void main(String... args) {
            List<String> list = new ArrayList<>(Arrays.asList("Hello", "Hi"));
            
            // 포함 여부 체크
            // 여기서 포함여부는 해당 객체 내의 equals() 로 정의된다. 여기선 String 이니까 String의 equals로 재정의 되는 것
            int isContainsHello = list.indexOf("Hello"); // 0
            int isContainsBye = list.indexOf("Bye"); // -1
          }
        }
        ```

     3. `Stream API` 를 사용

        ```java
        import java.util.ArrayList;
        import java.util.Arrays;
        import java.util.List;
        
        public class ListValueCheck {
          public static void main(String[] args) {
            
            List<String> list = new ArrayList<>(Arrays.asList("Hello", "Hi"));
            // Hello 포함 여부 체크
            
            long count = list.stream().filter(str -> "Hello".equals(str)).count();
            System.out.println("Count : " + count);
            if (count > 0) {
              System.out.println("Hello는 리스트에 포함된 문자열입니다.");
            }
          }
        }
        
        ```

        
        
         

7.  람다

   1. 람다란?
      * **람다 표현식**은 메서드로 전달 할 수 있는 익명 함수를 단순화한 것이라고 할 수 있다. 
      * **람다 표현식** 에는 이름은 없지만 파라미터 리스트, 바디, 반환 형식, 발생할 수 있는 예외 리스트는 가질 수 있다.
   
   2. 특징
   
      * 익명
        * 보통의 메소드와 달리 이름이 없어서 익명이라 표현한다. 
      * 함수
        * 람다는 메소드 처럼 특정 클래스에 종속되지 않아 함수라고 부른다. 
        * 하지만 메소드 처럼 파라미터 리스트, 바디, 반환형식, 가능한 예외 리스트를 포함한다.
      * 전달
        * 람다 표현식은 메소드 인수로 전달하거나 변수로 저장할 수 있다.
      * 간결성
        * 익명 클래스처럼 많은 자질 구레한 코드를 구현할 필요가 없다. 
   
   3. 기능 
   
      * 람다가 기술적으로 자바 8이전에 없었던 새로운 기능을 제공하는 것은 아니다. 
   
      * 다만 동작 파라미터등을 이용할 때 익명 클래스와 같은 판에 박힌 코드를 구현할 필요가 없다는 것이다. 
   
      * 즉, 코드가 간결하고 유연해진다. 
   
      * ```java
        Comparator<Apple> byWeight = new Comparator<Apple>() {
          public int compare(Apple a1, Apple a2) {
            return a1.getWeight().compareTo(a2.getWeight())
          }
        }
        
        
        Comparator<Apple> byWeight = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight())
        ```
   
      * 
   
8. 익명 클래스

   * 이름이 없는 클래스를 의미한다.
   * 익명 클래스를 이용하면 클래스 선언과 인스턴스화를 동시에 할 수 있다.
   * 인터페이스와 클래스로 만들 수 있다.

9. 함수형 인터페이스
   * 단 하나의 추상 메서드를 갖는 인터페이스
   * 예시로는 `Comparator` , `Runnable` 등이 있다.
   * 인터페이스의 경우 default Method 가 있더라도 추상 메서드가 오직 하나면 함수형 인터페이스이다. 

10. 메소드 시그니처

    * 메소드의 이름과 파라미터만을 메소드 시그니쳐라고 한다.![image](https://user-images.githubusercontent.com/55227984/144543229-04e363aa-ef5c-4d3f-b2a0-bc42a76829e0.png)

      