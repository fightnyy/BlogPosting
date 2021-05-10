GC는 언제 발생할까

- \- 새로운 객체들이 eden 영역이 할당이 되고 더이상 할당할 수 없을 경우 **Minor GC**가 발생한다.

  \- Mark and Sweep 알고리즘에 따라 Reachable 객체와 Unreachable 객체를 식별한다.(Mark)

  \- eden 영역의 Unreachable 객체는 제거된다.(Sweep)

  \- Reachable 객체는 Survivor 영역으로 이동하고 Age 값이 증가한다. 이때 Survivor 영역은 둘 중 한 곳에 객체가 저장되면 나머지 한 곳은 반드시 비어 있는 상태여야 한다.

  \- Minor GC가 발생할 때마다 Survivor 영역이 바뀐다.

  \- 위과 같은 Minor GC 과정이 반복되면서 Age 값이 특정 임계점에 도달한 객체는 Old Generation 영역으로 이동한다.

  \- Old Generation 영역이 가득 차면 **Major GC(또는 Full GC)**가 발생한다.

- 단편화





static 영역은 어디로 갔는지

원래는 permGen영역 



클래스 로더

static은 GC의 대상이 되지 않는다.



CMS



parallel



System.GC()를 사용자가 쓰면 안되는 이유

- 쓰레드가 다 멈춤
- 판단은 GC가 하는데 무의미한 코드가 될 수 있다.



Finalize

try resource 



connection

statement

resultset



JDBC 할때 왜 역순으로 열어주고 닫아줄까?

마지막만 닫아줘도 되는데 왜 명시적으로 다 닫아줄까?
왜냐하면 그 이전에 에러 터지면 어디서 부터 닫아줘야할지도 모르고   



G1Gc의 영역 일반적이 GC heap영역과 다른지

- heap영역을 고정된 크기로 분할함
- 좋은점: 한 region영역안에 gc에 대상이 되는 많은것 부터 gc처리를 한다.
- re

































* Epic
  - 대략적인 기능 정의, 세부 사항은 스토리로 분류
  - 