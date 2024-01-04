#kubernetes 
# Kubernetes(K8s)

- open source orchestration system (컨테이너를 자동으로 관리해주는 Tool)
- ![K8s 기본구조]()
	- 개발자 kubecli -----> kube-api server----> worker node에 **kubelet** 명령 ---> Pod 관리
	- kubelet(각 컨테이너의 실행을 보장하는 서비스) - 지정된 컨테이너를 실행하도록 Docker에 명령

## Master Node & Worker Node

- **Master Node** - 주요 컨트롤 유닛으로서 Worker Nodes를 관리하는 주체
- kubectl - 쿠버네티스 CLI
- ETCD - 설정 관리, 서비스 discovery, 스케줄링 등을 위한 데이터를 저장하는 저장소 역할
----
- **Worker Node** - 할당된 task를 요청대로 수행하며, master node와 통신하는 시스템
- kubelet - 각 노드에서 실행되는 일종의 agent. docker api를 이용하여 docker demon과 통신을 통해 컨테이너 실행
- kube-proxy - 각 노드에서 실행되는 네트워크 프록시. 지속적으로 service, pod의 변경 사항을 확인한다.

## Why K8s ?

- 컨테이너가 수가 증가할수록 그 환경이 복잡해진다.
- 쿠버네티스는 컨테이너를 pod로 분류해서 이 문제를 해결한다.
- Docker는 **단일 호스트**에서 동작하는 컨테이너 실행 및 관리를 위한 도구라면 K8s는 **여러 호스트** 및 클러스터 환경에서 컨테이너화된 애플리케이션을 자동으로 배포하고 운영하는 데에 중점을 둔다는 점이 차이점이다. 이 둘은 각각의 강점을 가지고 있어, 적절한 상황에서 조합하여 사용하는 것이 일반적인 모던 컨테이너 기반 애플리케이션 개발 및 운영의 접근 방식이다.

## Pods

- container를 실행하기 위한 object
- K8s의 가장 기본이 되는 단위
- 하나의 pod 안에 여러 개의 컨테이너가 들어갈 수 있다.
- 동적으로 생성되고, 장애가 생기면 자동 restart되면서 IP가 변경됨.

![pod](https://i0.wp.com/bespin-wordpress-bucket.s3.ap-northeast-2.amazonaws.com/wp-content/uploads/2022/06/%EA%B7%B8%EB%A6%BC62.png?resize=378%2C301&ssl=1)

## [[Service]]

- 요청 트래픽을 지정된 파드로 전송한다.
- 서로 다른 Pod가 동일한 서버에 있든, 다른 서버에 위치하든 상관 없이 통신할 수 있게 한다.

## Namespace

- 쿠버네티스 클러스터 내의 논리적인 분리 단위
- 컨테이너가 하나의 독립된 서버와 같이 동작할 수 있게 한다.

## Ingress Controller

- 들어오는 요청을 적절한 서비스로 전달해야 하는 역할

## Container 교체가 필요한데?

- **Rolling update** 


## Minikube

- Mini + kube
- 쿠버네티스 클러스터 구축을 간단하게 할 수 있도록 만들어주기 위해서 시작된 프로젝트


---
## Ref

- [쿠버네티스 네트워킹 이해하기#1: Pods](https://coffeewhale.com/k8s/network/2019/04/19/k8s-network-01/)
- [클라우드 시장의 대세, 쿠버네티스란 무엇인가?]( https://www.youtube.com/watch?v=JNc11rxLtmE)
- [책-그림과 실습으로 배우는 도커 & 쿠버네티스](https://www.yes24.com/Product/Goods/108431011)
- [[15단계로 배우는 도커와 쿠버네티스]]
