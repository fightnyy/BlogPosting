이번에 로그인을 하게 되면서 CORS란 것을 처음으로 알게 되었습니다. 그래서 이번 포스팅은 CORS가 무엇인지 알아보고자 합니다.

### 상황

* 제가 처한상황은 react와 Spring을 하나의 localhost에서 같이 올려 통신을 하고자 합니다. react의 경우 localhost:3000 인 상황이고 Spring의 경우 localhost:8080 인
  상황입니다.

### Origin

* console.log(location.origin)

### SOP

* Same origin Policy 란 뜻으로 같은 출처인 경우에만 리소스를 받을 수 있도록 하는것(보안을 위해서) CSRF, XSS 를 방지하기 위해서 라고 합니다.
* HTTP와 HTTPS를 사용하면 모두 이 정책을 사용합니다.
* 하지만 웹을 사용하다보면 다른 출처의 리소스도 사용해야할 경우가 많습니다. 따라서 예외의 조항도 필요합니다.
    * __CORS__ 정책을 지킨요청
    * 실행 가능한 스크립트<script>
    * 랜더될 이미지<image>
    * 스타일 시트<style>
    * ETC.....

### CORS

* 시나리오
    * Request 요청에 Origin Header가 있고 Response 요청에 Access-Control-Allow-Origin이란 헤더가 있습니다.
    * 이 두개가 같다면 브라우저는 같은 출처라고 판단합니다.

* Cross-Origin Resource Sharing 의 약자로서 웹 어플리케이션의 도메인이 다른 도메인의 리소스에 대해서 접근이 허용되는지 체크하는 방법이라고 합니다.

* 웹 어플리케이션은 리소스를 요청하는 서버의 도메인, 프로토콜 또는 포트가 다를경우 cross-origin HTTP request 요청을 실행합니다.

* 보안의 이유로 브라우저는 cross-origin HTTP request에 대해서 same-origin policy를 적용하여 동작합니다. 예를 들어 algopoolja.com 이라는 도메인의 클라이언트에서
  리소스를 요청할 때는 algopoolja.com 이라는 도메인의 서버일 경우에 CORS 문제가 발생하지 않고 정상적으로 동작합니다.

    * 여기서 same-origin-policy란 두 origin 간의 프로토콜, 포트, 호스트가 같아야 하는 것을 의미합니다.

    * same-origin-policy가 아닌 cross-origin의 종류에는

        * 다른 도메인(ex. `example.com` 에서 `test.com` 으로)
        * 다른 하위 도메인(ex. `example.com`에서 `store.example.com` 으로)
        * 다른 포트(ex. `example.com` 에서 `example.com:81` 로)
        * 다른 프로토콜(ex. `https://example.com` 에서 `http://example.com`으로)

    * MDN에서 보기 좋게 정리된 자료가 있습니다.

      기준 URL : http://store.company.com/dir/page.html

      | URL                                             | Outcome      | Reason               |
          | ----------------------------------------------- | ------------ | -------------------- |
      | http://store.company.com/dir2/other.html        | same origin  | path만 다른경우      |
      | http://store.company.com/dir/inner/another.html | same origin  | path만 다른경우      |
      | https://store.company.com/page.html             | cross origin | 프로토콜이 다른 경우 |
      | http://store.company.com:81/dir/page.html       | cross origin | 포트가 다른경우      |
      | http://news.company.com/dir/page.html           | cross origin | 도메인이 다른경우    |

* 하지만 두 도메인이 서로 다를 경우엔 CORS 문제가 발생하므로 이를 해결하기 위해 Header 설정을 해주어야 하며 이를 해결해야만 cross-origin Http Request에 대해서 정상적으로 요청과
  응답이 이루어 집니다.

#### Preflight Request

* CORS를 검색하면 거의 `Preflight`란 단어를 자주 보게 됩니다. 해당 단어를 번역하면 `미리 보내기`, `사전 전달` 이라고 할 수 있습니다.

* 단어의 뜻처럼 실제 요청을 보내도 안전한지 판단하기 위해 preflight요청을 보냅니다.

* Browser가 OPTIONS 메소드를 통해서 예비요청을 보냅니다.(Origin을 서버로 보내게 됩니다.)

* Origin에 대한 정보 뿐만 아니라 자신이 예비 요청이후 보낼 본 요청에 대한 다른 정보들도 같이 포함 되어 있습니다.(Access-Control-Request-Method 등)

* 요청에 Origin과 응답의 Access-Control-Allow-Origin을 브라우저가 비교해 출처를 판단하여 다르면 에러를 발생시키고 접근할 수 있는 출처람녀 본 요청을 보내 요청을 처리합니다.

* 서버 사이드 영역이 아닌 브라우저 영역이기 때문에 서버는 200대 성공 코드를 반환합니다.

    * OPTION 메소드란?

        * 목표리소스와의 통신 옵션을 설명하기 위해 사용됩니다

      . 

#### Simple Request

* 예비 요청이 없습니다. 따라서 Preflight에서 했던 예비요청을 모두 본 요청에서 처리합니다.
* 제한적인 환경에서만 사용이 가능합니다.
    * 해당 request를 사용하기 위해선 요청 메소드는 GET, HEAD, POST 중 하나여야 합니다.
    * Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더를
      사용하면 안됩니다.(JWT 때 사용할 헤더에도 못사용해서 REST API에선 사용하지 않습니다.)
    * Content-Type을 사용하는 경우에는 `application/x-www=form`

* 그 외에는 Preflight와 동일합니다.



