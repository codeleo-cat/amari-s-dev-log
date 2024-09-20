---
created: 2024-01-03T17:12
updated: 2024-01-10T16:04
---
- Web server
- Nginx의 컨테이너는 리눅스 배포판 중 하나인 데비안( #Debian)에 기반하여 만들어졌다.  

## 다양한 웹 서버 중 NGINX를 선택해야 하는 이유
apache server와 다른 점 -> 많은 client 요청 처리에 있어서 Nginx가 더 유리하기 때문이다. (대규모 트래픽)
또한 다음의 기능을 지원한다.
- 로드밸런싱
- reverse proxy
- 캐싱

### Apache Server 
✅ 동적 컨텐츠 자주 처리하는 web application, 소규모 웹 사이트에 적합
- **Process / Thread 기반 architecture**를 사용한다. == 각각의 요청마다 process나 thread를 할당하기 때문에 Memory 부족 / 많은 동시 요청을 처리할 때 CPU 사용량 증가하는 등 구조적 한계가 존재함.
- C10K (Connection 10000개의 문제)
- keep-alive 시간만큼 connection을 유지한다.
- **동적 콘텐츠**(예: PHP, Python, Ruby 등)를 모듈을 통해 자체적으로 처리 가능하다.

### Nginx 
✅ 많은 커넥션 처리, 정적 파일, 간단 설정, 비동기 **Event Driven** architecture
- Since ~ 2004년
- Single thread로 수천~수만에 달하는 동시 connection을 처리할 수 있다.
- 정적 파일에 대한 요청을 스스로 처리 가능하다. (HTML, CSS, JavaScript 등) + 동적 컨텐츠는 다른 백엔드 서버와 통합하여 처리한다.
- 설정 파일이 간단하고 직관적이며, 설정을 실시간으로 수정해도 서버를 재시작할 필요가 없다. reload만 하면 되고 이건 Apache도 마찬가지.

#### Nginx 구성
master process 와 worker process로 구성됨.
시간이 오래 걸리는 작업은 따로 수행한다. Thread Pool에 그 task를 위임한다.

https://namu.wiki/w/NGINX