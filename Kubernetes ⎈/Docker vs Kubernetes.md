---
created: 2024-03-18T12:09
updated: 2024-03-18T12:12
---
#kubernetes #docker 🗓 *updated at* 2024/04/15

Docker는 컨테이너화 런타임.
컨테이너화된 앱을 패키징하고 배포하는 효율적인 방법을 제공하지만
Docker만으로는 **대규모로 컨테이너를 실행하고 관리하기 어렵다**.

ex)
- 여러 서버에서 컨테이너 조정 및 예약
- 가동 중지 시간 없이 앱 배포

---

이를 해결하기 위한 컨테이너 Orchestration 솔루션이 Kubernetes.
- **container 집합을 pod로 grouping**하여 네트워크 overhead를 줄이고 resource 사용 효율을 높임.
	ex) App server, Redis Cash, SQL DB
- **운영 자동화**
	ex) LB (부하 분산), resource 격리 및 할당, pod의 scale up / out
- **뛰어난 확장성과 유연성**

## **_Why K8s ?_**

- 컨테이너가 수가 증가할수록 그 환경이 복잡해진다.
- 쿠버네티스는 컨테이너를 pod로 분류해서 이 문제를 해결한다.
- [Docker](app://obsidian.md/Docker)는 **단일 호스트**에서 동작하는 컨테이너 실행 및 관리를 위한 도구라면 <br/> K8s는 ==여러 호스트 및 클러스터 환경에서 컨테이너화된 애플리케이션을 자동으로 배포하고 운영==하는 데에 중점을 둔다는 점이 차이점이다.
- 이 둘은 각각의 강점을 가지고 있어, 적절한 상황에서 조합하여 사용하는 것이 일반적인 모던 컨테이너 기반 애플리케이션 개발 및 운영의 접근 방식이다.

---
### Ref
- [Kubernetes와 Docker의 주요 차이점 및 컨테이너화에서 해당하는 부분](https://www.atlassian.com/ko/microservices/microservices-architecture/kubernetes-vs-docker)
