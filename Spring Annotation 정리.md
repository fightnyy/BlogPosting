왜 저는 스프링에서 어노테이션들을 볼떄마다 처음보는거 같을까요? 이번기회에 다 정리하려고 합니다. 이 상태로는 정말 안되겠어요 ㅠㅠ

![노홍철/무한도전 - 나무위키](data:image/jpeg;base64,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)

#### @Component

* Component-scan을 선언에 의해 특정 패키지 안의 클래스들을 스캔하고 해당 어노테이션이 있는 클래스에 대해서 bean을 생성합니다.

#### @Qualifier

* 같은 타입의 빈이 두 개 이상이 존재하는 경우에 스프링이 어떤 빈을 주입해야 할 지 알 수 없어서 스프링 컨테이너를 초기화하는 과정에서 예외를 발생시킨다.
* 이 경우 @Qualifier과 @Autowired를 함께 사용하여 정확히 어떤 bean을 사용할지 지정하여 특정 의존 객체를 주입할 수 있도록 한다.

#### @Inject

* @Autowired와 동일한 기능을 수행하지만 이는 JSR 표준이다. 따라서 Spring 프레임워크 이외에는 해당 어노테이션을 써야하는데 그럴일은 거의 없다.

#### @Valid

* 요청데이터를 검증하는 어노테이션
* 만약 어떤 DTO에 여러 javax.validation(@NonNull, @Size)와 같은 검증을 하고 이에 대한 것을 파라미터로 받을때 예를들어 @Valid UserDto userdto 라고 하면 여기서
  userdto가 여러 검증과 일치하는지 확인해 주는 역할을 하고 맞지 않으면 검증예외를 알려줍니다.

#### @Builder

* 어느 필드에 어떤 값을 채워야 할지 명확하게 정하여 생성시점에 값을 채워줍니다.

* 그렇다면 생성자와 뭐가 다를까요?
    * 생성 시점에 값을 채워주는 역할은 똑같습니다.
    * 하지만 빌더를 사용한다면 어느 필드에 어떤 값을 채워야 할지 명확하게 인지할 수 있습니다.

#### @Jsonignore

* 필드 레벨에 적용되어 해당 필드를 **Jackson이 무시**할 수 있도록 합니다. @JsonignoreProperties가 클래스 레벨에서 무시할 필드를 지정해준다면 @JsonIgnore는 필드 하나하나에 붙여 무시하도록 지정하는 방식입니다.

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

### @JsonInclude

* `@JsonInclude` 는 특정 조건에 해당하는 `property` 를 제외하고 `se/deserialize` 할 수 있도록 도와준다. 아래의 예시는 `NON_NULL` 조건으로 `null` 인 `property` 는 대상에서 제외하게 된다.

```java
@JsonInclude(Include.NON_NULL)
public class MyBean {
  public int id;
  public String name;
}

@Test
public void testJsonInclude() throws JsonProcessingException {
  MyBean myBean = new MyBean(1, null);
  
  System.out.println(new ObjectMapper().writeWithDefaultPrettyPrinter().writeValueAsString(myBean));
}

/** @JsonInclude 적용전 */
{
  "id" : 1,
  "name" : null
}

/** @JsonInclude 적용 후 */
{
  "id" : 1
}
```



### @JsonProperty

