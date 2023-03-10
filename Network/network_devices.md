## 네트워크 기기

## 네트워크 기기의 처리 범위

네트워크 기기는 계층별로 처리 범위를 나눌 수 있음.

물리 계층을 처리할 수 있는기기와 데이터 링크 계층을 처리할 수 있는 기기 등이 있음.

그리고 상위 계층을 처리하는 기기는 하위 계층을 처리 할 수 있지만 그 반대는 불가.

L7 스위치는 애플리케이션 계층을 처리하는 기기로, 그 밑의 모든 계층의 프로토콜을 처리할 수 있지만 AP는 물리 계층 밖에 처리하지 못함.

- 애플리케이션 계층: L7 스위치
- 인터넷 계층: 라우터, L3 스위치
- 데이터 링크 계층: L2스위치, 브리지
- 물리 계층: NIC(Network Interface Card), 리피터, AP

## 애플리케이션 계층을 처리하는 기기

### L7 스위치

스위치는 여러장비를 연결하고 데이터 통신을 중재하며 목적지가 연결된 포트로만 전기 신호를 보내 데이터를 전송하는 통신 네트워크 장비

로드 밸런서라고도 하며, 서버의 부하를 분산하는 기기. 클라이언트로 부터 오는 여청들을 뒤쪽의 여러 서버로 나누는 역할을 하며 시스템이 처리할 수 있는 트래픽 증가를 목표로 함.

URL, 서버, 캐시, 쿠키들을 기반으로 트래픽을 분산함. 또한, 바이러스, 불필요한 외부 데이터 등을 걸러내는 필터링 기능 또한 가지고 있으며 응용 프로그램 수준의 트래픅 모니터링도 가능함.

만약 장애가 발생한 서버가 있다면 이를 트래픽 분산 대상에서 제외해야 하는데, 이는 정기적으로 헬스 체크(health check)를 이용하여 감시하면서 이루어짐.

#### L4스위치와 L7스위치 차이

로드밸런서로는 L7스위치뿐만 아니라 L4 스위도 있음. L4스위치는 전송 계층을 처리하는 기기로 스트리밍 관련 서비스에서는 사용할 수 없으며 메시지를 기반으로 인식하지 못하고 IP와 포트를 기반으로 트래픽을 분산.

L7로드밸런서는 IP, 포트 외에도 URL, HTTP 헤더, 쿠키 등을 기반으로 트래픽 분산.

클라우드 서비스 등에서는 L7스위치를 이용한 로드밸런싱은 ALB(Application Load Balancer) 컴포넌트로 함.

L4스위치를 이용한 로드밸런싱은 NLB(Network Load Balancer)컴포토넌트라 한다.

#### 헬스 체크

L4스위치 또는 L7스위치 모두 헬스 체크를 통해 정상적인 서버 또는 비정상적인 서버를 판별. 전송 주기와 재전송 횟수 등을 설정한 이후 반복적으로 서버에 요청을 보내어 확인함.

TCP, HTTP 등 다양한 방법으로 요청을 보내며 이 요청이 정상적으로 이루어졌다면 정상적인 서버로 판별.

EX) TCP 요청을 보냈는데 3-way handshake가 정상적으로 일어나지 않으면 비정상.

> L4 스위치의 헬스체크
> 
1. TCP

L4 스위치가 서버와 3-way handshake를 실시. 논리적인 연결인 커넥션이 생성하게 되는데 헬스체크가 목적일 경우 FIN packet을 날려 커넥션 제거.

1. HTTP

3-way handshake를 실시하는 것은 동일. 추가로 HTTP Request(Get)을 전송하여 웹페이지를 요구.

- get과 post의 차이를 설명하는 면접질문이 자주 나온다고함!

#### 로드밸런서를 이용한 서버 이중화

로드밸런싱이란 분산식 웹 서비스로 여러 서버에 부하를 나누어 안정적으로 서비스를 유지할 수 있게 함.

