# Intellij Debugging

<img width="1000" alt="Screen Shot 2021-10-16 at 8 32 22 PM" src="https://user-images.githubusercontent.com/55227984/137590178-a2ea6e1f-a520-45cc-94ec-5a9a0f88f735.png">




## 디버그 모드란?

* `BreakPoint` (빨간점) 을 눌러 해당 지점까지 실행이 되면 그 포인트에서 멈추는것 

  #### 실행방법
  * 자신이 원하는 breakpoint를 건다
  * intellij 에서는 초록색 벌레 모양![image](https://user-images.githubusercontent.com/55227984/137585832-050987db-ee3e-4e9f-ad4e-94e13f95b69e.png)을 누르면 된다.

## 기능설명

* `왼쪽`
  * 현재 breakpoint 까지 오기위해서 거쳤던 스택을 의미한다.
  
* `variables`
  * 현재 breakpoint 까지 지역변수들을 의미한다.

* `resume` 

  * 다음 breakpoint 로 넘어간다.

* `step over`

  * 다음 줄로 넘어간다. 

* `step into`

  * pdb에서 s라고 생각하면 된다.

  * 해당 줄에서 클래스가 만들어지거나 메소드가 실행되는게 있다면 해당 클래스의 생성자 또는 그 메소드 안으로 들어간다.

    * ```java
      //Car.java
      class Car{
        
        void drive(){
          System.out.println("슝슝");
        }
      }
      
      
      //Main.java
      
      class Main{
        public static void main(String[] args){
          Car tesla = new Car();
          tesla.drive(); // 여기서 step into로 들어가면 Car 클래스에 drive로 들어간다.
        }
      }
      ```

* `step out`

  * `step into` 의 정반대 개념 
  * `step into` 해서 들어갔던거를 나온다고 생각하면 된다.
  * 보통 `step into` 로 파고들어간 라인을 빠져나오려 할 때 많이 사용한다.

* `drop frame`
  * `call stack` 을 거슬러 올라간다.
  * `step out` 과 별 차이 없어 보이지만 가장 큰 차이점은 `step out` 은 해당 라인이 실행된 후 돌아가지만, `drop frame`은 해당 라인이 실행되기 전으로 돌아간다.

* `run to cursor`
  * breakpoint 를 찍지 않고도 넘어갈 수 있다. 
  * 커서가 있는쪽으로 넘어간다.
  * 귀찮을 때 활용해도 좋겠다
  
* `evaluate`
   <img width="388" alt="Screen Shot 2021-10-16 at 11 04 26 PM" src="https://user-images.githubusercontent.com/55227984/137590498-b7829595-dabb-4413-8b09-f4c25a3b3068.png">
  
  * 위의 빨간색 네모는 `evaluate` 라는 기능인데 해당 기능을 이용하면 `breakpoint` 를 기준으로 여태까지 사용했던 local 필드들에 메소드들을 사용할 수 있다.
  * 여기서 아래 그림을 보면 `Expression` 이라고 코드를 칠 수 있는 공간이 한줄로 되어있는데 이것이 불편하면 `shift+Enter` 를 치면 `code fragment` 를 칠 수 있다.
  * 아래 그림과 같이 클래스 구조가 있다고 가정해보면 `s` 에 해당하는 메소드 및 여러 부가 기능들을 사용할 수 있는것이다.(`t` 도 물론 마찬가지다.)
    * 근데 실제로 실행되는거니까 `Dao` 에 값을 넣는다던지 그런 행동은 하지말자......
     ![image](https://user-images.githubusercontent.com/55227984/137590557-1f4cb72e-06f3-42e3-be9d-d1ae7f685a00.png)
  
* `watch`

   ![image](https://user-images.githubusercontent.com/55227984/137590768-dbdd2079-b9a9-441f-8a30-1da3d361bfbf.png)
   
   * 위의 사진에서도 보이지만 intellij 에서 `breakpoint` 를 걸때 variables 는 그렇게 가독성이 좋지 않다. 
   * 따라서 별도의 기능인 `watch` 라는 기능이 있다.(위의 그림에서 `+` 버튼을 누르고 자신이 사용하고자 하는 변수를 입력하면 된다.)
   * 예를 들어 내가 `evaluate` 에서 사용했던 자바코드처럼 `t.getClass()` 등을 입력할 수도 있다.
   * 즉`variables` 를 더 잘 활용할 수 있는 방법이다.

## 꿀팁

* 그런데 생각보다 우리가 디버거를 안쓰는 이유는 간단하다. 바로 `for` 문 같은 경우이다.
  * 예를 들어 우리가 DB에서 어떤 리스트를 꺼내오는데 해당 리스트의 58번째 값을 궁금하다고 가정해보자.
  * 이 경우, 우리는 어떻게 해야할까? 하나하나 `step over` 를 찍어야 할까? 
  * 이때 쓸 수 있는게 바로 `breakpoint` 에 조건을 거는것이다.(그냥 마우스 우클릭을 하면 된다.)
   <img width="500" alt="Screen Shot 2021-10-16 at 8 32 22 PM" src="https://user-images.githubusercontent.com/55227984/137586162-dcc7d6ed-824a-41ee-9af3-00c30b63b205.png">
  
  * 이렇게 `condition` 이 걸리는것을 볼 수 있는데 이때 우리가 조건문 넣듯이 사용하면된다.
    * i == 5
    * User.getName().equals("구구")

