# IP

* 컴퓨터가 연결된 네트워크 끝단의 주소
* 즉, 컴퓨터의 IP 주소는 언제든지 바뀔 수 있다.
  * 컴퓨터는 이동식 트레일러이고 
  * IP 는 이 트레일러가 자리잡은곳의 도로명 주소라고 생각하면 된다.
  * 그런데 집에 가만히 있어도 바뀔 수 있다.



## IPv4

* 0~255 숫자 4개로 이어진 형태
* 46억개가 나올 수있다.
  * 그런데 세계인구는 76억......
* 따라서 이문제를 해결하기 위해 `공인 IP` 와 `사설 IP` 라는 것이 나눠졌다. 

<img width="2022" alt="Screen Shot 2021-10-18 at 3 52 39 PM" src="https://user-images.githubusercontent.com/55227984/137682735-fefcd902-a218-4dad-97f5-fa9c35f665d5.png">

* 공인 IP 는 지구상에 단 하나만 있는 것이고 사설 IP 는 겹치더라고 괜찮은 것이다.

![image](https://user-images.githubusercontent.com/55227984/137682827-9dea95a8-894b-4031-8318-300c306b3b70.png)

![image-20211018155542176](/Users/nayeong-yun/Library/Application Support/typora-user-images/image-20211018155542176.png)

* 집이나 회사같은곳에서는 이렇게 한 공인 IP(123.4.5.234) 아래 기기마다 사설 IP 가 부여되는 식으로 인터넷을 사용한다. 

![image](https://user-images.githubusercontent.com/55227984/137683171-7a276ae9-317a-4cd9-8ce8-86cefaf04d0c.png)

* 주소가 이 범주 안에 들어간다면 사설 IP라고 보면 된다.
* 여기서 하나 알아야 할 것은 **사설 IP를 쓰는 컴퓨터는 그 컴퓨터에서 공인 IP를 쓰는 서버등의 다른 컴퓨터로는 접근 할 수 있지만 다른 컴퓨터에서 사설 IP를 쓰는 컴퓨터로는 접근할 수 없다. **
  * 왜냐하면 **104동 203호** 만 가지고는 우리가 찾을 수 없으니까.....
* 여기서 알 수 있는것은 모두에게 공개될 서버용 컴퓨터에 사설 IP를 쓰면 안된다는 것이다.
  * 왜냐하면 서버를 찾아가기 위한 IP는 절대 고유해야 외부에서 찾아갈 수 있기 때문이다. 
  * 따라서 웹서비스를 하는 서버들은 공인 IP를 가지고 있다. 
* 그럼 우리집에선 웹 서비스를 하지 못하는 걸까?
  * 아니다. 할 수있다. 다음과 같은 2가지 방법을 사용하자



## 포트포워딩

* 공유기 설정으로 공인 IP에 포트들을 개방해서 내부의 사설 IP마다 하나씩 연결하는 방법이다.

  ![image](https://user-images.githubusercontent.com/55227984/137686061-ccbdab65-8a9d-49af-96b8-318fb260e571.png)



## DMZ

* 공인 IP의 모든 포트를 하나의 사설 IP에 모아주는 방식이다.

  ![image](https://user-images.githubusercontent.com/55227984/137686168-45affd41-cdc4-4675-84ac-280479ec7bc7.png)

  





## 고정 IP(static IP) vs 유동 IP(dynamic IP)

* 공인과 사설 IP 모두 고정 또는 유동 IP 가 될 수 있다. 
* 그렇다면 왜 유동 IP가 있을까? 그냥 다 고정IP로 하면 되지 않을까?
  * 활용할 수 있는 IP에 제한이 있다......
* 서버의 경우 고정 IP를 사용하지 않으면 매우 곤란한 상황이 펼쳐진다. 
  * 내가 naver에 들어갔는데 아까 까지만 해도 잘 접속 되는게 이제 접속이 안돼......
* 일반 가정이나 기기들에는, 주기적으롤 IP를 회수 해서 인터넷을 사용중인 곳에만 나눠주는 유동 IP 방식을 사용한다. 
  * 고정 IP보다 저렴하고
  * IP가 바뀌기 때문에 해킹으로도 고정 IP보다는 어느정도 안전하다.
* 그런데 여기서 의문인건 내가 포트포워딩 방식을 통해서 가정집에서 웹사이트나 NAS를 운영하려고 하는데 IP 주소가 바뀌어 버린다면?
  * DDNS를 사용하자



## DDNS(Dynamic DNS)

* ![image](https://user-images.githubusercontent.com/55227984/137685022-eff3a78f-98fc-418e-9aac-de15f78478cd.png)
* 동적 DNS
  * 수시로 바뀌는 유동 IP를 감지해서 고정된 도메인에 연결 시켜주는 것
  * 업체마다 유료 또는 무료로 제공됨 
  * 즉, ~~~~.ddns.net 이라는 주소에다 내 컴퓨터의 유동 IP를 연결할 수 있다.
* 포트포워딩과 DDNS를 사용하면 개인적으로 운영할 소규모 인터넷 서비스 정도는 운영할 수 있다. 





## IPv6

* 16진수 4자리가 8개 이어진 형태

* 사실상 고갈된 걱정이 없다.....

  ![image](https://user-images.githubusercontent.com/55227984/137684826-6b8856ea-45f3-4cba-b95a-c4dc7a8461ea.png)