* 객체의 JSON 변환 시 key 의 이름을 개발자가 원하는 대로 설정할 수 있게 한다.

  ```java
  @Getter
  @Setter
  public class User {
    @JsonProperty("user_id")
    private String userId;
    
    @JsonProperty("user_password")
    private String userPassword;
  }
  
  //Test.java
  class Test {
    @Test
    void myTest(){
      User user = new User();
      user.setUserId("youngyun");
      user.serUserPassword("hello");
      
      System.out.println(new ObjectMapper().writeValueAsString(user));
    }
  }
  // result
  // 만약 @JsonProperty 를 하지 않았다면
  {"userId" : "yougyun", "userPassword": "hello"}
  // @JsonProperty 한 결과
  {"user_id" : "youngyun", "user_password" : "hello"}
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

* Http 요청에 대해 매칭되는 request parameter 값이 자동으로 들어갑니다.

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

* 사실상 fectch join(어떤 entity를 가져올때 연관된 entity를 한번에 가져오는것)을 쉽게 하기 위해 사용합니다.

#### @MappedSuperclass

* 객체 입장에서 공통 매핑 정보가 필요할 때 사용합니다.
* 주로 등록일, 수정일, 등록자, 수정자 같은 전체 엔티티에서 공통으로 적용하는 정보 모을 때 사용합니다.
* 참고로 JPA에서 @Entity 클래스는 @Entity나 @MappedSuperclass로 지정한 클래스만 상속할 수 있습니다.

#### @Primary

* 어떤 interface의 구현체가 2개이상일 때 특정 구현체에 @Primary 를 붙여주면 해당 구현체가 우선순위를 가집니다.
* 단, Qualifier가 더 우선순위가 높음

#### @NoArgsConstructor

* 파라미터가 없는 기본 생성자를 생성해줍니다.

#### @AllArgsConstructor

* 모든 필드 값을 파라미터로 받는 생성자를 만들어줍니다.

#### @RequiredArgsConstructor

* `final` 이나 `@NonNull` 인 필드 값을 파라미터로 받는 생성자를 만들어줍니다.

#### @Tranactional

* transaction `begin`, `commit`을 자동으로 수행해줍니다.

* 한 작업 내에 어떤 작업에서 예외가 발생하면 `rollback`(all or nothing) 처리를 자동으루 수행해줍니다.

* 사실 해당 어노테이션을 사용할 때 헷갈린 점이 많이 있습니다. 그 문제는 [여기][https://mommoo.tistory.com/92]서 잘 다뤄주고 있으니 해당 부분을 참고해주시면 될 거 같습니다. (참고로
  update 쿼리는 레포에서 찾을때 발생합니다)
  
* ```java
  @Autowired
  private PlatformTransactionManager transactionManager;
  
  @Autowired
  private Buyer buyer
  
  @Autowired
  private Seller seller;
  
  public void buy() {
  
    DefaultTransactionDefinition def = new DefaultTransactionDefinition();
    TransactionStatus status = transactionManager.getTransaction(def);
  
    try {
        buyer.send();			
        seller.receive();
        
        transactionManager.commit(status);
  
    } catch(Exception e) {
  
        transactionManager.rollback(status);
  
    } 
  }
  
  // 같은 의미이다.
  
  @Autowired
  private Buyer buyer
  
  @Autowired
  private Seller seller;
  
  @Transactional
  public void buy() {
  
    buyer.send();			
    seller.receive();
  
  }
  ```
  
  

#### @EnableWebSecurity

* SpringSecurityFilterChain이 자동으로 포함됨
* 스프링시큐리티 사용을 위한 어노테이션

#### @Value (org.springframework.beans.factory.annotation)

* 공통 값들을 정의해 놓은 파일에 접근하여 원하는 데이터를 읽어와 사용한다고 생각하면 됩니다.
* @Value에서 사용할 값은 application.yml 파일에서 정의 할 수 있습니다.
* 편하게 주입받을 수 있다는 장점이 있습니다.
* 이걸 lombok 의 value 인줄 알고 1시간동안 헤맸습니다 ㅠㅠ



### @Value(lombok)

* 클래스 레벨에 @Value 어노테이션 선언시 사용할 수 있다.

* ```java
  @Value
  public class ValueExample {
    String name;
    String email;
  }
  ```

* 이렇게 되면 기본적으로 필드가 `private` 와 `final` 이 붙은 상수가 된다.

* `@Data` 어노테이션 처럼 `eqauls, hashCode, toString` 을 함께 만들어 준다.

* 단 final이 붙은거처럼 setter는 만들어주지 않는다.

### @RequestBody

* JSON -> Java Object
* JSON 타입으로 변환하기 위한 객체(DTO)에 getter 와 setter 메서드가 존재해야 한다

### @SuppressWarnings

* 컴파일러가 일반적으로 경고하는 내용 중 "이건 경고 하지 않아도 될 거 같아" 라고 판단할 때 쓰는거
  * 대표적으로 Intellij 의 `Problems` 에 아무것도 뜨지 않게 사용할 수 있다. (`all` 사용시)
* 대표적인 예시로는 아래와 같다.
  * `all` : 모든 경고를 억제
  * `deprecation` : 사용하지 말아야 할 메소드 관련 경고 억제
  * `unchecked` : 검증되지 않은 연산자 관련 경고 억제
  * `unused` : 사용하지 않는 코드 관련 경고 억제 



### @ResponseBody

* 서버에서 클라이언트로 응답 데이터를 전송하기 위해서 사용하는 역할
* 자바 객체를 `HTTP` 응답 본문의 객체로 변환
* `Json` 혹은 `XML` return이 본



### @ExceptionHandler

* `@Controller, @RestController` 가 적용된 `Bean` 내에서 발생하는 예외를 잡아 하나의 메소드에서 처리해주는 기능

* 사용법

  ```java
  @RestController
  public class MyRestController {
    ...
    ...
    @ExceptionHandler(NullPointerException.class)
    public Object nullex(Exception e) {
      System.err.println(e.getClass());
      return "myService";
    } 
  }
  ```

  * 위와 같이 `@ExceptionHandler` 라는 어노테이션을쓰고 인자로 잡고 싶은 예외 클래스를 등록해주면 된다.
  * 2개 이상의 예외도 잡을 수 있다.
    * `ExceptionHandler({Exception1.class, Exception2.class})` 이런식으로 두 개 이상 등록도 가능하다.
  * 위의 예시로는 `MyRestController` 에 해당하는 Bean에서 `NullPointerException` 이 발생하게 되면 `nullex` 메소드가 호출되게 된다.

* 주의사항

  * `Controller` , `RestController` 에만 적용이 가능하다.(`@Service` 같은 빈에서는 되지 않는다.)
  * 극단적인 예시로 모든 예외를 처리하고 싶다면 `ExceptionHandler(Exception.class)` 를 사용하면 된다.
  * `return` 타입은 자유롭게 해도 된다.
  * `ExceptionHandler` 를 등록한 `Controller` 에만 적용된다. 다른 `Controller` 에서 `NullPointerException` 이 발생하더라도 예외를 처리 할 수 없다.
    * 즉, `MyRestController` 에만 적용이 가능하다. 

### @ControllerAdvice

* `ExceptionHandler` 가 하나의 클래스에 대한 것이라면, `@ControllerAdvice` 는 모든 `@Controller` 즉, 전역에서 발생할 수 있는 예외를 잡아 처리해주는 `annotation` 이다.

* `RestControllerAdvice` 의 구현체를 보면 아래와 같다.

* ```java
  @Target(ElementType.TYPE)
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @ControllerAdvice
  @ResponseBody
  public @interface RestControllerAdvice {
    //...
  }
  ```

  * 즉, `viewResolver` 를 통해 예외처리페이지로 리다이렉트 시켜주려면 `@ConrollerAdvice` 만 사용해도 되고, API 서버여서 에러 응답으로 객체를 리턴해야한다면 `@ResponseBody` 어노테이션이 추가된 `@RestControllerAdivce`를 통해 객체를 리턴할 수 도 있다.

* ```java
  @RestControllerAdvice
  public class MyAdvice {
  	@ExceptionHandler(CustomException.class)
    public String custom(){
      return "hello custom";
    }
  }
  ```

* `@RestController` 에서 예외가 발생하든 `@Controller` 에서 예외가 발생하든 `@ControllerAdvice +@ExceptionHandler` 조합으로 다 잡을 수 있고 `@ResponseBody` 의 필요 여부에 따라 적용하면 된다.

* 또한 전역의 예외가 아니라 패키지 단위로 이를 제한할 수 있다.

* ```java
  @RestControllerAdivce("com.example.demo.login.controller")
  ```

  * 이렇게 하면 `login` 모듈에 있는 `RestController` 에서 발생하는 예외를 잡을 수 있다.

### @Nullable

* ?

### @NonNull 

* ?



### @Data

* `@Getter`

* `@Setter`

* `@RequiredArgsConstructor`

* `@ToString`

* `@EqualsAndHashCode`

  위의 어노테이션을 한꺼번에 설정 해주는 어노테이션이다.

### @EntityListers

* `JPA Entity` 에서 이벤트가 발생할 때마다 특정 로직을 실행시킬 수 있는 방식



### @CreatedDate

* `Entity` 가 생성되어 저장될 때 시간이 자동으로 저장된다.



### @LastModifiedDate

* 조회한 `Entity` 의 값을 변경할 때 시간이 자동 저장
* 만약 `Entity` 등록시 수정일은 null 로 두고 싶다면?
  * 스프링 부트 설정 클래스에 `@EnableJpaAuditing(modifyOnCreate = false)` 옵션 추가



### @EntityListenners(AuditingEntityListener.class)

* 해당 클래스에 Auditing 기능을 포함
* 이벤트에 반응하는 클래스 임을 알리기 위한 설정에 대한 어노테이션



### @ WebMvcTest

* 서블릿 컨테이너 모킹위해 사용하는 것
* `@Controller` , `@RestController` 가 설정된 클래스를 찾아 메모리에 생성해준다.
* 서블릿 컨테이너를 모킹한 `MockMvc` 타입의 객체를 목업하여 컨트롤러에 대한 테스트 코드를 작성할 수 있다.



### @AutoConfigureMockMvc

* 서블릿 컨테이너 모킹 위해 사용하는 것
* `MockMvc` 함께 사용 하면 별다른 설정 없이 간편하게 테스트를 진행 할 수 있다.
  *  즉, `MockMvc` 를 안쓸거면 필요없는 어노테이션이다.



### @EnableWebMvc

* `@Enable...` 로 시작하는 어노테이션은 자바 설정에서 편의를 제공하기 위해 도입되었다.
* 해당 어노테이션을 사용하면 `Spring Framework` 에서 설정해줘야 하는 여러 Config 를 알아서 세팅해준다.



### @Configuration

* 설정파일을 만들기 위한 어노테이션 또는 `Bean` 을 등록하기 위한 어노테이션이다.
  * `xml` 을 사용하여 만들 수도 있지만 자바 클래스를 사용하면 자동완성, 오타등을 쉽게 발견할 수 있기 때문이다.
* 해당 어노테이션을 클래스 레벨로 설정시 1개 이상의 `Bean` 을 등록하고 있음을 나타낸다.



### @JoinColumn

* 외래키를 매핑할 때 사용한다.

* 참고

  * `@JoinColumn` 생략

    해당 어노테이션을 생략하면 외래키를 찾을 때 기본전략을 사용한다.

    ```java
    @ManyToOne
    private Team team;
    ```

    * 기본전략 : `필드명 + _ + 참조하는 테이블의 컬럼명`
    * `team(필드명)_TEAM_ID(참조하는 테이블의 컬럼명)`

### @Column

* 객체 필드를 테이블 컬럼을 매핑한다.



### @idClass

* 복합키를 사용하기 위해 쓴다.

### @GeneratedValue

* hello

### @SpringBootTest

* `full application config` 를 로드해서 통합 테스트를 진행하기 위한 어노테이션
* 설정해 놓은 `config, context, components` 를 모두 로드한다.
* `DataSource bean` 을 그대로 사용하기 때문에 `in-memory`, 로컬, 외부 상관 없이  DB를 사용해서 테스트가 실행된다.
* 테스트 할 때마다 DB가 롤백 되지 않기 때문에  `@Transactional` 을 추가로 달아주어야한다.



### @Embedded

* `Entity` 내에서 새로운 객체를 정의해서 사용하고자 할때 사용해야하는 어노테이션
* 값 타입을 사용하는 곳에 표시
* `@Embedded` 혹은 `@Embeddable` 중 하나만 사용하면 된다.



### @Embeddable

* `Entity` 내에서 새로운 객체를 정의해서 사용하고자 할때 사용해야하는 어노테이션
* 값 타입을 정의하는 곳에 표시
* `@Embedded` 혹은 `@Embeddable` 중 하나만 사용하면 된다.

### @ElementCollection

* hello



### @PostConstruct

* `WAS` 가 띄워질 때 실행된다.
* 빈들이 띄워지고 실행된다.
* 객체가 생성된 후 별도의 초기화 작업을 위해 실행하는 메소드이다.

### @PreConstruct

* 객체 생성 전에 실행해야할 메소드 앞에 붙인다.



### @CollectionTable

* Good

### @DataJpaTest

* 실제 데이터베이스를 사용하지 않고 인메모리 데이터 베이스를 사용해서 테스트함
  * 더욱 빠르게 테스트를 해볼 수 있다.
* 오직 JPA 컴포넌트들만 테스트하기 위한 어노테이션
* 아래처럼 사용하면 실제 DB로 테스트 해볼 수 있다.

```java
@DataJpaTest
@AutoConfigureTestDatabase(replace = Replace.NONE)
```

### @AttributeOverrides

* 임베디드 타입에 정의한 매핑정보를 재정의 하기 위해 사용한다.

  ```java
  @Entity
  public class Member {
    
    @Id
    @GeneratedValue
    private Long id;
    
    private String name;
    
    @Embedded
    Address homeAddress;
    
    @Embedded
    @AttributeOverrides({
      @AttributeOverride(name="city", column=@Column(name
                                                    = "COMPANY_CITY")),
      @AttributeOverride(name="street", column=@Column(name
                                                     = "COMPANY_STREET")),
      @AttributeOverride(name="zipcode", column=@Column(name
                                                       = "COMPANY_ZIPCODE"))
    })
    Address companyAddress;
  }
  ```



## Annotation은 아니지만 알아두면 좋은것



* ### ResponseEntity

  * 기본적인 Object 만 반환하는것이 아닌 상태코드, 응답 메세지 등을 포함하기 위해선 `ResponseEntity` 를 사용해야한다.
  * 사용방법 : `ResponseEntity<반환할 타입>`

  



## 꿀팁

* ### 버전확인

  * 스프링에서 어떤 `dependency` 를 받을때 해당 스프링에 맞는 버전을 받기 위해선 가끔 버전을 확인해줘야 할때가 있다 그럴때는

  ```markdown
  spring.io -> Projects -> Spring boot -> learn -> 나에게 맞는 버전의 Refence Doc -> Dependency Version 을 들어가면 된다.
  ```

* ## 스프링의 코드를 삽입 방법은 크게 2가지 방법이 있다.

  * 바이트코드 생성(CGLIB 사용)
  * 프록시 객체 사용
    * 2가지 방법중 Spring 은 기본적으로 프록시 객체 사용이 선택된다. 그렇기에 interface가 반드시 필요하다.
    * 하지만 SpringBoot 는 기본적으로 바이트코드가 생성된다. 따라서 굳이 interface가 필요 없다.

* ### `gradle` 통해 의존 관계 나오는 명령어

  ```shell
  ./gradlew dependencies --configuration compileclasspath
  ```

* ### 생성자보다는  빌더패턴을 사용하자

  ```java
  return new ResponseEntity<someDto>(someDto, headers, HttpStatus.valueOf(200));
  
  return ResponseEntity.ok()
    		 .headers(headers)
    		 .body(someDto);
  ```

  
  
* ### JPA에서 엔티티를 저장할 때 연관된 모든 엔티티는 영속 상태여야 한다. 

* ### 왜 Entity는 그대로 노출되어선 안되고  Dto로 변환되고 반환되어야 하는가?

  * 특히 API의 경우 만약 Entity 클래스가 수정되었다.(필드명이든지) 

  * 이럴경우 client 딴에서는 이를 사용하는데 큰 무리가 생긴다! 하지만 Dto를 사용한다면 dto는 바꿔주지 않고 사용할 수 있기때문에 이를 방지할 수 있다.

  * 또한 내부 설계를 걸리는 꼴이 되기 때문에 보안에 취약하다.

  * 그런데 이 경우 코드가 DTO로 한번 변환 해주어야 하기 때문에 코드의 길이가 길어진다는 문제점이 있다. 이를 방지하기 위해선 DTO 안에 Entity로 변환해주는 형태를 만들어주면 이를 해결할 수 있다.

    ```java
    public class MemberDto {
      
      private Long id;
      private String userName;
      private String teamName;
      
      public MemberDto(Long id, String userName, String teamName){
        this.id = id;
        this.userName = userName;
        this.teamName = teamName;
      }
      
      //여기가 말하고자 하는 부분!
      public MemberDto(Member member){
        this.id = member.getId();
        this.userName = member.getUserName();
      }
    }
    
    
    //이렇게 하고 그냥
    public Page<MemberDto> list(Pageable pagealbe) {
      return memberRepository.findAll(pageable).map(member -> new MemberDto(member)); //이를 메소드 레퍼런스로 바꿀 수도 있음
    }
    ```

    

## 사전지식

* ### Mock

  * 테스트를 위해 만든 모형

* ### Mocking

  * 테스트를 위해 실제 객체와 비슷한 모의 객체를 만드는 것

* ### Mock-up

  * Mocking 한 객체를 메리에서 얻어내는 과정

* ### AOP

  * 회원가입 하는 로직에 가서 시간측정을 남겨 줄 수 있어요?
  * 모든 Service에 이런 로직을 남겨주세요