로드밸런서는 대표적인 기능으로 서버 이중화를 들 수 있다. 로드밸런서는 2대 이상의 서버를 기반으로 가상 IP를 제공하고 이를 기반으로 안정적인 서비스를 제공.

로드밸런서가 제공한 0.0.0.12010이란 가상 IP에 사용자들이 접근하고 뒷단에 사용 가능한 서버인 0.0.0.12011과 0.0.0.12012를 기반으로 서빙함. 이렇게 하면 0.0.0.12011이란 서버에 장애가 발생해도 그것과는 무방하게 0.0.0.12012 서버를 기반으로 안정적인 서비스를 운용 가능함.

> 로드 밸런싱 알고리즘
> 
1. 라운드로빈 방식 (Round Robin Method)

서버에 들어온 요청을 순서대로 돌아가며 배정하는 방식. 클라이언트의 요청을 순서대로 분배. 서버와의 연결(세션)이 오래 지속되지 않는 경우에 적합

1. 가중 라운드로빈 방식 (Weighted Round Robin Method)

각각의 서버마다 가중치를 매기고 가중치가 높은 서버에 클라이언트 요청을 우선 배분.

서버의 트래픽 처리 능력이 상이한 경우 사용되는 부하 분산 방식.

1. IP 해시 방식 (IP Hash Method)

클라이언트의 IP주소를 특정 서버로 매핑하여 요청을 처리하는 방식. 사용자의 IP를 해싱하여 로드를 분배하기 때문에 사용자가 항상 동일한 서버로 연결되는 것을 보장

1. 최소 연결 방식 (Least Connection Method)

요청이 들어온 시점에 가장 적은 연결상태를 보이는 서버에 우선적으로 트래픽 배분. 자주 세션이 길어지거나, 서버에 분배된 트래픽이 일정하지 않은 경우 적합.

1. 최소 리스폰타임 (Least Response Time Method)

서버의 현재 연결 상태와 응답시간을 모두 고려하여 트래픽 배분. 가장 적은 연결 상태와 가장 짧은 응답시간을 보이는 서버에 우선적으로 분배.

|  | L4 로드 밸런서 | L7 로드 밸런서 |
| --- | --- | --- |
| 네트워크 계층 | Layer 4 전송계층 (Transport layer) | Layer7 응용계층 (Application layer) |
| 특징 | TCP/UDP 포트 정보를 바탕으로 함 | TCP/UDP 정보는 물론 HTTP의 URL, FTP의 파일명, 쿠키 정보 등을 바탕으로 함 |
| 장점 | 1. 데이터 안을 들여다보지 않고 패킷 레벨에서만 로드를 분산하기 때문에 속도가 빠르고 효율이 높음.
2. 데이터의 내용을 복호화할 필요가 없기에 안전함.
3. L7 로드 밸런서보다 가격이 저렴함 | 1. 상위 계층에서 로드를 분한하기 때문에 훨씬 더 섬세한 라우팅이 가능함
2. 캐싱 기능을 제공함
3. 비정상적인 트래픽을 사전에 필터링할 수 있어 서비스 안정성이 높음 |
| 단점 | 1. 패킷의 내용을 살펴볼 수 없기 때문에 섬세한 라우팅이 불가능 함.
2. 사용자의 IP가 수시로 바뀌는 경우라면 연속적인 서비스를 제공하기 어려움 | 1. 패킷의 내용을 복호화해야 하기에 더 높은 비용을 지불해야 함.
2. 클라이언트가 로드 밸런서와 인증서를 공유해야 하기 때문에 공격자가 로드밸런서를 통해서 클라이언트에 데이터에 접근할 보안상의 위험성이 존재함. |

참고 : 

