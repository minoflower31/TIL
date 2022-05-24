# 인터넷 네트워크

인터넷 내부에서 컴퓨터끼리 통신하는 방법은 메시지를 주고 받아야 한다. (클라이언트 - 서버)

## IP

인터넷 망은 복잡하게 연결된 노드들로 구성되어 있는데, 어떻게 메시지를 보낼까? <br>
> IP 주소를 부여한다.

IP의 역할은 지정한 IP 주소에 데이터를 전달하는데, 이 때 `패킷`이라는 통신 단위로 전달한다. <br>

### IP 패킷 정보

<img width="295" alt="image" src="https://user-images.githubusercontent.com/56334513/169987608-4550a5a5-fd3f-4255-be28-b5c4a00848f3.png">

## IP 프로토콜의 한계

### 비연결성
+ 패킷을 받을 대상이 없거나 서비스가 불능 상태여도 패킷을 전송한다. 즉, 클라이언트는 서버가 패킷을 잘 받을 수 있는 상태인지도 모름.
  + e.g. 편지를 전송할 때 상대의 주소가 동일한지, 바뀌었는지 알 수 없다.

### 비신뢰성
+ 중간에 패킷이 사라지거나 순서대로 오지 않는다면?
  + 패킷에 따라 전송되는 노드가 달라질 수 있음.

### 같은 IP 서버에 통신하는 애플리케이션이 2개 이상이면 어디로 보낼지 알 수 없다.

<br>

## TCP/UDP

### 인터넷 프로토콜 스택의 4계층

<img width="359" alt="image" src="https://user-images.githubusercontent.com/56334513/169988622-0fe46572-fbe5-4597-bb82-684abb169f9d.png">

<br>

### 프로토콜 계층 별 역할

<img width="519" alt="image" src="https://user-images.githubusercontent.com/56334513/169988789-26b595b2-6f03-4d9b-8271-fc40de2861ca.png">

    Ethernet frame == MAC 주소

<br>

### TCP/IP 패킷 정보

<img width="336" alt="image" src="https://user-images.githubusercontent.com/56334513/169989194-63307473-66f2-4641-aa2f-1e838fa6edbb.png">

<br>

### TCP(Transmission Control Protocol) 특징
1. 연결 지향적이다. `TCP 3-way-handshake`
2. 데이터 전달을 보장한다.
   + 클라이언트가 데이터를 전송하면 서버는 잘 받았다는 응답을 보낸다.
3. 순서를 보장한다.
   + 클라이언트에서 패킷 순서 **1->2->3**으로 전송하는데 만약 서버에서 **1->3->2**로 오면 **패킷2부터 재전송하라고 응답함**
4. 그래서 신뢰할 수 있는 프로토콜이다.

#### 3-way-handshake

<img width="479" alt="image" src="https://user-images.githubusercontent.com/56334513/170066807-74752294-4362-4cfe-9a6a-6ab801ce72ca.png">

### UDP(User Datagram Protocol) 특징
1. 이것은 하얀 도화지다.. (기능이 거의 없음)
2. 연결 지향적이지 않다.
3. 데이터 전달을 보장하지 않는다.
4. 순서를 보장하지 않는다.
5. **하지만 단순하고 빠른 장점이 있다!**
    + IP 기능 + `PORT`, `CHECKSUM` 추가

+ 최근에 각광받는 기술이며 웹 브라우저에서 http 통신할 때 TCP 3-way-handshake 용량을 줄이기 위해 UDP로 대체 (http2,http3)
    
<br>

## PORT

서버 내에 여러 어플리케이션이 존재하면 IP 주소로는 분별할 수 없다.
+ 출발지 PORT와 목적지 PORT로 해결할 수 있다.

<br>

같은 IP 주소내에서 **프로세스**로 구분하면 된다. <br>
`0~65535`까지 할당 가능하며, `0~1023`은 잘 알려진 포트이므로 사용하지 않는 것이 좋다.
+ FTP: 20,21
+ TELNET: 23
+ HTTP: 80
+ HTTPS: 443
> IP는 아파트이고, PORT는 아파트 동 호수

<br>

## DNS (Domain Name System)

도메인 명을 IP 주소로 변환시키는 시스템이다.
+ IP 주소는 기억하기 어렵고, 변경될 가능성이 있다.

<img width="248" alt="image" src="https://user-images.githubusercontent.com/56334513/170065160-994872dd-337c-4270-9435-b2ad138ad1b6.png">