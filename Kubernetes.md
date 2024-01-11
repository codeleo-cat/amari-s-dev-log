# 컨테이너화된 워크로드와 서비스를 관리하기 위한 이식성이 있고, 확장가능한 오픈소스 플랫폼

- open source orchestration system (컨테이너를 쉽고 빠르게 배포, 관리해주는 Tool)
- [[K8s 기본구조_과거]]
	&rarr; 더이상 docker가 K8s 런타임이 아니게 됨. 여러 충돌 문제 등으로

- 현재는 **containerd** 가 사용되고 있음.
![현재 기본 k8s 구조](https://lh6.googleusercontent.com/4NGAPzwhkL0GTNjkAEFN9iWX_Wc0ZE-AZxAxEw4E5aOntuGmv764b3ZYQUyapSnP9BrlUs2rUyo5kiCrj5QuiMHw3-dz2vPUDma029Qt3tej9QABEHFSsOBsq6LjLfFhTBgMhAAc)

- 개발자 kubectl -----> Api server----> worker node에 **kubelet** 명령 ---> Pod 관리
- kubelet(각 컨테이너의 실행을 보장하는 서비스) - 지정된 컨테이너를 실행하도록 Docker에 명령

### Master Node & Worker Node

**Master Node** 🔑 **컨테이너 선단을 지휘하는 통제함** 
&rarr; 주요 컨트롤 유닛으로서 Worker Nodes를 관리하는 주체
- **Scheduler** - **스케줄링**. pod를 적절한 worker node에 배포(할당)하는 component. <br/> 즉, 새로운 컨테이너가 어느 노드에서 실행될 지 결정하는 component.
- **Controller Manager (CM)** - **컨트롤러 관리**. node의 상태 모니터링(직접 하는 것은 아니고 API Server로부터 state 값을 전달받음), 로그 확인 가능
	- Node Controller
	- Replication Controller
	- Deployment Controller
- **ETCD** - 설정 관리, 서비스 discovery, 스케줄링 등을 위한 데이터를 저장하는 **저장소** 역할 (key-value)

**Worker Node** 🔑 **파드가 배포 및 실행되는 노드. 파드를 생성하고 관리하는 실질적인 주체**
&rarr; 할당된 task를 요청대로 수행하며, master node와 통신하는 시스템
- **kubelet** - 각 노드에서 실행되는 일종의 **agent**. 
- **kube-proxy** - 각 노드에서 실행되는 네트워크 프록시. 
	&rarr; 지속적으로 service, pod의 변경 사항을 확인한다.
	&rarr; worker node로 유입되는 트래픽을 적절한 pod로 라우팅한다.
- **container runtime** - pod에서 실행될 컨테이너 엔진. containerd

### Why K8s ?

- 컨테이너가 수가 증가할수록 그 환경이 복잡해진다.
- 쿠버네티스는 컨테이너를 pod로 분류해서 이 문제를 해결한다.
- [[Docker]]는 **단일 호스트**에서 동작하는 컨테이너 실행 및 관리를 위한 도구라면 K8s는 **여러 호스트** 및 클러스터 환경에서 컨테이너화된 애플리케이션을 자동으로 배포하고 운영하는 데에 중점을 둔다는 점이 차이점이다. 이 둘은 각각의 강점을 가지고 있어, 적절한 상황에서 조합하여 사용하는 것이 일반적인 모던 컨테이너 기반 애플리케이션 개발 및 운영의 접근 방식이다.

### [[K8s Object]]

- 쿠버네티스 시스템에서 영속성을 가지는 Object

### Pods

- container를 실행하기 위한 object
- K8s의 가장 기본이 되는 단위
- 하나의 pod 안에 여러 개의 컨테이너가 들어갈 수 있다.
- 동적으로 생성되고, 장애가 생기면 자동 restart되면서 IP가 변경됨.

![pod](https://i0.wp.com/bespin-wordpress-bucket.s3.ap-northeast-2.amazonaws.com/wp-content/uploads/2022/06/%EA%B7%B8%EB%A6%BC62.png?resize=378%2C301&ssl=1)

### [[Service]]

- 요청 트래픽을 지정된 파드로 전송한다.
- 서로 다른 Pod가 동일한 서버에 있든, 다른 서버에 위치하든 상관 없이 통신할 수 있게 한다.
### Namespace
#namespace
- 쿠버네티스 클러스터( #Cluster; 쿠버네티스가 구성된 환경) 내의 논리적인 분리 단위
- 컨테이너가 하나의 독립된 서버와 같이 동작할 수 있게 한다.

### Ingress Controller

- 들어오는 요청을 적절한 서비스로 전달해야 하는 역할

### Auto Scaling

- HPA (Horizontal Pod Autoscaler)
- VPA (Vertical Pod Autoscaler)
- Cluster Autoscaler
- ##### AWS Fargate와의 비교
	- Fargate는 serverless 컴퓨팅 환경. K8s에서는 노드 관리가 필요하지만 Fargate에서는 필요 없음. 
	- K8s에서는 노드 기반의 스케일링이 필요 VS Fargate에서는 컨테이너 기반의 스케일링만 필요


### Container 교체가 필요한데? (배포 방식 3가지)

- **Rolling update** - 정해진 비율만큼의 파드만 점진적으로 배포
	최소한의 오버헤드와 다운타임
- **Blue/Green** - ver 1.0 과 ver 2.0 을 구성하고, 트래픽을 ver 2.0 쪽으로 전환하는 방식
	구버전(Blue)과 동일한 환경에 신버전(Green)의 배포를 전부 구축하고, LB를 수정하여 Green 을 한꺼번에 가리키게 한다.
	 *단점* - 잠시라도 신버전을 완전히 새 환경에서 구축해야 되기에 일시적이라도 시스템 자원 2배 필요
- **Canary** - ver 2.0을 일부 배포하고, 트래픽도 일부만 ver 2.0으로 전환한다. 배포에 문제가 없다면 ver 2.0을 점진적으로 배포 및 트래픽을 전환한다.

### Managed By CSP
- AWS - EKS (Amazon Elastic Kubernetes Service)
- Azure - AKS (Azure Kubernetes Service)
- GCP - [[GKE]] (Google kubernetes Engine)

### Minikube

- Mini + kube
- 쿠버네티스 클러스터 구축을 간단하게 할 수 있도록 만들어주기 위해서 시작된 프로젝트
- [[ArgoCD + K8s 실습]]


---
## Ref

- [쿠버네티스란 무엇인가-공식docs](https://kubernetes.io/ko/docs/concepts/overview/)
- [쿠버네티스 네트워킹 이해하기#1: Pods](https://coffeewhale.com/k8s/network/2019/04/19/k8s-network-01/)
- [클라우드 시장의 대세, 쿠버네티스란 무엇인가?]( https://www.youtube.com/watch?v=JNc11rxLtmE)
- [책-그림과 실습으로 배우는 도커 & 쿠버네티스](https://www.yes24.com/Product/Goods/108431011)
- [[15단계로 배우는 도커와 쿠버네티스]]
