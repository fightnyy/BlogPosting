#### Redis

* REmote DIctionary Server(Redis)
* 연산 결과를 메모리에 저장하여 캐시용도(In-Memory 방식)로 사용
    * 혹은 가벼운 DB storage 처럼 사용

#### Cache

* 결과를 미리 저장해두었다가 빠르게 서비스를 해주는 것을 의미한다
* Factorial을 DP로 구성할 때
    * 20881!은 20880!를 계산해 두고 저장해두었다면 계산은 금방할 수 있다

![image-20210515152921259](/Users/nayeong-yun/Library/Application Support/typora-user-images/image-20210515152921259.png)

![image-20210515153052205](/Users/nayeong-yun/Library/Application Support/typora-user-images/image-20210515153052205.png)

그렇다면 아래와 같은 자바 선언도 in-memory 인데 왜 사용하지 않을까요?

```java
private final Map<String, Object> Store = new HashMap<>();
```

* 서버가 여러대인 경우 Consitency의 문제가 발생할 수 있습니다
* Multi-Threaded 환경에서 Race condition이 발생할 수 있습니다
    * Race-condition 이란 여거 개의 Thread가 경합하는 것을 의미합니다
    * Context Switching에 따라 원하지 않는 결과가 발생할 수 있습니다

* 이때 Redis는 Race Condition을 해결합니다
    * 기본적으로 Single Threaded 방식이며
    * Atomic Critical Section에 대한 동기화를 제공합니다
    * 서로 다른 Transaction Read/Wirte를 동기화 할 수 있습니다

#### Redis가 memcached 보다 좋은 이유

* Collection이 가능함, memcache 도 가능하지만 C와 파이썬의 차이라고 이해하면 될듯
    * 개발의 편의성 증가
    * 개발의 난이도 감소

#### Redis의 특징

* 엄청 빠르다
    * disk 보다 대략 10배 정도 빠르다
* Data Expire 가능
    * 지정된 시간 후에 만료 가능 : 3시간 후 만료
    * 만료 시간 지정 가능 : yyy/MM/dd hh:mm:ss 이후에 만료

#### Redis를 사용하는 이유

1. 예를들어 랭킹을 매기는 서버를 개발한다고 할때 가장 간단한 방법으로는

> DB에 유저의 Score를 저장하고 Score로 order by로 정렬 하고 읽어오는 방법

이 있다. 하지만 __개수__ 가 많아지면 속도에 문제가 발생할 수 있다. => 결국은 디스크를 사용하는것이므로

__하지만__ Redis를 사용하면 __Redis가 제공하는 Sorted Set__을 이용하면, 랭킹을 구현할 수가 있다.

또한, Replication도 가능하다. 다만 가져다 쓰면 한계에 종속적일 수 있다. 랭킹에 저장해야할 id가 1개당 100byte라고 했을 때

10명 = 1K

10000명 = 1M

이런식으로

2. 개발의 난이도 감소하는 부분

* Redis의 경우는 자료구조가 Atomic 하기 때문에, Race Condition을 피할 수 있다. 그래도 잘못짜면 안된다고 합니다

#### 왜 Collection이 중요한가

* 외부의 Collection을 잘 이용하는 것으로, 여러가지 개발 시간을 단축 시키고 문제를 줄여줄 수 있기 때문에 Collection이 중요

#### Redis의 사용처

* Remote Data Store
    * A서버, B서버, C서버에서 데이터를 공유하고 싶을때, 즉 여러 서버에서 같은 데이터를 공유할 때
* 한대에서만 필요하다면, 전역 변수를 쓰면 되지 않을까?
    * Redis 자체가 Atomic을 보장해준다.(싱글 스레드이기 때문)
    * Atomic 자료구조와 Cache로 인한 빠른 속도로 접근 가능
* 주로 많이 쓰는 곳들
    * 인증 토큰 등을 저장(String 또는 hash)
    * Ranking 보드로 사용(Sorted Set)
    * 유저 API Limit
    * 잡 큐(list)

#### Redis Collections

* __String__ => Key, Value 형태로 저장하는 형태
* List => 처음과 마지막에 정보를 변경(삭제, 추가, 수정) 할 때는 사용하지만 정보가 중간에 있다면 사용하면 안됨
* Set => 중복된 데이터를 담지 않기 위해서 사용함, 순서가 없음
* __Sorted Set__ => 순서있는 Set
* Hash

__Bold__ 표시는 Redis 에서 가장 많이 쓰는 자료구조를 나타냅니다

