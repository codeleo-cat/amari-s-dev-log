---
created: 2024-01-28T11:36
updated: 2024-03-03T15:29
---
- Internet Protocol Suite (컴퓨터들이 서로 정보를 주고받는 데 쓰이는 **프로토콜의 집합**)를 4계층으로 설명한 것. <br/> TCP/IP가 가장 많이 쓰여 TCP/IP Protocol Suite라고도 불린다.
- [[OSI 7 Layer]]보다 먼저 나온 규격. 현재 더 많이 사용된다.

|  | 계층 | 역할 | 프로토콜 | 기기 | PDU |
| ---- | ---- | ---- | ---- | ---- | ---- |
| **L4** | 응용 계층 | 컨텐츠를 어떤 형식으로 전송하나? | #HTTP, FTP, SMTP, SSH, DNS, SNMP | L7 스위치 | Message (Data) |
| L3 | 전송 계층 | 데이터를 어떻게 전송하나? | [TCP](https://developer.mozilla.org/ko/docs/Glossary/TCP), UDP | 라우터 | Segment |
| L2 | 인터넷 계층 | 전화선의 주소를 어떻게 설정해야 하는가? | IP, ARP, RARP | 브릿지 | Packet |
| L1 | 네트워크 액세스 계층 | 물리적으로 전화선을 어떻게 구성해야 하나? | Ethernet, wifi | 리피터 | Bits, Frame |
- LDAP (**Lightweight** Directory Access Protocol) 
네트워크 상에서 조직이나 개인, 파일, 디바이스 등을 찾아볼 수 있게 해주는 s/w 프로토콜<br/> 30년 넘은 프로토콜로 주로 AD(Active Directory와 비교됨.)

- TCP (**T**ransmission **C**ontrol **P**rotocol)
전송 제어 프로토콜로, 데이터와 패킷이 보내진 순서대로 전달하는 것을 보장한다. 
송수신되는 데이터의 에러를 제어함으로서 신뢰성있는 데이터 전송을 보장.
#TCP #3-way-handshake
송수신 호스트가 연결을 설정하고, 데이터를 주고받은 후 연결을 해제하는 과정

- Repeater(리피터) - 감쇠된 신호를 재생시키기 위한 목적으로 사용되는 네트워크 장치

*Ref
- [TCP/IP 프로토콜](https://inten.tistory.com/entry/TCP-IP-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
- [[실기-요약]]