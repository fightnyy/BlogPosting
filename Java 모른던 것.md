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

     