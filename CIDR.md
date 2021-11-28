# CIDR

* Classless Inter Domain Routing



## 사전지식

* `IPv4` 는 총 32비트의 숫자로 구성되어있다.(4,294,967,296개)

  * 588,514,304 개는 특정한 목적으로 선점되어 있다.
  * 따라서 가용한 IP 는 3,706,452,992개이다.
  * 이미 충분하지 않다는 것을 알 수 있는데 이를 해결하기 위해 **사설 네트워크(Private Network)** 를 사용한다.

* 사설망

  * 하나의 `Public IP` 를 여러 기기가 공유할 수 있는 방법

  * 하나의 망에는 `Private ip` 를 부여받은 기기들과 gateway로 구성

    * 각 기기는 인터넷과 통신시 `Gateway` 를 통해 통신

  * `Private Ip` 는 지정된 대역의 IP만 사용가능

    사용 가능한 사설망 대역폭

    ![image](https://user-images.githubusercontent.com/55227984/138667523-4fd8d35c-1a0a-4acd-aeb6-d4db962c9fe2.png)

  * 그렇다면 정확히는 어떻게 통신을 하는 것일까?

    ![image](https://user-images.githubusercontent.com/55227984/138668464-0d1a7795-31e0-4c83-af04-859ea17275a9.png)

    * 우선 `192.168.0.2` 가 `61.123.44.1` 에 통신을 하고 싶다면 `NAT` 에 기록을 하고 통신을 하는것이다.

## CIDR 이란?

* 주소의 영역을 여러 네트워크 영역으로 나누기 위해 IP 를 묶는 방식

  * 여러개의 사설망을 구축하기 위해 망을 나누는 방법
  * `Classless` <-> `Classful`(예전에 A,B,C,D 클래스 나눴던거 생각)

* CIDR 표기법

  ![image](https://user-images.githubusercontent.com/55227984/138671950-2bba5590-9a8c-4040-a15e-e5de07a05d61.png)

  * 첫번째/ 마지막 IP 는 예약이 되어있기 때문에 사용이 불가능하다.
    * 첫번째 IP 는 네트워크 자체를 가르키는 IP
    * 마지막 IP 는 Broadcast IP
  * AWS 에서는 총 5개의 Address 를 예약
    * 0: 네트워크 자체 어드레스
    * 1: VPC Router
    * 2: DNS
    * 3: 나중에 사용할 요소(Future use)
    * 마지막 : Broadcast

![image](https://user-images.githubusercontent.com/55227984/138671573-480fd640-56fa-4251-b4ba-91d13f8cb2cb.png)



## 참고

* NAT가 무엇인가요?
  * `Network Address Translation` 은 IP 패킷의 TCP/UDP 포트 숫자와 소스 및 목적지의 IP 주소 등을 재기록하면서 라우터를 통해 네트워크 트래픽을 주고 받는 기술
  * 사설 네트워크에 속한 여러개의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하기 위해 사용된다. 