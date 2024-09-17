---
created: 2024-01-28T14:41
updated: 2024-03-03T14:53
---
- **네트워크 통신**이 일어나는 과정을 7단계로 나누어 ISO에서 정의한 네트워크 표준 모델

|     | 계층        | 역할                                              | 장비/기타                  | PDU      | Protocol                  |
| --- | --------- | ----------------------------------------------- | ---------------------- | -------- | ------------------------- |
| L7  | 응용 계층     | 최종 목적지. 직접 응용 서비스 수행                            | **= TCP/IP 응용 계층**     |          | HTTP, FTP                 |
| L6  | 표현 계층     | 전송하는 데이터의 표현 방식 결정                              | JPEG, MPEG, GIF, ASCII |          |                           |
| L5  | 세션 계층     | 연결 유지                                           | API, [[Socket]]        |          |                           |
| L4  | 전송 계층     | IP와 Port를 이용하여 Process와 통신. <br/>통신 노드 간 연결 제어. | **= TCP/IP 전송 계층**     | Segment  | TCP, UDP                  |
| L3  | 네트워크 계층   | 데이터를 (목적지까지) 어떻게 전송하나? *Router*                 | **= TCP/IP 인터넷 계층**    | *Packet* | IP, ICMP, IGMP, ARP, RARP |
| L2  | 데이터 링크 계층 | L1을 통해 송수신되는 정보의 오류와 흐름 관리                      | 브릿지, 스위치, 이더넷          | Frame    | Ethernet                  |
| L1  | 물리 계층     | 통신 케이블로 데이터를 전송하는 물리적인 장비                       | 리피터, 허브, 통신 케이블        | Bit      | IEEE802.11                |
## 물리 계층 (L1) ✅ 전기 신호로 변환 + 물리적 매체 통한 전송
- 두 대의 컴퓨터 사이의 통신. 데이터를 전기 신호로 변환 + 실제 케이블, 무선 등의 물리적 매체를 통해 전송하는 계층
- 특징 - H/W 적 구현
- PDU : bit
- 장비 : 리피터, 허브, 통신 케이블, NIC

## 데이터 링크 계층 (L2) ✅ 1계층 오류 감지 및 수정 + 물리적 링크 통한 전송
- 같은 네트워크에 속한 컴퓨터 사이의 통신. 데이터 전송
- PDU : frame (Framing - 원본 데이터를 감싼 것)
- 장비 : 스위치(Switch), 이더넷, 브릿지(Bridge)

#Switching ✅ 네트워크 내부에서 패킷 전송을 담당 (물리적인 전송)
[스위칭 개념](https://www.youtube.com/watch?v=oAbukpZbpTg) /  [스위치 기능 ](https://www.youtube.com/watch?v=jKCV6s6FKrg) /  [스위치:2계층 장비](https://catsbi.oopy.io/315731e3-1730-4690-ad8f-663e0af7621b)

스위치 : ✅ 네트워크 내에서 **packet을 받아 필요한 곳으로 보내주는 역할**
 1. Learning : Source MAC address를 기반으로 MAC address table을 만드는 기능
 2. Forwarding : Destination MAC Address가 연결되어 있는 port로 frame을 전달하는 기능
 3. Filtering : 프레임이 유입된 port로 다시 frame을 전송하지 않는 기능
 4. Flooding : MAC 주소 테이블에 목적지에 MAC에 대한 정보가 없을 경우 frame을 모든 port로 전송하는 기능 = 테이블에 없는 도착지 주소를 가진 패킷이 들어오면 스위치는 전체 포트로 패킷을 전송한다.

## 네트워크 계층 (L3) ✅ 패킷을 목적지에 전송
- 서로 다른 네트워크에 속한 컴퓨터 사이의 통신. 데이터를 쪼개어 **패킷을 만들어 목적지까지 전달**하는 계층.
- PDU : packet
- Protocol : IP, ICMP (오류 보고)
- 장비 : 라우터 (Router)
- IP주소를 이용해 길을 찾고 (라우팅) 다음 라우터에게 데이터를 넘겨주면서 (포워딩) 목적지 컴퓨터로 데이터를전송함. + 데이터 전송 중 발생하는 **오류**를 검출하고 수정함.

#Router ✅ 최적 경로 선택 
기본적으로 L3 Switch. Internet은 router의 집합체이다.
packet이 router에 도착하면, 이 라우터 간 특정 프로토콜로 통신하며 최적화된 경로를 찾는데, 이 경로 선택의 근거가 되는 것이 routing table ⭐️

#Routing Protocol  ✅ 라우터가 패킷을 식별하고 네트워크 경로를 선택하는 프로세스
- [# 라우팅이란 무엇입니까?](https://aws.amazon.com/ko/what-is/routing/)
- 라우팅 및 스위칭(MPLS, BGP, OSPF, SR, VXLAN 등)                                                  
	-  **OSPF** (Open Shortest Path First) : 여러 경로 중 최소 link cost인 경로를 선택 
	-  [VXLAN](https://atthis.tistory.com/6)  VLAN + X(**eXtensible**) : 기존 VLAN이 제공할 수 있는 규모 이상으로 네트워크 segmentation을 수행하는 데 사용된다. 
	-  **SR** (Segment Routing) : 라우터가 packet header를 알아서 읽어서, 이를 목적지까지 전송 
	-  **BGP** (Border Gateway Protocol) : 유일한 외부 Gateway 프로토콜. 규모가 큰 망을 지원
	- **MPLS** (Multi Protocol Label Switching) : IP 주소가 아닌 레이블을 사용하여 네트워크 트래픽을 라우팅하는 기술 


## 전송 계층 (L4) ✅ 신뢰성 있는 데이터 전송을 보장
- IP와 port 번호를 이용하여 **프로세스**( #process )와 통신하는 계층
- TCP, UDP

## 세션 계층 (L5) ✅ 세션 설정/유지/종료
- packet 전달의 개념에서 나아가 시스템, 프로세스 간 연결 개념으로 나아간다.
- 호스트 간에 데이터 전송 순서를 제어, 중단된 세션을 재개 (**대화제어**, **동기화**)

## 표현 계층 (L6) ✅ 데이터를 특정 형식으로 변환
- 응용 계층으로 보낼 **데이터 번역기**의 역할
- 세션 계층에서 받은 **데이터를 암호/복호화, 압축, 구문 변환, 그래픽 명령어 해석** 하여 응용계층으로 보낸다.

## 응용 계층 (L7) ✅ 네트워크 서비스를 사용자에게 제공하는 계층
- User가 네트워크에 접근할 수 있는 인터페이스와 서비스를 제공한다.
- Protocol : FTP(파일 전송), SMTP(이메일), HTTP (웹 브라우징), DNS 

*GSLB* (Global Server Load Balancing)는 전 세계에 분산된 서버나 데이터 센터 간에 트래픽을 분산시키기 위해 DNS(Domain Name System)를 사용하는 로드 밸런싱 기술입니다. 이는 대규모 웹 서비스나 애플리케이션에서 성능 향상, 가용성 보장, 장애 복구 등을 목적으로 사용됩니다.



### Ref
---
- [# [10분 테코톡] 🔮 히히의 OSI 7 Layer](https://www.youtube.com/watch?v=1pfTxp25MA8)
- [네트워크 관리사 2급 필기 정리](https://devbirdfeet.tistory.com/292)