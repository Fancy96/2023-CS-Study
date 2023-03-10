# Proxy Server
##### 클라이언트에서 서버로 접속 시 직접적으로 접속하지 않고 중간에 대신 전달해주는 서버
<image src="./img/proxyServerImg.png" width="600" />

## 1. 동작 원리

1. **요청** : 사용자가 웹 브라우저에서 도메인을 입력한다.
2. **전달** : 요청에 대하여 캐시 역할을 하는 프록시 서버로 전달한다.
3. **확인** : 프록시 서버 내에 도메인 홈페이지의 페이지를 가지고 있는지 체크한다.
3. 가지고 있는 경우 :
    - 홈페이지가 있는 서버에 자신이 가진 페이지가 최신 버전인지 체크
    - 필요한 경우 갱신할 부분만 가져옴
4. 가지고 있지 않을 경우 :
    - 홈페이지가 있는 서버와 연결하여 페이지를 가져온다.


## 2. Proxy Server는 왜 필요할까?
### [ 보안 : 요청과 응답의 필터링 ]
<image src="./img/proxyServer_Security.png" width="600" />

- 프록시 서버를 이용하지 않으면 서버의 주소가 쉽게 노출되고 다른 익명의 사용자가 서버로 접근하기 쉬워짐
- 프록시 서버가 중간에 경유하게 되면 서버의 IP 숨기는 것 가능
- 프록시 서버를 방화벽으로 사용하기도함 (프록시 방화벽)

#### 방화벽( firewall)

- 보안 규칙에 기반한, 들어오고 나가는 네트워크 트래픽을 모니터링하고 제어하는 네트워크 보안 시스템
- 신뢰할 수 있는 내부 네트워크, 신뢰할 수 없는 외부 네트워크 간의 장벽을 구성

#### 프록시 방화벽

- 세션에 포함되어 있는 정보의 유해성을 검사하기 위함
- 방화벽에서 세션을 종료하고 새로운 세션을 형성하는 방식
- 출발지에서 목적지로 가는 세션을 가로채어서 출발지에서 방화벽까지의 세션과 방화벽에서 목적지까지 두 세션으로 만든 다음 하나의 세션에서 다른 세션으로 정보를 넘겨주기 전 검사를 수행하는 형태
- 패킷 필터에 비해 많은 부하를 주어서 속도는 느리지만 더 많은 검사 가능
- 프로토콜 변경 등 추가적인 기능 수행 가능
</br>

### [ 분산 처리 : 캐시 사용 로드 밸런싱 ]
<image src="./img/proxyServer_Cache.png" width="600" />

- 프록시 서버 중 일부는 프록시 서버에 요청된 내용을 캐시를 사용해 저장해둠
- 캐시에 저장된 내용에 대한 재요청은 서버 따로 접속 필요 X -> 전송 시간 절약, 외부트래픽 줄임으로써 병목현상 방지
</br>

### [ 우회 ]
<image src="./img/proxyServer_detour.png" width="600" />

클라이언트 1,2,3이 인터넷과 직접적으로 통신하게 되면 IP가 직접적으로 인터넷에 드러나게된다.
이는 보안적으로 취약할 수 밖에 없다. 

**중간에 프록시 서버를 둔다면, 인터넷 서버는 프록시 서버와 통신하기 때문에 클라이언트들의 IP를 알 수 없게 된다!** 

이는 보안적인 취약점을 커버할 수 있게 되며, 특정 서버를 우회하여 들어갈 수 있도록 허용해주는 역할도 하게 된다.



### [ ACL : 사이트 접근 정책 ]
ACL : Proxy Server에 접속할 수 있는 범위를 설정하는 옵션

사이트 접근에 대한 접근 정책을 정의할 수 있다.

> **접근 제어 목록(access control list, ACL) 또는 액세스 제어 목록**
> 
> : 개체나 개체 속성에 적용되어 있는 허가 목록
> 이 목록은 누가 또는 무엇이 객체 접근 허가를 받는지, 어떠한 작업이 객체에 수행되도록 허가를 받을지를 지정하고 있다.


