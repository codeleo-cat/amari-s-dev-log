https://wormwlrm.github.io/2021/01/24/Introducing-WebRTC.html 참조하여 정리


- 여러 대의 컴퓨터가 하나의 공인 IP Address를 공유하는 #NAT
- 유휴 상태의 IP를 일시적으로 임대받아, 이 IP Address를 기타 관련 구성 정보 (ex 서브넷 마스크 등) 를 클라이언트에게 자동으로 제공하는 프로토콜 #DHCP 
- 일반적으로는 #Router 가 NAT 역할을 한다. <br/> 외부에서 접근하는 공인 IP 와 포트 번호를 확인해서 현재 네트워크 내의 사설 IP를 적절히 매핑한다. <br/> 그래서 브라우저 간 통신을 하려면, 라우터의 공인 IP주소와 포트 번호를 알아야 하는데 특정 라우터들은 방화벽 설정이 되어 있기도 하다. <br/> &rarr; 라우터를 통과해서 연결할 방법을 찾는 과정을 **NAT traversal** 이라고 한다.

- STUN 서버 (*Session Traversal Utilities for NAT*)
	단말이 자신의 공인 IP주소와 포트를 확인하는 과정에 대한 프로토콜. 
	자기 자신을 식별할 수 있는 정보를 반환해 준다.

- TURN (*Traversal Using Relay NAT*)
	네트워크 미디어를 중개하는 서버를 이용하는 것.
	STUN 서버를 통해 자기 자신의 주소를 못 찾아냈을 경우, 대안으로 이용한다.

- SNAT (Source NAT) - 출발지 주소를 변경하는 NAT
- DNAT (Destination NAT) - 도착지 주소를 변경하는 NAT

### Signaling

- 간단히 말해, 두 장치의 제어 정보를 교환하는 과정
- 시그널링 서버를 직접 구축한다면
	- 웹 소켓
	- 서버 전송 이벤트
	- 폴링 기법 ( #Polling)