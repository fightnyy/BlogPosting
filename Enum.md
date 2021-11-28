# Enum

* 열거형(enumerated type) 이라고도 부른다.
* 서로 연관된 상수들의 집합
  * 이렇게 함으로써 서로 다르게 정의된 `Enum` 은 비교할 수 없다는 특징을 지닌다.
* 열거형 자체가 `class` 이기 때문에 내부에서 `생성자, 필드, 메소드` 를 가질 수 있다.
* 값들이 변경되지 않도록 보장한다. 
* 또한 `values()` 라는 메소드를 통하여 상수들을 담은 배열로 처리할 수 있다.  



```java
class Fruit{
  public static final Fruit APPLE = new Fruit();
  public static final Fruit PEACH = new Fruit();
  public static final Fruit BANANA = new Fruit();
}

enum Fruit{
  APPLE, PEACH, BANANA
}


//위의 코드 2개는 정확히 동일한 의미를 가진다고 생각하면 된다.
//즉 enum은 사실 클래스이다. 
```



```java
class Fruit{
  public static final Fruit APPLE = new Fruit();
  public static final Fruit PEACH = new Fruit();
  public static final Fruit BANANA = new Fruit();
}

enum Fruit{
  APPLE("red"), PEACH("pink"), BANANA("yellow")
  
  private String color; // 필드도 가질 수 있다.
  
  public String getColor(){ // 메소드도 가질 수 있다.
    return this.color;
  }
     
  Fruit(){ // 생성자도 가질수 있다.
    System.out.println(:"call Constructor " + this) // class와 동일한 값이라는걸 다시 한번 증명한다. 
  } 
  
  Fruit(String color) {
    this.color = color
  }
}


class Print{
  public static void main(String[] args) {
    for(Fruit f : Fruit.values()){
      System.out.println(f);
    }
  }
}


```

