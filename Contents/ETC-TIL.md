# [Coroutine](https://dev.gmarket.com/82) (Co + Routine)
- 비동기적으로 실행되는 코드를 간소화하기 위한 패턴. 
- 코드 양 감소. 
- 스레드 제어를 직접한다.

# Bastion host : **내부 네트워크에 접속할 수 있는 게이트웨이**.
ex) private subnet 내에 있는 DB와 같은 리소스 접근을 위한 입구 역할을 하는 서버. (접근 제어 기능)
= jump host

# Blob (Binary Large Object)
- blob은 대용량의 객체를 가리킨다. ( 이진 데이터를 담고 있는 큰 객체)
- 웹에서 사용되는 데이터 형식으로 txt, png, video 등 다양한 형식을 지원한다.

**Azure Blob vs File storage**
- Azure Files → 단순히 파일을 탑재하고 액세스 (docx, png 와 같은 표준 파일 확장자 처리)
- Azure Blob -> application을 통해 프로그래밍 방식으로 data에 access (구조화되지 않은 데이터 - 오디오, 비디오, 이미지 등)

# gRPC (Google Remote Protocol)
#grpc
원격 프로시저 호출(RPC) 클라이언트-서버 통신 모델을 기반으로 API를 만들고 사용하는 시스템.

- Cloud Native Computing Foundation에서 관리하는 오픈 소스 API 아키텍처 및 시스템
- Data Access - 서비스 (기능)를 직접 호출한다.
- gRPC는 데이터 전송에 Protocol Buffer와 HTTP 2를 사용함.
- REST와 달리, gRPC는 양방향 스트리밍 통신을 제공함.

