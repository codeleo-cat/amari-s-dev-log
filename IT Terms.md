
| [[Docker]] | [[Kubernetes]] | [[Nginx]] | [[GCP]] |  |
| ---- | ---- | ---- | ---- | ---- |
#DevOps
- 인프라 구축(세팅)부터 배포, 운영까지의 과정을 최적화하는 업무를 하는 사람
- 탈중앙 - 블록체인의 핵심. #Web3
- 내가 만든 서비스를 많은 고객들이 이용해주었으면 좋겠다. 

#RBAC
- Role-Based Access Control
- 역할 별로  액세스 권한을 부여한다.

#ABAC
- Attribute-Based Access Control
- 사용자 특성, 개체 특성, 작업 유형 등에 따라 액세스를 결정. 보다 세부적인 control이 가능하다.

#MAU
- Monthly Active User

#Type vs #Interface 
- **Type**
	data의 구조적인 특성을 나타낸다. 
	& 기호를 이용해 확장할 수 있다.
	원시 값, tuple, union 타입 선언이 가능하다.
	computed value 사용 가능

* **Interface**
	클래스나 객체의 행동에 대한 추상적인 계약을 정의한다.
	**선언적 확장**(같은 이름의 interface 선언 시, 자동으로 확장)이 가능하다. 
	extends 키워드를 이용해 확장할 수 있다.
	**Object type**을 설정할 때 사용 가능하다.

#Const vs #Let
- Const는 재할당이 불가능
- let은 재할당이 가능

#Serverless
-  BaaS (**Backend as a Service**)나 FaaS (**Function as a Service** / AWS Lambda, Azure Functions) 등에 의존하여 앱이 동작하는 것을 의미한다.
실제 '물리적' 서버가 없는 것으로 이해해야 한다. 
- *장점* - on-demand 방식이라는 장점이다.
- *단점* - 검색 엔진처럼 속도가 생명인 application에 이상적인 방법은 아니다. 항시 실행 중이 아니라 trigger에 의해 서버를 실행하고 종료하기 때문에 대기 시간이 오래 걸린다.

#NFS
&rarr; 네트워크에서 파일 시스템을 공유. K8s의 pod에서 persistent 볼륨 학습 환경으로 사용.

#Vagrant
&rarr; virtual box와 연계하여 가상 머신 관리를 자동화해 주는 도구.

#Virtual_box
&rarr; windows, mac, linux 서버에서 사용 가능한 hyper visor, vagrant의 백엔드로서 학습용 가상 환경 구축에 사용.

#Warmstart vs #Coldstart in Lambda
- Warm start : 이미 실행 준비가 완료된 상태
- Cold start : 배포 패키지의 크기와 코드의 초기화 시간에 따라 새 실행환경으로 호출을 라우팅할 때 **지연 시간**이 발생하는 현상.

#Enum ✅ 열거형 변수
- TypeScript가 자체적으로 구현하는 기능(JS not support)
왜 권장하지 않나? **tree-shaking 불가**. (export했지만 어디에서도 import하지 않은 모듈/코드) 사용하지 않는 코드를 삭제하는 기능. 

