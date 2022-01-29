# Spring Cloud Sleuth(1)

# 소개

* Spring Cloud Sleuth 는 Spring Cloud 를 위한 분산 추적을 구현한 솔루션입니다.



## 용어

* Spring Cloud Sleuth 는 구글에서 개발한 Dapper 의 용어를 빌려 사용합니다.
* `Span` 
  * 일의 가장 기본적인 단위입니다.
  * 예를 들어 RPC에 응답을 보내는 것처럼 RPC 를 보내는 것은 새로운 Span 입니다.
  * 스팬은 고유한 64비트 ID 로 식별됩니다. 
  * 또한 Trace(스팬의 총 묶음)라는 고유한 64비트 아이디가 있습니다.
  * 스팬은 설명, 타입 스탬프 이벤트, 키-값 주소, 그리고 스팬 ID, Process ID(주로, IP 주소) 등이 있습니다.
  * 스팬은 시작하고 멈출 수 있으며 타이밍 정소를 추적할 수 있습니다. 일단, 스팬을 만들었다면 어느시점에 무조건 멈춰야 합니다.
  * Trace 를 시작한 초기 스팬을 `root span` 이라고 합니다. `root span` 의 ID 는 `trace` 의 ID 와 일치합니다.



* `Trace`
  * Tree와 같은 구조로 형성된 Span의 세트입니다.

* `Annotation`

  * 이벤트를 기록하기 위해 사용됩니다.

  * `Brave Instrumentation` 을 사용하면 우리는 더이상 `Zipkin` 이 클라이언트와 서버가 누구인지, 요청이 어디서 시작되었는지, 어디서 종료되었는지 이해할 수 있도록 특별한 이벤트를 설정할 필요가 없습니다. 

  * 그러나 학습목적으로 이러한 이벤트를 표시하여 어떤 종류의 동작이 발생했는지 강조합니다.

    * `cs` 
      * Client Sent
      * Client 가 해당 리퀘스트를 만들었다는 의미입니다.
      * 이 주석은 Span의 시작을 의미합니다.
    * `sr`
      * Server Received
      * 서버가 request를 받고 처리하기 시작하는 것을 의미합니다.
      * `sr-cs` = `network latency` 입니다.
    * `ss`
      * Server sent
      * 요청에 대한 처리(Client의 요청에 대한 응답을 보내줌)가 완료되었다는 주석입니다.
    * `cr`
      * Client Received
      * `Span` 의 끝을 나타냅니다.
      * Client가 서버로 부터 응답을 잘 받았다는것을 나타냅니다.
      * `cr-cs = Client 가 Server 로 부터 응답을 받을 수 있는 총 필요한 시간` 을 나타냅니다.

    ![Trace Info propagation](https://raw.githubusercontent.com/spring-cloud/spring-cloud-sleuth/2.2.x/docs/src/main/asciidoc/images/trace-id.png)

    * 아래 이미지는 `Span` 과 `Trace` 가 System 에서 어떻게 보이는지에 대한 예시를 나타냅니다. (`Zipkin` 주석을 포함시킨 형태입니다.)

    * 아래 색깔은 스팬을 나타냅니다. (즉 A-G 까지 보면 스팬이 총 7개라는 것을 알 수 있습니다.)

      ```markdown
      Trace Id = X
      Span Id = D
      Client Sent
      ```

      * 위의 예시에서 `Trace Id 는 X` 이고 `Span Id 는 D` 이며 `cs` 이벤트가 발생한 것을 볼 수 있습니다. 

      ![Parent child relationship](https://raw.githubusercontent.com/spring-cloud/spring-cloud-sleuth/2.2.x/docs/src/main/asciidoc/images/parents.png)

      `parent-child` 형태 관계도로 나타내면 Span 은 위처럼 표시됩니다.