[로드밸런서(Load Balancer)의 개념과 특징](https://m.post.naver.com/viewer/postView.naver?volumeNo=27046347&memberNo=2521903)

### 인터넷 계층을 처리하는 기기

인터넷 계층을 처리하는 기기로는 라우터와 L3스위치가 있음.

#### 라우터

라우터(router)는 여러개의 네트워크를 연결, 분할, 구분시켜주는 역할을 하며 “다른 네트워크에 존재하는 장치끼리 서로 데이터를 주고받을 때 패킷 소모를 최소화하고 경로를 최적화하여 최소 경로로 패킷을 포워딩 하는 라우팅 장비.

> 라우팅 경로 결정 순서
> 

Longest Match Rule → AD → Metric

#### L3스위치

L3스위치란 L2스위치의 기능과 라우팅 기능을 갖춘 장비. L3스위치를 라우터라고 해도 무방. 라우터는 소프트웨어 기반의 라우팅과 하드웨어 기반의 라우팅을 하는 것으로 나눠지고 하드웨러 기반의 라우팅을 담당하는 장치를 L3스위치라 함.

| 구분 | L2스위치 | L3스위치 |
| --- | --- | --- |
| 참조 테이블 | MAC 주소 테이블 | 라우팅 테이블 |
| 참조 PDU | 이더넷 프레임 | IP 패킷 |
| 참조 주소 | MAC 주소 | IP 주소 |

> L2스위치와 ARP
> 
1. 출발지 주소가 MAC주소 테이블에 없으면 주소를 저장하고 자신의 스위치에 연결되어 있는 서버나 컴퓨터의 정보를 수집
2. 목적지 주소가 MAC주소 테이블에 없으면 전체 포트로 전달(Broadcastring)
3. 목적지 주소가 MAC주소 테이블에 있으면 해당 포트로 전달
4. 출발지와 목적지가 같은 네트워크 영역이면 다른 네트워크로 전달하지 않음. (IP구조를 보면 네트워크 영역 파악 가능)

MAC 주소 테이블의 각 주소는 일정 시간 이후에 삭제

1. 스위치의 MAC주소 테이블은 시간이 지나면 삭제
2. 삭제되는 이유는 테이블 저장 공간을 효율적으로 사용
3. 해당 포트에 연결된 PC가 다른 포트에 옮겨진 경우도 발생
4. 기본 300초 저장, 다시 프레임이 발생되면 다시 카운트

ARP

IP주소를 통해 MAC주소를 알려주는 프로토콜

1. 컴퓨터 A가 컴퓨터 B에게 IP통신을 시도하고 통신을 수행하기 위해 목적지 MAC주소를 알아야 한다.
2. 목적지 IP에 해당하는 MAC주소를 알려주는 역항을 ARP가 해준다.

> 라우팅 테이블
> 

 컴퓨터 네트워크에서 목적지 주소를 목적지에 도달하기 위한 네트워크 노선으로 변환시키는 목적으로 사용 됨.

 라우팅 프로토콜의 가장 중요한 목적이 바로 이러한 라우팅 테이블의 구성

> IPv4구조
> 

IPv4는 32비트로 네트워크 ID와 호스트 ID로 나뉘어져 있음. ‘ . ’(dot)으로 구분된 Octet(8bit / 1byte) 4개 조합.

네트워크ID : 어떤 네트워크 인가

호스트ID : 해당 네트워크의 어느 컴퓨터 인가

| 클래스 이름 | 내용 | 공인 IP 주소 범위 ( 각 비어있는 중간 범위는 사설 IP 주소 범위) |
| --- | --- | --- |
| A | 대규모 네트워크 주소 | 1.0.0.0 ~ 9.255.255.255
11.0.0.0 ~ 126.255.255.255 |
| B | 중형 네트워크 주소 | 128.0.0.0 ~ 172.15.255.255
172.32.0.0 ~ 191.255.255.255 |
| C | 소규모 네트워크 주소 | 192.0.0.0 ~ 192.167.255.255
192.169.0.0 ~ 233.255.255.255 |
| D | 멀티캐스트 주소 |  |
| E | 연구 및 특수용도 |  |

네트워크 주소 : 전체 네트워크에서 작은 네트워크를 식별하는데 사용하는 주소 

ex) c 클래스에서 호스트ID가 0인 192.168.1.0이 네트워크 주소

