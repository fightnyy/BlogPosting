# callback 함수

* ## 정의

  * 함수에 파라미터로 들어가는 함수

* ## 용도

  * 순차적으로 실행하고 싶을때 사용

* ## 참고

  * 다른데서 만든 함수도 사용할 수 있음

  * 콜백함수에 함수명 작명할 수도 있음(하지만 굳이......)

  * 콜백함수가 필요한 함수들에만 콜백함수를 사용할 수 있다.

    * ```javascript
      document.querySelector('.button').addEventListener('click', function(){
        
      })
      
      //-----------
      setTimeout(function() {
        
      }, 1000)
      ```

* ## 실행 예시

  * ```javascript
    function first(함수){
      conosle.log(1)
      함수()
    }
    
    function second(){
      console.log(2)
    }
    
    first(second)
    ```

