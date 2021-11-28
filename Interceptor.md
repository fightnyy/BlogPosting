# Interceptor



## 개념

* 사용자의 요청이 컨트롤러에게 가기전에 가로채거나 서버의 응답이 사용자에게 가기전에 가로챈다.

  * `DispatcherServelt` 이 컨트롤러에 요청하기 전,후에 요청( `HttpRequest`),응답( `HttpResponse`) 객체를 가로채어 가공한다.

* 인터셉터는 `HandlerInterceptor` 인터페이스로 구현하거나 `HandlerInterceptorAdapter` 클래스를 상속받는 방법이 있다.

  ```java
  public class MyInterceptor implements HandlerInterceptor{
  
      // preHandle() 메소드
      // controller가 요청되기 전에 호출됨
      @Override
      public boolean preHandle(// 매개변수 Object는 핸들러 정보를 의미한다.
              HttpServletRequest request, HttpServletResponse response,
              Object obj) throws Exception {
          System.out.println("MyInterCeptor - preHandle");
          return false; // 반환이 false라면 controller로 요청을 안함
      }
  
      // postHandel() 메소드
      // controller의 handler가 끝나면 처리됨
      @Override
      public void postHandle(
              HttpServletRequest request, HttpServletResponse response,
              Object obj, ModelAndView mav)
              throws Exception {
      }
  
      // afterCompletion() 메소드
      // view까지 처리가 끝난 후에 처리됨
      @Override
      public void afterCompletion(
              HttpServletRequest request, HttpServletResponse response,
              Object obj, Exception e)
              throws Exception {
      }
  }
  ```

  1. `preHandle()` 메소드는 컨트롤러가 호출되기 전에 실행됩니다.
  2. `postHandle()` 메소드는 컨트롤러가 실행된 후 실행됩니다.
  3. `afterComplete()` 메소드는 뷰의 실행까지 마치고 실행도비니다.

## 비교

* `Filter` 와 `Interceptor` 의 차이
  * 호출 시점
    * `Filter` 는 `DispatcherServlet` 이 실행되기 전, `Interceptor` 는 `DispatcherServlet` 이 실행된 후