브로드캐스트 주소 : 네트워크에 있는 컴퓨터나 장비 모두에게 한 번에 데이터를 전송하는데 사용되는 전용 IP 주소

ex) c 클래스에서 호스트 ID가 255인 192.168.1.255가 브로드캐스트 주소

## 데이터 링크 계층을 처리하는 기기

L2스위치와 브리지가 있음.

### L2스위치

장치들의 MAC주소를 MAC 주소 테이블을 통해 관리하며, 연결된 장치로 부터 패킷이 왔을 때 패킷 전송을 담당함.

IP주소를 이해하지 못해 IP주소를 기반으로 라우팅은 불가능. 단순히 패킷의 MAC주소를 읽어 스위칭하는 역할을 함.

목적지가 MAC주소 테이블에 없다면 전체 포트에 전달하고 MAC주소 테이블의 주소는 일정 시간 이후 삭제하는 기능도 있음.

### 브리지

브리즈는 두 개의 근거리 통신망(LAN)을 상호 접속할 수 있도록 하는 통신망 연결 장치로, 포트와 포트 사이의 다리 역할을 하며 장치에 받아온 MAC주소를 MAC주소 테이블로 관리.

브리지는 통신망 범위를 확장하고 서로 다른 LAN등으로 이루어진 ‘하나의’ 통신망을 구축할 때 사용

### 물리 계층을 처리하는 기기

NIC, 리피터, AP가 있음.

### NIC

LAN카드라고 하는 네티워크 인터페이스 카드는 2대 이상의 컴퓨터 네트워크를 구성하는데 사용.

네트워크와 빠른 속도로 데이터를 송수신 할 수 있도록 컴퓨터 내에 설치하는 확장 카드.

각 LAN 카드에는 각각을 구분하기 위한 고유의 식별 번호인 MAC주소가 있음.

### 리피터

리피터는 들어오는 약해진 시호 정도를 증촉하여 다른 쪽으로 전달 하는 장치. 이를 통해 패킷이 더 멀리 갈 수 있음. 하지만 이는 광케이블이 보급됨에 따라 현재는 잘 쓰이지 않음.

### AP

AP(Access Point)는 패킷을 복사하는 기기

AP에 유선 LAN은 연결한 후 다른 장치에서 무선 LAN기술(와이파이 등)을 사용하여 무선 네트워크 연결을 할 수 있음.

## 예상질문

1. 로드 밸런싱의 의미와 알고리즘에는 어떤 것이 있을까?

1. get과 post의 차이점은?

GET요청은 서버에 존재하는 정보를 요청합니다. 이 때 반환되는 정보는 정보 자체가 아니라 정보의 표현입니다.(뒤의 내용은 REST와 연관이 있고, 굳이 답변하지 않으셔도 됩니다.) 일반적으로 Request Body는 입력하지 않는 것이 일반적이며, 레거시 시스템의 경우 요청을 받아들이지 않을 수 있습니다. 캐싱을 수행하기 때문에 캐싱되지 않는 요청은 GET 요청이 맞지 않을 수 있습니다.

POST요청은 서버에 정보를 생성하는 것을 요청합니다. 예전 HTTP 통신은 POST 요청으로 데이터 삭제, 수정도 form요청으로 같이 수행했습니다. POST 요청은 서버의 상태를 변경시키기 때문에 멱등성이 유지되지 않습니다. 보통 Request Body에 요청하는 데이터를 담아 전송합니다.

1. put과 path의 차이점은?

• PUT 요청은 서버에 존재하는 데이터를 수정하거나 존재하지 않으면 생성합니다. CRUD로 따지면 C,U입니다.

• PATCH 요청은 서버에 존재하는 데이터를 일부 수정합니다. CRUD로 따지면 U입니다.

추가로 delete메서드도 있음.

• DELETE 요청은 서버에 데이터를 제거할 것을 요청합니다. 존재하지 않아도 동일하게 동작합니다. CRUD로 따지면 D입니다.
