# DTO 와 VO



* `DTO`  : 데이터 전달용

  * Data Transfer Object

  * 데이터를 전달하기 위해 사용하는 객체

  * 데이터를 담아서 전달하는 바구니

    ![image](https://user-images.githubusercontent.com/55227984/138800497-2d7e5b6e-3d98-426c-b96e-7186210164ad.png)

    * 오직 `getter/setter` 메서드 만을 갖는다.
    * 다른 로직을 가지지 않는다.

    ![image](https://user-images.githubusercontent.com/55227984/138800730-954edcc6-6eae-4e6b-af77-ada823510612.png)

    * 위와 같이 사용하여(`setter 제거 하고 생성자로 값을 공급받음`) 불변객체로 사용해 더욱 안정성을 높일 수 있다.

  ![image](https://user-images.githubusercontent.com/55227984/138800898-0a94ec8a-7db8-41ce-a1a8-c90c0a1047f1.png)

  * `Entity` 와 `Dto` 는 꼭 분리해서 사용하여야 한다.

* `VO` : 값 표현용  

  * `Value Object`
  
  * 값 그 자체를 표현하는 객체
  
  * 두 객체의 모든 필드의 값이 동일하면 두 객체는 같은 객체이다. 
  
    * 이를 위해서 `hashCode` 와 `equals` 메소드를 오버라이딩 해야한다.
  
      <img src="https://user-images.githubusercontent.com/55227984/138802358-9e6df57a-3005-4fe4-9844-4fde843a464b.png" width = "800" height="300"/>

<img src="https://user-images.githubusercontent.com/55227984/138801681-88473fdd-7a4d-4705-9581-364ecd1f6a4f.png" width = "800" height="500"/>



* 위의 사진처럼 무조건 `VO` 는 불변객체로 만들어야 한다. 그래야 값이 바뀌지 않았다는것을 보장할 수 있기 때문이다.

## 결론

|             | DTO                                             |                    VO                     |
| :-----------: | :-----------------------------------------------: | :---------------------------------------: |
| 용도        | 레이어 간 데이터 전달                           |               값 자체 표현                |
| 동등 결정   | 속성값이 모두 같다고 해서 같은 객체가 아니다    |     속성값이 모두 같으면 같은 객체다      |
| 가변 / 불변 | `setter` 존재 시 가변, `setter` 비 존재 시 불변 |                   불변                    |
| 로직        | `getter/setter` 외의 로직을 갖지 않는다.        | `getter/setter` 외의 로직을 가질 수 있다. |

  