로그인이 이렇게 구현하기 어려웠다니 정말 웹을 구현하면서 드는 생각은 웹 개발자 분들 정말 존경합니다! 이런걸 얼마나 해오신겁니까 도대체......

![2083.jpg](http://cdn.ppomppu.co.kr/zboard/data3/2019/1021/m_20191021172247_bcianlfo.jpg)

우선 로그인을 알아보기 전에 인증(Authentication)과 인가(Authorization)가 무엇인지 알아봐야합니다.



### 인증(Authentication)

* 보호된 리소스에 접근하는것을 __허용하기 이전__에 등록된 유저의 신원을 입증(validating) 하는 과정
* 식별가능한 정보로 서비스에 등록된 유저의 신원을 입증하는 과정
  * 로그인



### 인가(Authorization)

* 요청된 리소스에 접근할 수 있는 __권한이 있는 인증(authenticated)된 유저__인지 입증(validating) 하는 과정
  * 티스토리에 글을 수정할 수 있는 권한



즉, 인증이 되고 나서 인가가 되는것이라는것을 알 수 있습니다.

따라서 인증되었지만 권한이 없다.(인가X) 는 말이 되지만

인가되었지만 인증은 되지 않았다. 라는 말은 되지 않습니다. 



그렇다면 여기서 사용자가 티스토리에서 경험하는 과정을 통하여 설명을 곁들어 보겠습니다.

1. 인증하기(로그인하기) 는 __Request Header__를 통해 이루어집니다.
2. 인증 유지하기 는  __Browser__ 를 통해 이루어집니다.
3. 안전하게 인증하기 는 __Server__ 를 통해 인증이 됩니다.
4. 효율적으로 인증하기 는 __Token__ 을 통해 인증이 됩니다.
5. 다른 채널을 통해 인증하기 는 __OAuth__ 를 통해 진행됩니다.



먼저 빠르게 OAuth를 간단하게 다뤄보겠습니다.

### OAuth

* 다른 웹사이트 상의 자신들의 정보에 대해 접근 권한을 부여 할 수 있는 공통적인 수단, 개방형 표준
  * 즉 구글로그인, 페이스북 로그인, 깃헙 로그인 등이 사용하는 인증절차를 OAuth 라고 할 수 있습니다.



### 인증 유지하기(Browser)

* REST API로 진행되는 과정을 설명해보자면 client와 Server는 __Http__를 통해 통신을 하게 됩니다.

* __Http__는 __무상태성(stateless)__라는 특징을 가지고 있습니다. 따라서 어떤 요청을 하기 위해선 매번 인증(로그인)을 해야하는 문제를 갖게 됩니다. 이때 저희는 browser(정확히 얘기하면 browser의 storage)의 힘을 빌릴 수 있습니다.

* 따라서 이 로그인 정보(id와 password를 BASE 64로 인코딩한 정보)를 browser(local storage, session storage, cookie, 여기서는 cookie로 설명드리겠습니다.)에 저장해서 매 요청마다 이 정보와 같이 사용자의 요청을 보내면 됩니다.

* 하지만 이 방법은 해커에게도 매우 편한 방법입니다. (클라이언트는 상대적으로 server에 비해 보안에 취약합니다.)

  ![img](https://blog.kakaocdn.net/dn/bUov8W/btqUuNzVp4a/hQIYkinrGQF0pWc6qKOENK/img.png)

  ![img](https://blog.kakaocdn.net/dn/JSCQE/btqUGUYs85E/8PzHk5T4FAeOm7MKUdDBc0/img.png)



### 안전하게 인증하기(Server)

* 따라서 저희는 server를 이용하고자 합니다.

* 이 방법에서는 server가 client에게 쿠키를 보내주고 끝나는 것이 아닌 서버 자체에 session이란것을 갖게 됩니다.

* 이런 방법에서의 장점은 session은 만료기간이란 것을 가질 수 있으므로 해커가 클라이언트의 세션정보를 탈취 하더라도 만료기간이 지나면 유효하지 않다라는 것을 알 수 있습니다.

* 또한 session은 server에서 관리 되기 때문에 탈취가 된 session을 서버에서 삭제하면 된다는 장점이 있습니다.

  ![img](https://blog.kakaocdn.net/dn/9yUW9/btqUvCY7TFS/zlJmTSWiVm53akGdnyPskK/img.png)

  ![img](https://blog.kakaocdn.net/dn/cC76ek/btqUtVSr7f3/5v7tY7Hn1HV61tmTs9Kf31/img.png)

* 하지만 __서비스가 너무 잘되서 여러 서버를 둘 경우__ 문제가 발생하게 됩니다. 서버를 여러개 둘 경우 로드밸런서를 통해 클라이언트는 서버에 접근을 하게 되는데 이때 로그인은 1번서버에서 하고 이후 요청은 3번서버에서 진행할 경우 회원의 session 정보가 1번서버에 있기 때문에 3번서버는 해당 요청을 수행을 옳바르게 할 수 없습니다. 

* 즉, 해당문제는 세션를 각 서버가 관리하기 때문에 발생한 문제이므로 이는 session 만 관리하는 __session storage__ 라는 것을 통해 문제를 해결 할 수 있습니다.

* __session storage__는 각 서버에서 관리하던 session들을 하나의 storage에서 관리하는 storage입니다.

* 하지만 이경우도 문제가 있습니다. 바로 클라이언트가 너무 많아져서 계속 session storage에 접근하면 결국 session storage 가 다운되는 문제점이 발생합니다.

![image](https://user-images.githubusercontent.com/55227984/117329646-d3cfb400-aecf-11eb-8312-8bf7aa685f54.png)





### 효율적으로 인증하기(Token)

* 여러 Token이 있지만 여기서는 JWT로 설명을 하겠습니다.
* JWT에 대한 설명은 [여기][https://algopoolja.tistory.com]을 참고해주시면 됩니다. 간단히 설명하자면 secret key를 활용하여 JWT를 만들어 냅니다.  따라서, secret key를 매우 소중히 서버에 관리해야합니다.(털리면 끝나요 ㅠㅠ)
* JWT는 해독하기 [쉬워][https://jwt.io/] 민감한 정보(비밀번호)는 담지 않습니다.
* 예시를 들어보면 client가 서버로 인증 절차를 거치면 server는 secret key를 활용해서 client에게 Access token을 전해줍니다. 
* 이후 client와 server 간의 통신이 이루어질때 server는 client로 부터 받은 token을 자신이 가진 secret key를 활용해 유효한 토큰인지 확인하고 -> 사용자의 정보(이름, 만료시기, 권한)를 파악합니다.
* 따라서 서버가 여러곳이 있어도 secret key를 이용하여 해독을 하기 때문에 서버의 확장성이 원활하게 됩니다.
* 하지만 사용자의 token이 탈취당하면 해커 역시 사용자와 같은 권한을 가지게 됩니다. 따라서 만료시기를 정하게 됩니다.  따라서 만료시기가 지나게 되면 해커도 사용할 수  없고 사용자도 사용할 수 없게 됩니다.
* 따라서 이를 해결하기 위해 __Refresh Token__ 이란 해결책이 등장합니다. 
* Refresh Token을 살펴보면 서버는 Refresh Token 과 Access Token을 만듭니다. 이때 Access Token은 저장하지 않고 Refresh Token만 저장합니다. 이때 client에게는 Access Token과 Refresh Token을 둘 다 주게 됩니다.

* 이때 client의 Access Token이 만료되면 Server는 client에게 더이상 token이 유효하지 않다는 것을 알려주고 다시 client는 refresh token과 access token을  server에게 보내주어 이 token들이 유효하다면 server는 새로운 access token을 client에게 주게 됩니다. 
* __핵심__ 
  * 토큰으로 상태관리를 하기에 따로 세션을 둘 필요가 없다.
  * 따라서 세션에 비해 DB를 많이 거치지 않아도 되서 속도가 조금 빨라진다.
  * 토큰 관리를 해야한다. 결국 토큰도 탈취당할 수 있다.



##### 추가적인 정보

##### Jsessionid

* 톰캣 컨테이너에서 세션을 유지하기 위해 발급하는 키입니다.
* 동작 순서
  * 1. 브라우저 최초 접근시 톰캣은 Response 헤더에 JsessionID를 Request 헤더에 담아 서버로 보냅니다
    2. 발급받은 JsessionID는 클라이언트 단 쿠키로 저장됩니다.
    3. 만약 요청이 다시 발생할 경우, 톰캣은 발급받은 저장된 JsessionID를 request 헤더에 담아 서버로 보냅니다.
    4. 서버는 request 헤더에 있는 JsessionID를 토대로 세션 메모리 영역에 상태를 유지할 수 있는 값을 저장합니다.