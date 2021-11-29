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

     * 