#### Sorted Sets

* Score는 double 타입이기 때문에, 값이 정확하지 않을 수 있다. 컴퓨터에서는 실수가 표현할 수 없는 정수값들이 존재한다

#### Collections 주의사항

* 하나의 컬렉션에 너무 낳은 아이템을 담으면 좋지 않다
    * 10000개 이하 몇천개 수준으로 유지하는게 좋다.
* Expire는 Collection의 Item 개별로 걸리지 않고 전체 Collection 에 대해서만 걸린다
    * 즉 해당 10000개의 아이템을 가진 Collection에 expire가 걸려있다면 그 시간 후에 10000개의 아이템이 모두 삭제 된다.

### Redis 운영

* 메모리 관리를 잘하자
* O(N) 관련 명령어는 주의하자
* Replication
* 권장 설정 Tip

#### 메모리 관리를 잘하자

* In-Memory Data Store 니까 관리를 진짜 잘해야 한다.
* Physical Memory 이상을 사용하면 문제가 발생한다
    * Swap이 있다면 Swap 사용으로 해당 메모리 Page 접근시마다 늦어진다
    * Swap이 없다면? => OOM 으로 죽을수 있음
* Maxmemory를 설정하더라도 이보다 더 사용할 가능성이 큼
    * JMalloc을 사용하는데 예를들어 메모리 페이지 사이즈가 4096byte라고 가정하면 1byte의 요청만 하더라고 4096byte가 나간다 이때 4096을 추가로 달라고 하면 Redis는 4097을
      사용한다고 하지만 실제로는 8192byte가 나갈 수 도 있는것이다.
* RSS 값을 모니터링 해야함
* 큰 메모리를 사용하는 instance 하나보다는 적은 메모리를 사용하는 instance 여러개가 안전하다.(Read 할때는 괜찮지만 Write 할때 fork를 해서 예를들어 8GB 사용하면 16GB가 되지만
  32GB를 사용하면 64GB로 되게됨)
* Redis 는 메모리 파편화가 발생할 수 있다. 4.x 대부터 메모리 파편화를 줄이도록 jemalloc에 힌트를 주는 기능이 들어갔으나, jemalloc 버전에 따라서 다르고 동작 가능
* 다양한 사이즈를 가지는 데이터 보다는 유사한 크기의 데이터를 가지는 경우가 관리하는데 유리하다.

#### 메모리가 부족할 때는?

* Cache is cash!!
    * 좀 더 메모리 많은 장비로 Migration
    * 메모리가 빡빡하면 Migration 중에 문제가 발생할 수도
* 있는 데이터 줄이기
    * 데이터를 일정 수준에서만 사용하도록 특정 데이터를 줄임
    * 다만 이미 Swap을 사용중이라면, 프로세스를 재시작 해야함

### O(N) 관련 명령어는 주의하자

* Redis는 Single Threaded
    * 한번에 하나의 명령만 수행이 가능합니다
    * 그러면 Redis가 동시에 여러개의 명령을 처리 할 수 있을까? => 못합니다
    * 따라서 긴 시간이 필요한 명령을 수행하면? => 안됩니다
    * 해당 명령어들은 상당히 오래걸리기 때문에 주의해서 사용합니다
        * KEYS
        * FLUSHALL, FLUSHDB
        * Delete Collections
        * Get All Collections

### 결론

* 기본적으로 Redis는 매우 좋은 툴
* 그러나 메모리를 빡빡하게 쓸 경우, 관리하기가 어려움
    * 32GB 장비라면 24GB 이상 사용하면 장비 증성을 고려
* Cache 일 경우는 문제가 적게 발생
    * Redis 가 문제가 있을 때 DB 등의 부하가 어느정도 증가하는지 확인이 필요
    * Consistent Hashing도 실제 부하를 아주 균등하게 나누지는 않음. Adaptive Consitent Hashing을 이용해 볼 수 있음
        * 정기적인 migration이 필요
        * 가능하면 자동화 툴을 만들어서 이용
    * RDB/AOF 가 필요하다면 Secondary에서만 구동

#### 추가적으로 알아보면 좋을 것들

* Redis를 저장소처럼 -> Redis Persistent, RDB, AOF
* Redis의 메모리는 제한되어있기 때문에 주기적으로 Scale out, Back up을 해야함 -> Redis Cluster
* 부하 분산 -> Constant Hashing
* Data Grid -> Spring Gemfire, Hazlecast