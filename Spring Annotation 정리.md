왜 저는 스프링에서 어노테이션들을 볼떄마다 처음보는거 같을까요? 이번기회에 다 정리하려고 합니다. 이 상태로는 정말 안되겠어요 ㅠㅠ 

![노홍철/무한도전 - 나무위키](data:image/jpeg;base64,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)

#### @Component

* Component-scan을 선언에 의해 특정 패키지 안의 클래스들을 스캔하고 해당 어노테이션이 있는 클래스에 대해서 bean을 생성합니다.



#### @Qualifier

* 같은 타입의 빈이 두 개 이상이 존재하는 경우에 스프링이 어떤 빈을 주입해야 할 지 안ㄹ 수 없어서 스프링 컨테이너를 초기화하는 과정에서 예외를 발생시킨다. 
* 이 경우 @Qualifier과 @Autowired를 함께 사용하여 정확히 어떤 bean을 사용할지 지정하여 특정 의존 객체를 주입할 수 있도록 한다.



#### @Inject

* @Autowired와 동일한 기능을 수행하지만 이는 JSR 표준이다. 따라서 Spring 프레임워크 이외에는 해당 어노테이션을 써야하는데 그럴일은 거의 없다.



#### @Valid

* 요청데이터를 검증하는 어노테이션
* 만약 어떤 DTO에 여러 javax.validation(@NonNull, @Size)와 같은 검증을 하고 이에 대한 것을 파라미터로 받을때 예를들어 @Valid UserDto userdto 라고 하면 여기서 userdto가 여러 검증과 일치하는지 확인해 주는 역할을 하고 맞지 않으면 검증예외를 알려줍니다.



#### @Builder

* 어느 필드에 어떤 값을 채워야 할지 명확하게 정하여 생성시점에 값을 채워줍니다.

* 그렇다면 생성자와 뭐가 다를까요?
  * 생성 시점에 값을 채워주는 역할은 똑같습니다.
  * 하지만 빌더를 사용한다면 어느 필드에 어떤 값을 채워야 할지 명확하게 인지할 수 있습니다.



#### @Jsonignore

* 필드 레벨에 적용되어 해당 필드를 Jackson이 무시할 수 있도록 합니다. @JsonignoreProperties가 클래스 레벨에서 무시할 필드를 지정해준다면 @JsonIgnore는 필드 하나하나에 붙여 무시하도록 지정하는 방식입니다.



```java
public class User{
  @JsonIgnore
  public int id;
  
  public String name;
}


/** @JsonIgnore 적용 전 **/
{
  "id" : 1,
  "name" : "My Bean"
}

/** @JsonIgnore 적용 후 **/
{
  "name" : "My Bean"
}
```





#### @RequestParam

* HTTP GET 요청에 대해 매칭되는 request parameter 값이 자동으로 들어갑니다.

  * Url 뒤에 붙는 parameter 값을 가져올 때 사용합니다.

  * http://localhost:8080/home?index=1&page2

    ```java
    @GetMapping("/home")
    public String show(@RequestParam("page") int pageNum)
    {
      System.out.println(pageNum);
    }
    
    // 결과: 2
    ```

    

#### @PathVariable

* Http 요청에 대해 매칭되는 request parameter 값이 자도으로 들어갑니다.

  * uri에서 각 구분자에 들어오는 값을 처리할 때 사용합니다.

  * http://localhost:8080/index/1

  * REST API에서 값을 호출 할때 주로 많이 사용합니다.

    ```java
    @PostMapping("/index/{idx}")
    @ResponseBody
    public boolean deletePost(@PathVariable("idx") int postNum)
    {
      System.out.println(postNum);
    }
    
    //결과 1
    ```

    

#### @ModelAttribute

* view에서 전달해주는 parameter를 class(VO/DTO)의 멤버 변수로 binding 해주는 Annotation입니다.
* binding이 되는 기준은 class의 멤버 변수명과 setter의 method명이 일치해야 합니다.



#### @Param

* 메소드의 파라미터를 이름 붙여진 파라미터를 통해 쿼리에 bind시키는 어노테이션입니다.

  * ex

    ```java
    @Query("select m from Member m where m.username= :username and m.age =:age")
    List<Member> findUser(@Param("username")String username, @Param("age") int age);
    ```





#### @Entitygraph

* 사실상 fectch join(어떤 entity를 가져올때 연관된 entity를 한번에 가져오는것)을 쉽게 하기 위해 사용



#### @MappedSuperclass

* 객체 입장에서 공통 매핑 정보가 필요할 때 사용합니다.
* 주로 등록일, 수정일, 등록자, 수정자 같은 전체 엔티티에서 공통으로 적용하는 정보 모을 때 사용합니다.
* 참고로 JPA에서 @Entity 클래스는 @Entity나 @MappedSuperclass로 지정한 클래스만 상속할 수 있다.



#### @Primary

* 어떤 interface의 구현체가 2개이상일 때 특정 구현체에 @Primary 를 붙여주면 해당 구현체가 우선순위를 가짐
* 단, Qualifier가 더 우선순위가 높음



#### @NoArgsConstructor

* 파라미터가 없는 기본 생성자를 생성해줍니다.



#### @AllArgsConstructor

* 모든 필드 값을 파라미터로 받는 생성자를 만들어줍니다.



#### @RequiredArgsConstructor

* `final` 이나 `@NonNull` 인 필드 값을 파라미터로 받는 생성자를 만들어줍니다.
* "생성자의 파라미터가 하나이면 @Autowired를 안써도 된다"라는 말은 @RequiredArgsConstructor를 사용할 때 입니다. AllArgsconstructor는 안됩니다!



#### @Tranactional

* transaction `begin`, `commit`을 자동으로 수행해줍니다.
* 한 작업 내에 어떤 작업에서 예외가 발생하면 `rollback`(all or nothing) 처리를 자동으루 수행해줍니다. 
* 사실 해당 어노테이션을 사용할 때 헷갈린 점이 많이 있습니다. 그 문제는 [여기][https://mommoo.tistory.com/92]서 잘 다뤄주고 있으니 해당 부분을 참고해주시면 될 거 같습니다. (참고로 update 쿼리는 레포에서 찾을때 발생합니다)