## 3. 종류
### [ Forward 프록시 ]
<image src="./img/proxyServer_Forward.png" width="600" />

- 포워딩 프록시(Forwarding Proxy)를 통상적으로 프록시라고 부른다.
- **클라이언트가 인터넷에 직접 접근하는게 아니라 Forward Proxy Server가 요청을 받고 인터넷에 연결하여 결과를 클라이언트에 전달**
    - ex) 사용자가 naver.com에 연결하고자 할때 사용자가 PC에 직접 연결하는 것이 아닌 Forward 프록시 서버가 요청을 받아 naver.com에 연결하여 그 결과를 사용자에게 전달해줌
- 로컬 디스크에 데이터 저장
- 클라이언트 호스트들은 사용 중인 웹 브라우저를 이용하여 프록시 서버 사용 설정을 해야 함 => **사용 인식 가능**
- 대역폭 사용 감소
- 접근 정책 구현에 있어 다루기 쉽고 비용이 저렴
- 정해진 사이트만 연결할 수 있어 웹 사용 환경 제한 가능 => 기업환경에서 많이 이용
</br>

### [ Reverse 프록시 ]
<image src="./img/proxyServer_Reverse.png" width="600" />

> 여기서 Reverse는 '역전, 거꾸로'가 아닌 '배후, 뒷쪽'의 뜻

- 포워딩 프록시가 서버쪽에도 존재한다고 생각하면 된다.
- **클라이언트가 인터넷에 데이터를 요청하면 리버스 프록시가 이 요청을 받아 내부 서버에서 데이터를 받은 후 클라이언트에 전달**
    - ex) 사용자가 nvaer.com 웹 서비스에 데이터 요청 시, Reverse 프록시가 이 요청을 받아 내부 서버에서 데이터를 받은 후 이 데이터를 사용자에게 전달
- 클라이언트들 프록시 서버에 연결된 것 인식 X, 최종 사용자가 요청 리소스에 직접 접근하는 것처럼 느낌
- 클라이언트는 내부 서버에 대한 정보를 알 필요 없이 리버스 프록시에만 요청하면 됨

- 내부 서버가 직접 서비스 제공해도 되지만 이렇게 하는 이유는 **보안** 때문
    - 기업의 네트워크 환경에는 **DMZ** 라 불리는 내부네트워크/외부네트워크 사이에 위치한 구간 존재
    - 이 구간에 보통 메일 서버, 웹 서버, FTP 서버 등 외부 서비스를 제공하는 서버가 위치
    - 내부 서버 (WAS) 에 직접적으로 접근한다면 DB 에 접근이 가능하기 때문에 해킹 문제 발생할 수 있음
    - 리버스 프록시 서버를 DMZ에 두고 실제 서비스 서버는 내부망에 위치 시킨 후 서비스 하는 것이 일반적

- 내부 서버에 대한 설정으로 로드 밸런싱(Load Balancing) 이나 서버 확장 등에 유리
- SSL 암호화에 좋음
    - 본래 서버가 클라이언트들과 통신을 할때 SSL(or TSL)로 암호화, 복호화를 할 경우 비용이 많이 들게 된다.
    - 그러나 리버스 프록시를 사용하면 들어오는 요청을 모두 복호화하고 나가는 응답을 암호화해주므로 클라이언트와 안전한 통신을 할수 있으며 본래 서버의 부담을 줄여줄 수 있다. 


#### 로드 밸런싱(Load Balancing)
##### 요청에 대한 응답을 처리할 때, 서버에서 일을 부하분산시키는 작업

NGinx를 로드 밸런서로 사용하여 부담하는 부하를 줄여주는 작업을 많이 진행하고 있다.

<image src="./img/proxyServer_LoadBalancing.png" width="600" />

주황색 원 : 일에 대한 부하

