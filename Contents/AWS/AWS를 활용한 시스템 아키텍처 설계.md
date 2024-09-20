---
created: 2024-02-18T14:23
updated: 2024-03-13T21:10
---
[프리온보딩 백엔드 챌린지 3월](https://www.wanted.co.kr/events/pre_challenge_be_17)

### 사전질문
도커(docker)에 대해 설명해주세요
- 도커는 **컨테이너 기술 기반**의 오픈 소스 플랫폼으로, **application을** 가상화된 환경인 **컨테이너에 패키징하여 실행할 수 있게 해주는 기술**입니다. 각 컨테이너는 Application과 이에 필요한 Dependency를 포함하며 호스트 시스템과는 독립적으로 동작합니다.

도커(docker)를 활용하면 어떤 장점이 있나요?
- 도커 컨테이너는 어디서든 동일한 환경에서 실행될 수 있고, 가볍고, (컨테이너는 빠르게 시작되고 중지될 수 있기에) 배포 및 확장성이 뛰어납니다.

서버리스( #serverless)에 대해 설명해주세요
- 애플리케이션 개발 및 운영을 서버에서 직접하지 않고, 함수와 이벤트 중심의 컴퓨팅을 제공하는 것을 뜻합니다. 작성한 코드를 특정 이벤트에 바인딩하여 CSP가 이벤트가 발생될 때마다 필요한 컴퓨팅 리소스를 동적으로 할당하고 실행합니다.
- ex) AWS Lambda, Dynamo DB, AKS, Azure Functions, Cloud Functions

서버리스(serverless)를 활용하면 어떤 장점이 있나요?
- **on-demand** 방식이므로 비용 절감을 할 수 있고, auto-scaling이 적용되면 **트래픽 변동**에도 대응하기 쉽습니다. 또 서버 관리를 CSP에 위임하므로 **인프라 운영 및 관리에 대한 부담이 줄어듭니다**.

서비스를 배포한 경험에 대해 자유롭게 작성해주세요
- Your answer - Azure container app & AWS ECS 사용해서 실 서비스를 배포한 적이 있습니다.

AWS를 비롯한 클라우드 서비스의 장점은 무엇인가요?
- GPT's - 사용한 만큼만 요금을 지불하는 방식인 on-demand 방식을 사용하면 비용 절감에도 도움이 될 수 있고 많은 자동화된 서비스와 편리한 관리도구를 제공합니다. ex) DB, Storage, ML, Security, Data Back up etc... 그렇게 된다면 리소스 구축 뿐 아니라 관리도 용이해집니다. 

---
##### Week 1-1 
##### scaling을 고려한 아키텍처 설계

AWS EC2, AWS RDS, AWS ElasticCache, AWS ALB, AWS CloudFront, AWS DynamoDB, AWS Lambda  
- RDS를 활용한 데이터베이스 scaling  
- ALB + EC2를 활용한 서버 scaling  
- Serverless 활용 시 장단점

보통 시작은 RDB, 데이터 규모가 크면 NoSQL을 쓴다. ex) 네카라쿠배
RDB쓰면 천만 row만 되어도 full scan 때리면 15분 이상 걸리기도 한다.
index도 잘 걸어야 하고, 데이터 양이 많아지고 join 테이블 조회할 양이 많아지면 확실히 속도가 느리다.
그래서 현업에서는 **read에 NoSQL을 쓰고 write에 RDB**를 쓰는 경우가 종종 있다.

instagram에서 쓰는 카산드라 DB. multi-write가 가능하다. write를 분산시켜서 단일 write server가 죽는 걸 대비한다.
 

---
##### Week 1-2
##### SNS 뉴스피드 피드 서비스 설계

AWS SQS, AWS SNS, AWS Neptune, AWS Aurora, AWS ElasticCache, AWS CloudFront  
- Message Queue를 활용한 느슨한 결합이란? [[Kafka]]
- 서비스 운영 방식에 맞는 데이터 스키마 설계와 데이터베이스 선택  
- 올바른 ElasticCache 활용법

----
##### Week 1-3

Polling이 필요한 http 통신 VS web socket 통신

---

추천 영상은 cache 되어 있을 가능성이 높다.

**transcoding** : 원본 영상을 업로드 &rarr; 화질별로 나눠서 저장
Lambda는 15분 timeout이 존재하므로, SQS나 SNS를 추천한다.

DAG : https://en.wikipedia.org/wiki/Directed_acyclic_graph

pre-signed url
- client에서 s3로 바로 업로드
- server에 부하가 없으므로 대규모 서비스에서 사용할 때 합리적, 일반적으로 많이 사용됨.