#CIDR ✅ 클래스없는 도메인간 라우팅 기법
- [What is CIDR](https://aws.amazon.com/ko/what-is/cidr/) / [CIDR 쉽게 계산하기](https://kim-dragon.tistory.com/154) / [CIDR를 통해 알아보는 서브넷마스크](https://hwannny.tistory.com/86)
&rarr; Classless Inter-Domain Routing (사이더)
- 이전에는 IP 주소는 class 기반이었기에 IP 주소 할당이 비효율적+공간 낭비로 이어졌음.
CIDR를 사용하면 class 기반이 아닌 **네트워크 및 호스트 주소**를 검색하여 유연한 IP 주소 할당이 가능함. 
데이터 라우팅 효율성을 향상시키는 IP 주소 할당 방법
- 🔑 IP주소와 subnet mask를 동시에 표기하는 기법. **subnet 분할이 유연해진다**는 장점
	IP - 210.77.8.155, 서브넷 마스크 - 255.255.255.192 일경우
	210.77.8.155/26와 같이 표기한다.

* *class 기반*
Class A : 0.0.0.0 ~ 127.255.255.255 
Class B :  128.0.0.0 ~ 172.31.255.255 
Class C : 192.0.0.0 ~ 223.255.255.255
Class D : 224.0.0.0 ~ 239.255.255.255 &rarr; #Multicast 용으로 실제 IP 주소가 할당 X
Class E : 240.0.0.0 ~ 255.255.255.255 &rarr; 기타 목적으로 예약되어, 실제 IP 주소가 할당 X

#IPv4 vs #IPv6 ✅ IPv6는 IPv4를 대체하도록 설계된 네트워크 주소 지정 시스템.
- 부족한 주소 문제 해결을 위한 대안이 IPv6
- IPv4 : 32 비트 (2의 32승, 약 43억개)
- IPv6 : 128 비트 (2의 128승, 340간 개) &rarr; DHCP 서버가 없어도 IP 주소를 자동 할당 가능함.

#HTTP vs #HTTPS ✅ https는 http+암호화가 추가된 프로토콜
 ![200](https://www.cloudflare.com/img/learning/security/glossary/what-is-ssl/http-vs-https.svg)
- [http vs https](https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/)
- client와 server 간 통신을 위한 프로토콜
- HTTP(80)는 암호화되지 않는 데이터를, HTTPS(443)는 암호화된 데이터를 전송한다. <br/> 로드 속도도 더 빠르고 참조 링크도 잘 추적한다.
- HTTPS(Hypertext Transfer Protocol **Secure**) 🔑 HTTP의 확장 버전. 안전한 버전
- **Secure** - HTTP 요청 및 응답을 SSL 및 TLS 기술에 결합

#SSL vs #TLS

#Busybox
- 경량의 리눅스 배포 중 하나로, 여러 유틸리티와 명령줄 도구를 제공하는 **작은 이미지**.
- 많은 일반적인 UNIX 유틸리티의 작은 버전을 하나의 작은 실행 파일로 결합한다.

#DHCP  ✅ 네트워크 설정을 자동으로 해주는 방법

![[DHCP.png|400]]
[Dynamic Host Configuration Protocol](https://learn.microsoft.com/ko-kr/windows-server/networking/technologies/dhcp/dhcp-top)
공유기에 보통 DHCP server가 내장되어 있고, 컴퓨터에는 보통 DHCP client가 있다.
broadcast와 변칙적인 UDP를 사용해서 DHCP server - DHCP client 사이 필요한 정보를 교환한다.


#URI vs #URL ✅ URI는 식별자, URL은 식별자+위치
- URI (Uniform Resource Identifier)
인터넷 상의 리소스 자체를 식별하는 고유한 문자열 시퀀스
ex) elancer.co.kr
- URL (Uniform Resource Locator)
리소스의 위치를 나타내기 위한 규약.	URI + 프로토콜이 결합된 형태. (http, https)
ex) https://elancer.co.kr

#L2_Switch
- MAC 주소 기반으로 통신한다. MAC 주소는 48bit
- L2 Access : endpoint와 직접 맞닿는 스위치 
- L2 Distribution : Router로 데이터를 보내는 스위치
- uplink : 상위 계층 스위치로 연결되는 Line (= 더 큰 네트워크로 나가는 역할)

#MAC주소 ✅ 기기에 존재하는 NIC를 특정하는 주소 (**NIC에 대한 식별자**)
-  Media Access Control Address
PC를 비롯한 각종 단말, 프린터 같은 각종 기기에 존재하는 **NIC를 특정하는 주소** = 이더넷용 주소
FF:FF:FF:FF:FF:FF는 이더넷에 접속한 모든 NIC를 대상으로 일제히 송신한다 = #Broadcast 주소
데이터 링크 계층에서 사용되며, 물리적인 네트워크 장비를 식별하는데 사용된다.
- NIC (= Network Interface Controller, LAN 카드 / Network Interface Card)
	NIC에 여러 개의 IP를 바인딩 가능

#IP주소 ✅ **Host에 대한 식별자**
* Host : 네트워크에 연결된 컴퓨터

#port ✅ **Process에 대한 식별자**
- 컴퓨터가 data를 어느 process에게 전달해야 되는지를 알려주는 unique한 번호
- 전송 계층에서 사용되는 기술
- sender가 data를 보낼 때 **+** 수신자 컴퓨터의 process의 port 번호

#process ✅ 동작 중인 프로그램
- 프로그램이 실행되어서 메모리에 올라간 상태

#Endpoint
- API가 server에서 resource에 접근 가능하도록 하는 URL

#Switching ✅ 네트워크 내부에서 패킷 전송을 담당 (물리적인 전송)
[스위칭 개념](https://www.youtube.com/watch?v=oAbukpZbpTg) /  [스위치 기능 ](https://www.youtube.com/watch?v=jKCV6s6FKrg) /  [스위치:2계층 장비](https://catsbi.oopy.io/315731e3-1730-4690-ad8f-663e0af7621b)

#Switch : 교차로  ✅ 네트워크 내에서 **packet을 받아 필요한 곳으로 보내주는 역할**
&rarr; Switch는 기본적으로 L2 장비
 1. Learning : Source MAC address를 기반으로 MAC address table을 만드는 기능
 2. Forwarding : Destination MAC Address가 연결되어 있는 port로 frame을 전달하는 기능
 3. Filtering : 프레임이 유입된 port로 다시 frame을 전송하지 않는 기능
 4. Flooding : MAC 주소 테이블에 목적지에 MAC에 대한 정보가 없을 경우 frame을 모든 port로 전송하는 기능 = 테이블에 없는 도착지 주소를 가진 패킷이 들어오면 스위치는 전체 포트로 패킷을 전송한다.

#Router ✅ 최적 경로 선택 
기본적으로 L3 Switch. Internet은 router의 집합체
Packet이 Router에 도착하면, 이 Router들 간 특정 프로토콜로 통신하며 최적화된 경로를 찾는데, 이 경로 선택의 base가 되는 것이 ⭐️ Routing Table (이정표)

#Broadcast / #Unicast / #Multicast ✅ 네트워크 통신에서 사용되는 데이터 전송 방식
* 브로드캐스트 : 네트워크 상에 연결된 모든 장치에 데이터를 전송하는 방식. 주로 소규모 네트워크
* 유니캐스트 : 하나의 송신자가 특정 수신자 에게만 데이터를 전송하는 방식. 일반적으로 가장 효율적인 전송 방식
* 멀티캐스트 : 송신자가 그룹 내의 여러 수신자에게 데이터를 전송하는 방식. ex) 멀티 스트리밍
- broadcast strom - 스위치가 broadcast 트래픽을 계속 발생시켜 네트워크 다운을 초래하는 것

#STP ✅ Loop를 방지
Spanning Tree Protocol 
특정 Port를 차단 상태로 바꾸어 놓음

#Domain 
- 컴퓨터에 부여된 계층적인 이름 (사람이 외우기 쉬운 의미가 있는 이름)
- DNS : **도메인명과 IP 주소를 상호 변환**하는 구조 (Domain Name System)
	- 구성 : content server, cache server, 컴퓨터 내부의 stub resolver
	- 가치가 제대로 발휘될 때 : GSLB (Global Server Load Balancing)

#public_subnet ✅ 외부에서 직접 접근 가능한 서브넷
- public subnet에 속한 리소스는 외부에서 직접 접근이 가능함. 
- 리소스 - 웹 서버, 로드 밸런서, CDN 등
#CDN (Content Delivery Network) - end user에게 더 빠르게 contents 제공 가능, 로드 시간이 빨라진다.
- public ip를 통해 직접 인터넷과 통신 가능
- Internet g/w를 포함하여 외부와의 통신을 허용하는 라우팅 규칙을 지님

#private_subnet ✅ 외부에서 직접 접근 불가한 서브넷
- private_subnet에 속한 리소스는 외부에서 직접 접근이 불가능함.
- 리소스 - DB 서버, Application 서버
- 외부와 통신하기 위해서는 NAT(Network Address Translation) g/w를 사용하여야 함.

#connection_pool ✅ 미리 준비된 연결 객체
- 여러 client에서 동시에 db에 접근할 수 있도록 하는 메커니즘. 
- 원리 - 각 client는 필요할 때 db 연결을 pool에서 가져와 사용하고, 사용이 끝나면 pool에 반환함.
- *장점* 
db 연결을 맺고 해제하는 과정 -> 고 비용. connection pool을 통해 **이미 생성된 연결을 재사용함**으로써 연결을 맺고 해제하는 데 필요한 overhead를 줄일 수 있음. 
여러 client 요청에 대해 동시에 여러 연결을 효과적으로 처리할 수 있음.(***scaling***)
[connection pool이란?](https://shuu.tistory.com/130)

#NAT  ✅ 네트워크 주소 변환 
Network Address Translation
- private ip를 사용하는 device들을 **공인 IP 주소 하나로 변환**하여 인터넷과 통신할 있게 하는 기술
- IP 주소 부족 문제를 해결 + 방화벽 역할

**대칭키 방식 암호화** 
- 암호화하고 복호화하는 key가 똑같다.<br/>

#공개키_암호화
**비대칭키 방식 암호화 (=공개키 암호화)** 
- 공개키는 어디든 뿌려져도 된다. 복호화 시에는 개인키를 사용해서 풀기 때문. 다만 이 개인키 (private key)는 알려지면 절대 안된다.<br/>
- 결론
대칭키 + 비대칭키 혼합 방식을 사용해서 HTTPS (HTTP+*Secure*) 통신을 한다.
[대칭키 vs 비대칭키](https://www.youtube.com/watch?v=H6lpFRpyl14)

#RTO / #RPO
- **RTO** (Recovery Time Objective, 목표 복구 시간)
비상사태 또는 업무 중단 시점으로부터 복구되어 가동될 때까지의 소요 시간을 의미 
ex) 장애 발생 후 4시간 내 복구 가능 <br/>
- **RPO** (Recovery Point Objective, 목표 복구 시점)
비상사태 또는 업무 중단 시점으로부터 데이터를 복구할 수 있는 기준점을 의미
ex) 장애 발생 전인 지난 주 목요일에 백업시켜 둔 복원 시점으로 복구 가능

#SLA / #SLI / #SLA ✅ 서비스 레벨과 관련된 용어
[SLO, SLI, SLA란?](https://newrelic.com/kr/blog/best-practices/what-are-slos-slis-slas)
* **SLO** (Service Level Objectives, 서비스 레벨 목표) : 시스템에서 기대되는 가용성을 설정한 목표
* **SLI** (Service Level Indicator, 서비스 레벨 지표) : 시스템의 가용성을 파악하기 위한 핵심 측정치와 지표
* **SLA** (Service Level Agreements, 서비스 레벨 계약) : 시스템이 SLO를 충족하지 못할 경우 발생하는 상황, 합의된 내용을 설명하는 계약

#SRE
- Site Reliability Engineering, SRE : 사이트 안정성 엔지니어링

#BlackboxTest 🔑 **내부 구조를 모른다.**
- 소프트웨어의 내부 구조나 작동 원리를 모르는 상태에서 소프트웨어의 동작을 검사하는 방법
- A, B, C 요구사항 명세를 보면서 테스트하는 것으로, 주로 구현된 기능을 테스트한다. = (기능 테스트)

#3-tier-architecture 🔑 **Presentation.Application.Data**
![Client-Server-DB|400](https://www.zirous.com/wp-content/uploads/2022/10/Untitled-5-01.png)
- Presentation tier : 사용자가 직접 마주하게 되는 계층. GUI / Front-end
- Application tier : 정보를 처리하고 가공하며 비즈니스 로직을 처리하는 계층. 
	ex) 미들웨어 / Back-end. WAS (웹 애플리케이션 서버)
- Data tier = DB에 접근해서 데이터를 read/write 하는 것. DBMS