ref
- [the-difference-between-grpc-and-rest](https://aws.amazon.com/ko/compare/the-difference-between-grpc-and-rest/)

# Kafka - messaging service를 이용한 느슨한 결합.


# PM2 (**P**rocess **M**anager **2**)
Node.js로 만들어진 프로그램을 관리해주는 매니저
기능
- 프로그램이 꺼지면 자동으로 다시 켤 수 있다.
```shell
pm2 start app.js
```
- 코드에 변경상황이 생기면 자동으로 반영된다. 
```shell
pm2 start app.js --watch
```
- 로그 확인
```shell
pm2 log
```
ref
- [Docs-pm2](https://pm2.keymetrics.io/)

# Protobuf
보통 프로그램 하나를 개발할 때 웹과 API서버가 다른 언어로 개발돼요.
그런데 프로토버프를 이용하면 간단하게 언어를 변환할 수 있고 데이터가 가벼워지니 데이터도 빠르게 전달할 수 있어요. 어떻게 사용? .proto라는 확장자를 가진 파일로 데이터 구조를 정의한 뒤 프로토버프 컴파일러를 통해서 사용하고자 하는 언어로 컴파일

**[출처]** [Protobuf, 그건 또 머고?](https://blog.naver.com/markany_idea/223006791365)|**작성자** [마크애니](https://blog.naver.com/markany_idea)

# SDC23 Korea 글로벌 표준 Matter를 활용한 SmartThings Platform 고도화
one - 다양한 프레임워크 지원
NNStreamer - AI 서비스 (무인 매장 운영 솔루션. 3D 프린팅 진행 확인. framework 지원 - tensorflow, pytorch)
sudo code 형식.
Among-Device AI

MLOps - cloud 환경에서 ai 학습 등을 하고 dataset 관리.
Device MLOps의 필요성 - model / application coupled 분리

# Github Actions vs Jenkins 실행 속도
이 두 도구의 성능은 서버의 성능과 환경에 따라 달라진다고 이해하는 게 더 맞는 것으로 보입니다.  
Github Actions의 runner가 self-hosted 환경인 경우(사용자가 직접 서버를 설치해서 사용하는 경우)도 그렇고 Jenkins도 서버의 사양, JVM 설정 등에 따라 그 실행 속도에 차이가 있기 때문입니다.  
(Github Actions의 runner가 github에서 제공하는 [github-hosted-runner](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners)인 경우에는 다를 수 있습니다.

# Blue/Green Deployment 
배포는 두 개의 분리된 동일한 환경을 생성하는 배포 전략입니다. 한 환경(Blue)은 현재 애플리케이션 버전을 실행하고, 다른 환경(Green)은 새 애플리케이션 버전을 실행합니다. Blue/Green 배포 전략을 사용하면 배포가 실패할 경우 롤백 프로세스를 단순화하여 애플리케이션 가용성을 높이고 배포 위험을 줄입니다. Green 환경에 대한 테스트가 완료되면 실시간 애플리케이션 트래픽이 Green 환경으로 전송되고 Blue 환경은 더 이상 사용되지 않습니다.

# Monolithic
하나의 코드 기반을 갖춘 단일의 대규모 컴퓨팅 네트워크 ![monolithic](https://wac-cdn.atlassian.com/dam/jcr:95b9a276-c524-42b1-8d06-ded56d589858/Monolithic%20architecture@2x.png?cdnVersion=1434)
**쉬운 배포** – 하나의 실행 파일 또는 디렉터리로 인해 배포가 더 쉬워집니다.
**개발** – 하나의 코드 기반으로 애플리케이션을 구축하면 개발하기가 더 쉽습니다.
**성능** – 중앙 집중식 코드 베이스 및 리포지토리에서 하나의 API는 수많은 API가 마이크로서비스에서 수행하는 것과 동일한 기능을 수행할 수 있는 경우가 많습니다.

# Micro Service Architecture
주요 비즈니스, 도메인 별 문제를 별도의 독립적인 코드 베이스로 분리 ![MSA](https://wac-cdn.atlassian.com/dam/jcr:5308ccab-dc94-46f5-978c-8a77b8d5be57/Microservice%20architecture@2x.png?cdnVersion=1434)
마이크로서비스 아키텍처는 독립적으로 실행되는 단위로 구성되어 있기 때문에 각 서비스는 다른 서비스에 영향을 미치지 않고 개발, 업데이트, 배포 및 확장할 수 있습니다.

- [마이크로서비스와 모놀리식 아키텍처](https://www.atlassian.com/microservices/microservices-architecture/microservices-vs-monolith#:~:text=A%20monolithic%20application%20is%20built,of%20smaller%2C%20independently%20deployable%20services.)


 [CNCF (**Cloud Native Computing Foundation**)](https://cncf.landscape2.io/?group=projects-and-products)
	- 클라우드 네이티브 기술의 채택을 촉진하는 오픈 소스 소프트웨어 재단
	- stack 분류할 때 참고하면 좋을 것 같음.

# 넷플릭스가 스트리밍에 UDP가 아닌 TCP를 사용하는 이유
시간에 민감하고 포트 포워딩이 필요하지 않다.
더 많은 데이터를 한번에 압축 가능하다.
종단 간 연결을 사용하여 버퍼링을 줄여 비디오 품질을 높인다.
데이터 패킷의 재전송을 통해서 악성 코드나 장애 발생시 오류 복구를 보장.
source 와 destination 간 대역폭을 모니터링하여 그에 따라 스트리밍 프로그램의 **비디오 품질을 조정**한다.

[# 서버 증설 없이 처리하는 대규모 트래픽](https://toss.tech/article/monitoring-traffic)
**Throttling (스로틀링)**
기기의 CPU 등이 지나치게 과열될 때 기기의 손상을 막고자 클럭과 전압을 강제적으로 낮추거나 전원을 꺼서 발열을 줄이는 기능 -> 성능을 강제로 낮춤.

Redis에서 지연이 발생 > Redis를 사용하는 모든 곳에서 지연 발생 > 
해결 - 웹 서버에서 local cache를 사용해서 data를 서버 내에서 전부 캐싱하는 방법
데이터를 압축하면 크키가 작아져서 메모리 사용량 &darr;

**포인트 중복 지급 여부 확인**
- Redis 캐시 시스템에 포인트 적립 내역을 하나의 key에 append 후 db에 적립 내역을 저장.
- 이 과정에서 Throttling을 걸어서 DB 과부하 방지.

**선착순 포인트 지급**
- Redis에서 지원하는 Increment command 활용.

#### Redis - single thread로 동작.



# Error
- [Address already in use (Bind failed) 문제 해결방법](https://reallinux.co.kr/blog/199)
- [kubectl delete ns 안됨](https://ccambo.tistory.com/entry/Kubernetes-Troubleshooting-%EC%82%AD%EC%A0%9C%EB%90%98%EC%A7%80-%EC%95%8A%EB%8A%94-Namespace-%EA%B0%95%EC%A0%9C%EB%A1%9C-%EC%82%AD%EC%A0%9C%ED%95%98%EA%B8%B0)[해결](https://crois.net/k8s-namespace-%EA%B0%95%EC%A0%9C-%EC%82%AD%EC%A0%9C/)
