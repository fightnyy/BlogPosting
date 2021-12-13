# Hikari 란?



JDBC를 사용해서 java 어플리케이션과 DB가 연동되는 순서는 아래와 같다.

1. DB벤더에 맞는 드라이버 로드
2. DB서버의 IP, ID, PW 등을 DriverManager 클래스의 getConnection 메소드를 사용하여 Connection 객체 생성
3. Connection 으로 부터 PreparedStatement 객체를 받음
4. executeQuery를 수행하고 ResultSet 객체를 받아 데이터를 처리
5. 사용했던 ResultSet, PreparedStatement, Connection 을 처리

이 과정중 가장 오래걸리는 부분은 Connection객체를 얻는 2번 과정이다. 

> DB 서버와 애플리케이션 사이의 통신이기에 같은 장비에 둘다 존재하더라도 시간이 오래 걸린다.

* 만일 사용자가 급증하면 서버 환경에선 반복적으로 Connection 객체를 얻기 위해 엄청난 시간을 소모할 것이다. 
* 하지만 이때 **필요한 양만큼의 Connection 객체를 미리 얻어놓는다면** Connection 객체를 생성하는 부분에서 발생하는 대기 시간을 줄이고 네트워크의 부담을 줄 일 수 있다. 
* 이때 등장한 개념이 DB Connection Pool 이다.

![img](https://blog.kakaocdn.net/dn/p4AxG/btqSASB6bD8/8sowoSSZ1DolazOxCPDdbk/img.png)





## DB Connection pool 이란?

* 사용자의 요청에 따라 무수한 Connection 을 생성하면 서버에 과부하가 걸리게 된다.

* 이를 막기 위해 **미리 설정해놓은 일정수의 Connection 을 만들어 놓고 이것을 Connection Pool에 보관해두었다가 요청이 발생하면 제공해주고 사용이 끝났으면 다시 Connection Pool 에 반환하여 보관하는 기술** 

* 장점

  * DB 접속 설정 객체를 미리 만들어 연결하여 메모리상에 등록해 놓기 때문에 클라이언트가 빠르게 DB에 접속 할 수 있다.
  * DB Connection 수를 제한 할 수 있어 과도한 접속으로 인한 서버 자원 고갈 방지가 가능하다. 
  * DB 접속 모듈을 공통화해 DB 서버의 환경이 바뀔 경우 쉬운 유지보수가 가능하다.
  * 연결이 끝난 Connection 을 재사용 함으로써 새로 객체를 만드는 비용을 줄 일 수 있다.

* DataSource 와 DB Connection Pool의 차이

  > DataSource는 JDK 1.4부터 생긴 표준이다. DataSource는 Connection Pool로 연결을 관리해야 하고, 트랜잭션 관리도 가능하게 만들어야 한다. 따라서 DataSource 가 DB Connection Pool을 포함한다고 생각하면 된다.
  >
  > DataSource 는 자바 표준이 있지만 Connection Pool 은 Java 표준이 없기 때문에 WAS에 따라 사용법이 상이할 수 있다. 단, DataSource 는 자바 표준이 있어 WAS 에 상관없이 사용법이 동일하다.



## HikariCP 란?

* SpringBoot 2.0 부터 default로 설정되어 있는 DB Connection Pool 로써 Zero - Overhead가 특징으로 높은 성능을 자랑하는 DB Connection pool이다.
* 미리 정해 놓은 만큼의 Connection 을 Connection Pool 에 담아 놓는다. 그 후 요청이 들어오면 Thread가 Connection 을 요청하고, Hikari는 Connection Pool내에 있는 Connection을 연결해주는 역할을 한다.
* SpringBoot 환경에서는 `application.properties` 에서 간단하게 Hikari CP 의 설정을 할 수 있다.