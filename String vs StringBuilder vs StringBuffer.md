# String vs StringBuilder vs StringBuffer

* 연산이 많지 않을 때는 어떤걸 쓰도라도 이슈가 발생하지 않는다.
* 그러나 아래와 같은 상황에서는 많이 상황에 맞는 적절한 클래스를 사용해주어야 한다.
  * 연산횟수가 많다.
  * 멀티쓰레드 환경이다.
  * Race Condition 이다.



## 차이

* String은 `불변(immutable)`이지만 StringBuffer와 StringBuilder 는 `가변` 적이다.
 ![img](https://t1.daumcdn.net/cfile/tistory/99948B355E2F13350F)
  
* 위 처럼 오해할 수 있다. 하지만 hello world 는 다른 인스턴스인 것이다.

  * 따라서 변하지 않는 문자열을 자주 읽어들이는 경우 `String` 을 사용하면 좋다.
  * 문자열 추가,수정,삭제 등의 연산이 빈번하게 발생하는 알고리즘에 `String` 을 사용하면 `Heap` 에 많은 `Garbage` 가 생겨 힙메모리 부족으로 좋지 않은 영향을 미친다.
  
* 따라서 이를 해결하기 위해 `StringBuffer` , `StringBuilder` 클래스가 나왔다.

 ![img](https://t1.daumcdn.net/cfile/tistory/9923A9505E2F133608)

   * `StringBuffer` 와 `StringBuilder` 는 `append()`, `delete()` 등의 API 를 이용해서 동일 객채내에서 문자열을 변경하는게 가능하다.
      * 따라서 문자열을 자주 바꾸는 경우 해당 클래스를 사용하는것이 바람직하다.

* `StringBuffer` 와  `StringBuilder` 차이점은 **동기화의 유뮤** 이다.
  * `StringBuffer` 는 동기화 키워드를 지원해 **멀티쓰레드 환경에서 안전하다**
    * 참고로 `String` 도 **불변성** 을 가지기 때문에 멀티쓰레드 환경에서 **안정성** 을 가지고 있다.
  * `StringBuilder` 는 동기화를 지원하지 않기 때문에 **멀티쓰레드 환경에서는 적합하지 않다.**
    * 단, 싱글쓰레드인의 성능은 `StringBuffer` 보다 뛰어나다.

### 결론

`String` :  문자열 연산이 적고 멀티쓰레드 환경일 경우

`StringBuffer` : 문자열 연산이 많고 멀티쓰레드 환경일 경우

`StringBuilder` : 문자열 연산이 많고 단일쓰레드이거나 동기화를 고려하지 않아도 된는경우