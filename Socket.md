# Socket
- OS에 내장된 프로그램
- **L4 계층**에서 그 아래 계층에 접근하기 위한 인터페이스

### Process

- socket
- bind
- accept
- read, recv / write, send
- close

---
### TCP/IP 4 Layer 

- Internet Protocol Suite (컴퓨터들이 서로 정보를 주고받는 데 쓰이는 **프로토콜의 집합**)를 4계층으로 설명한 것
- OSI 7 layer보다 먼저 나온 규격. 현재 더 많이 사용된다.

|  | 계층 | 역할 | 프로토콜 | 기기 | PDU |
| ---- | ---- | ---- | ---- | ---- | ---- |
| **L4** | 응용 계층 | 컨텐츠를 어떤 형식으로 전송하나? | #HTTP, FTP, SMTP, SSH, DNS | L7 스위치 | Message (Data) |
| L3 | 전송 계층 | 데이터를 어떻게 전송하나? | [TCP](https://developer.mozilla.org/ko/docs/Glossary/TCP), UDP | 라우터 | Segment |
| L2 | 인터넷 계층 | 전화선의 주소를 어떻게 설정해야 하는가? | IP, ARP, RARP | 브릿지 | Packet |
| L1 | 네트워크 액세스 계층 | 물리적으로 전화선을 어떻게 구성해야 하나? | Ethernet, wifi | 리피터 | Bits, Frame |
- LDAP (**Lightweight** Directory Access Protocol) 
네트워크 상에서 조직이나 개인, 파일, 디바이스 등을 찾아볼 수 있게 해주는 s/w 프로토콜
- TCP 
전송 제어 프로토콜로, 데이터와 패킷이 보내진 순서대로 전달하는 것을 보장한다. 

### OSI 7 Layer

- **네트워크 통신**이 일어나는 과정을 7단계로 나누어 ISO에서 정의한 네트워크 표준 모델

|  | 계층 | 역할 | 장비/기타 | PDU |
| ---- | ---- | ---- | ---- | ---- |
| L7 | 응용 계층 | 최종 목적지. 직접 응용 서비스 수행 |  |  |
| L6 | 표현 계층 | 전송하는 데이터의 표현 방식 결정 | JPEG, MPEG, GIF, ASCII |  |
| L5 | 세션 계층 | 연결 유지 | API, [[Socket]] |  |
| L4 | 전송 계층 | IP와 Port를 이용하여 Process와 통신. <br/>통신 노드 간 연결 제어. | TCP/UDP |  |
| L3 | 네트워크 계층 | 데이터를 (목적지까지) 어떻게 전송하나? | 라우터 | Packet |
| L2 | 데이터 링크 계층 | L1을 통해 송수신되는 정보의 오류와 흐름 관리 | 브릿지, 스위치, 이더넷 | Frame |
| L1 | 물리 계층 | 통신 케이블로 데이터를 전송하는 물리적인 장비 | 리피터, 허브, 통신 케이블 | Bit |
## Ref

- [socket 기초 1](https://devocean.sk.com/blog/techBoardDetail.do?ID=165560&boardType=techBlog&searchData=&page=&subIndex=%EC%B5%9C%EC%8B%A0+%EA%B8%B0%EC%88%A0+%EB%B8%94%EB%A1%9C%EA%B7%B8)
- [# [10분 테코톡] 🔮 히히의 OSI 7 Layer](https://www.youtube.com/watch?v=1pfTxp25MA8
)