- 일에 대한 부하가 9
- 로드 밸런싱 작업으로 각 서버에서 3씩 처리할 수 있게 하여 일의 부하 최소화
- **서버를 추가로 증설하여 일의 부하를 분산시키는 방법을 Scale-out**
- **서버 증설이 아닌 기존 서버 성능자체를 확장시켜 업그레이드 Scale-up**


### [ 차이점 ]
1. End Point
    - Forward : 클라이언트가 요청하는 End Point가 실제 서버 도메인, 프록시는 둘 사이의 통신 담당
    - Reverse : 클라이언트가 요청하는 End Point가 프록시 서버 도메인, 실제 서버 정보 알 수 없음
2. 감추어지는 대상
    - Forward : **클라이언트**
    - Reverse : **서버**
3. 통신 대상
    - Forward : 클라이언트와 Proxy서버가 통신하여 인터넷을 통해 외부에서 데이터 가져옴
    - Reverse : Proxy 서버와 내부망 서버가 통신하여 인터넷을 통해 요청이 들어오면 Proxy 서버가 받아 응답해줌
</br>
</br>
</br>

## ❓ 관련 질문
#### Q1. 프록시 서버 사용시 페이지 내용과 데이터의 값이 계속 바뀌면?
A. 실제 서버에서 응답할 때 캐시 만료 기한을 설정합니다.
프록시 서버로 사용자가 요청했을 때 요청한 시각이 프록시에서 다운받은 시간에서 만료한 기간 이내면 프록시에서 다운로드 할 것이고,
그렇지 않으면 다시 실제 서버로 요청하게 됩니다.

#### Q2. 프록시 서버를 설명하고 사용 사례에 대해 설명해보세요.
A. 프록시 서버란 서버 앞단에 둬서 캐싱, 로깅, 데이터 분석을 서버보다 먼저하는 서버를 말합니다. 
이를 통해 포트 번호를 바꿔서 사용자가 실제 서버의 포트에 접근하지 못하게 할 수 있으며 공격의 DDOS 공격을 차단하거나 CDN을 프록시 서버로 달아서 캐싱 처이를 용이하게 할 수 있습니다. 
nginx로 Node.js로 이루어진 서버의 앞단에 둬서 버퍼 오버플로우를 해결하거나 CloudFlare를 둬서 캐싱, 로그 분석 등을 하는 사용 사레가 있습니다.

#### Q3. VPN과 프록시의 차이는?
A. Proxy와 VPN의 기본적인 차이점은 보안기능입니다. VPN은 보안기능이 탑재되어 있지만 Proxy는 보안성이 취약한 단점이 있습니다.
VPN의 용도는 중간에 공용망을 사용하더라도 사설망처럼 이용하는 것이기 때문에 공용망에서도 암호화되어서 통신이 이루어집니다.
따라서 중간 정보를 탈취당할 염려가 적습니다.
Proxy는 공용으로 사용하고 있는 서버이기 때문에 보안성이 없다는 단점이 있습니다. 하지만 IP주소를 바꾸어 우회하는 목적으로 많이 사용하고 있으며
가치가 없는 정보를 편하게 이용하려고 한다면 Proxy를 이용하는 것도 괜찮은 방법이라고 생각합니다.
</br>
</br>

## 📖 참고 자료
[프록시 서버(Proxy Server)란](https://june-17.tistory.com/226)

[Forward Proxy, Reverse Proxy 정의와 차이점](https://bcp0109.tistory.com/194)

[[CS] 면접질문 대비 - 네트워크](https://velog.io/@yanghl98/CS-%EB%A9%B4%EC%A0%91%EC%A7%88%EB%AC%B8-%EB%8C%80%EB%B9%84-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)

[[Network] 프록시(Proxy)와 로드 밸런싱(Load Balancing)](https://kth990303.tistory.com/355)

[[WEB] 🌐 Reverse Proxy / Forward Proxy 정의 & 차이 정리](https://inpa.tistory.com/entry/NETWORK-%F0%9F%93%A1-Reverse-Proxy-Forward-Proxy-%EC%A0%95%EC%9D%98-%EC%B0%A8%EC%9D%B4-%EC%A0%95%EB%A6%AC)

