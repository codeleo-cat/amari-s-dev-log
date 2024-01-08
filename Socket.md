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

- Internet Protocol Suite (컴퓨터들이 서로 정보를 주고받는 데 쓰이는 프로토콜의 집합)를 4계층으로 설명한 것

|  | 계층  | 역할 | 프로토콜 | 기기 | PDU |
| ---- | ---- | ---- | ---- | ---- | ---- |
| **L4** | 응용 계층 | 컨텐츠를 어떤 형식으로 전송하나? | HTTP, FTP, SMTP, SSH, DNS | L7 스위치 | Message (Data) |
| L3 | 전송 계층 | 데이터를 어떻게 전송하나? | TCP, UDP | 라우터 | Segment |
| L2 | 인터넷 계층 | 전화선의 주소를 어떻게 설정해야 하는가? | IP, ARP | 브릿지 | Packet |
| L1 | 네트워크 액세스 계층 | 물리적으로 전화선을 어떻게 구성해야 하나? | Ethernet, wifi | 리피터 | Bits, Frame |
### OSI 7 Layer


## Ref

- [socket 기초 1](https://devocean.sk.com/blog/techBoardDetail.do?ID=165560&boardType=techBlog&searchData=&page=&subIndex=%EC%B5%9C%EC%8B%A0+%EA%B8%B0%EC%88%A0+%EB%B8%94%EB%A1%9C%EA%B7%B8)
- [# [10분 테코톡] 🔮 히히의 OSI 7 Layer](https://www.youtube.com/watch?v=1pfTxp25MA8
)
