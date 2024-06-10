---
created: 2024-01-02T16:10
updated: 2024-03-18T11:48
---
#kubernetes 

- updated 📅 2024-06-09 

## K8s Runtime
&rarr; 여러 충돌 문제 등이 존재하여 더이상 docker가 K8s 런타임이 아니게 됨. 
- 현재는 **containerd** 가 사용되고 있음.
![현재 기본 k8s 구조](https://lh6.googleusercontent.com/4NGAPzwhkL0GTNjkAEFN9iWX_Wc0ZE-AZxAxEw4E5aOntuGmv764b3ZYQUyapSnP9BrlUs2rUyo5kiCrj5QuiMHw3-dz2vPUDma029Qt3tej9QABEHFSsOBsq6LjLfFhTBgMhAAc)

- 개발자 kubectl ---> Api server ---> worker node에 **kubelet** 명령 --->  Pod 관리
- kubelet(각 컨테이너의 실행을 보장하는 서비스) - 지정된 컨테이너를 실행하도록 Docker에 명령

---
## [[Object]]

- 쿠버네티스 시스템에서 영속성을 가지는 Object
- 포드(Pod), 레플리카셋(Replica Set), 서비스([[Service]]), 디플로이먼트(Deployment)
 
### Pod
- container를 실행하기 위한 object
- K8s의 **가장 기본이** 되는 단위
- 하나의 pod 안에 여러 개의 컨테이너가 들어갈 수 있다.
- 동적으로 생성되고, 장애가 생기면 자동 restart되면서 IP가 변경됨.

![pod](https://i0.wp.com/bespin-wordpress-bucket.s3.ap-northeast-2.amazonaws.com/wp-content/uploads/2022/06/%EA%B7%B8%EB%A6%BC62.png?resize=378%2C301&ssl=1)
### Service

- 요청 트래픽을 지정된 파드로 **전송**한다.
- 서로 다른 Pod가 동일한 서버에 있든, 다른 서버에 위치하든 상관 없이 통신할 수 있게 한다.

### ReplicaSet

-  Container의 집합(Pods)를 관리하는 **컨트롤러**
- 정해진 개수의 pod를 유지해주는 도구.
- 이 replica set이 삭제 되지 않아 pod를 지워도 다시 살아나는 문제가 발생할 수 있다.

### Namespace

- 🔑 K8s 객체 들을 **격리**해주는 공간
- 쿠버네티스 클러스터( #Cluster; 쿠버네티스가 구성된 환경) 내의 논리적인 분리 단위
- 컨테이너가 하나의 독립된 서버와 같이 동작할 수 있게 한다.
### Label
- Pod를 포함한 각종 K8s Object를 관리하기 위한 = 태그와 유사하다.
### DaemonSet
- 모든 Node 또는 특정 label을 가진 node에 하나씩의 동일한 pod를 배포해주는 resource
### StatefulSet
- ==Pod의 **상태**==를 저장하고 관리하는 Resource 
- Deployment와 거의 동일한 특성을 갖지만, 각 Pod의 순서와 고유성을 보장한다. - pod마다 각각 다른 storage를 사용해 각각 다른 state를 유지한다.
- Stateless Application - APACHE & NGINX
- Stateful Application - mongoDB & redis
	
### Ingress Controller
- 들어오는 요청을 적절한 서비스로 전달해야 하는 역할
- L7
- url 기반 routing

### Pattern
- [K8s Pod 구성 패턴](https://rection34.tistory.com/137) | [K8s Multi Container Design Pattern](https://waspro.tistory.com/775)
- **Side Car pattern**
	기본 컨테이너의 기능을 확장 or 보조하는 용도의 컨테이너를 **추가**하는 패턴
	ex) 로깅 목적의 컨테이너를 따로 side car로 추가한다.
	![300](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FccgdcF%2FbtrFT2YqujL%2FmiN0IvhlfJRNJKOKG01B0K%2Fimg.png)
- Ambassador pattern
	파드 안에서 #proxy 역할을 하는 컨테이너를 추가하는 패턴
	![300](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F72sMC%2FbtrFTOF7GrA%2FJGmTNSHx6qm7KsPd0kf191%2Fimg.png)
- Adapter pattern
	파드 외부로 노출되는 정보를 **표준화**하는 Adapter 컨테이너를 사용하는 패턴
	(adatper container를 통해 데이터를 변환)
	![300](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbNncKa%2FbtrFVULU9Bk%2F125IN14ld61lO0SsF9XR3k%2Fimg.png)

### Auto Scaling

- HPA (Horizontal Pod Autoscaler)
- VPA (Vertical Pod Autoscaler)
- Cluster Autoscaler
- ##### AWS Fargate와의 비교
	- Fargate는 serverless 컴퓨팅 환경. K8s에서는 노드 관리가 필요하지만 Fargate에서는 필요 없음. 
	- K8s에서는 노드 기반의 스케일링이 필요 VS Fargate에서는 컨테이너 기반의 스케일링만 필요

### Container 교체가 필요하다면? #Deployment

- **Rolling update** - 정해진 비율만큼의 파드만 점진적으로 배포
	최소한의 오버헤드와 다운타임
- **Blue/Green** - ver 1.0 과 ver 2.0 을 구성하고, 트래픽을 ver 2.0 쪽으로 전환하는 방식
	구 버전(Blue)과 동일한 환경에 신 버전(Green)의 배포를 전부 구축하고, LB를 수정하여 Green 을 한꺼번에 가리키게 한다.
	 *단점* - 잠시라도 신 버전을 완전히 새 환경에서 구축해야 되기에 일시적이라도 시스템 자원 2배 필요
- **Canary** - ver 2.0을 일부 배포하고, 트래픽도 일부만 ver 2.0으로 전환한다. 배포에 문제가 없다면 ver 2.0을 점진적으로 배포 및 트래픽을 전환한다.

### Managed By CSP
- AWS - EKS (Amazon Elastic Kubernetes Service)
- Azure - AKS (Azure Kubernetes Service)
- GCP - [[GKE]] (Google kubernetes Engine)

### Minikube
- Mini + kube
- 쿠버네티스 클러스터 구축을 간단하게 할 수 있도록 만들어주기 위해서 시작된 프로젝트
- [[ArgoCD]]
### Kubectl (CLI)
- kubectl create ns `namespace`
- kubectl delete ns `namespace`
- kubectl get po `pod`
- kubectl delete po `pod`
- kubectl edit po `pod`
- kubectl describe deploy `deployment`

---
## Ref

- [쿠버네티스란 무엇인가-공식docs](https://kubernetes.io/ko/docs/concepts/overview/)
- [쿠버네티스 네트워킹 이해하기#1: Pods](https://coffeewhale.com/k8s/network/2019/04/19/k8s-network-01/)
- [클라우드 시장의 대세, 쿠버네티스란 무엇인가?]( https://www.youtube.com/watch?v=JNc11rxLtmE)
- [책-그림과 실습으로 배우는 도커 & 쿠버네티스](https://www.yes24.com/Product/Goods/108431011)
- [[15단계로 배우는 도커와 쿠버네티스]]
- [예제로 배우는 쿠버네티스](https://essem-dev.medium.com/%EC%98%88%EC%A0%9C%EB%A1%9C-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-4b9751b23962)
- [End to end devops](https://blog.devops.dev/end-to-end-devsecops-kubernetes-project-4259f90722ef)
- [Service Type Comparison](https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types)