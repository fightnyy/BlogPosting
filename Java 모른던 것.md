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

   