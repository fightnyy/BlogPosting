# JS 모르던 것

## 1. `setTimeout()`

* 사용법 

  * ```javascript
    // function 은 타이머가 만료된 뒤 실행할 function
    // delay는 함수 또는 코드를 실행하기전에 기다릴 ms
    setTimeout(function, delay);
    
    ```

  * 

* `setTimeout` 은 비동기 함수로, 이 함수를 사용해서 다음 함수 호출을 "일시정지" 할 수 는 없다.

  ```javascript
  setTimeout(()=> {
    console.log("첫 번쨰 실행")
  }, 5000);
  
  setTimeout(()=> {
    console.log("두 번째 실행")
  }, 3000);
  
  setTimeout(()=> {
    console.log("세 번쨰 실행")
  }, 1000);
  
  
  // 이렇게 하면 3번째 실행 => 2번쨰 실행 => 1번째 실행 순으로 나온다.
  ```

* 

