---
created: 2024-02-01T09:57
updated: 2024-02-01T13:56
---
#kubernetes 
## **쿠버네티스 개요**

- Kubernetes (K'와 's' 사이의 문자 수를 나타내는 8을 사용하여 K8s로 줄여 쓰기도 한다.) 조타수를 의미하며 2014년 Google에서 Open source로 release되어 현재 CNCF에 소속된 오픈소스이다.
- **==여러 서버의 자원을 클러스터링해 컨테이너를 배치하는 것==**이 K8s의 핵심 기능.
- open source orchestration system (컨테이너를 쉽고 빠르게 배포, 관리해주는 Tool) 로, 컨테이너 기반의 서비스 운영에 필요한 오케스트레이션 기능을 지원한다. 

## **쿠버네티스 구조**

**Object : 쿠버네티스 시스템에서 영속성을 가지는 Object**

- #### **Pods** : Container의 집합
    - 컨테이너 application의 기본 단위. 
    - 포드에 정의된 여러 개의 컨테이너는 하나의 완전한 애플리케이션으로 동작함.
        
- #### **Replica Set** : Pods를 관리하는 컨트롤러
    - 일정 개수의 포드를 유지하는 컨트롤러
    - Creating new pods, Autoscaling of pods

- #### **Deployment** : Replica Set, Pod의 배포를 관리
    ![deployment | 500](https://blog.kakaocdn.net/dn/cLoH0P/btq7WVyob2b/MoxhDlL08W61ktDs70cVkk/img.png)
    - Replica Set의 상위 오브젝트
        - Why Replica Set 있는데 deployment를 쓰나? → 애플리케이션의 업데이트와 배포를 더욱 편하게 만들기 위해서. (for rollback, rolling update)
    
- #### **Service** : Pod를 연결하고 외부에 노출
    - K8s에서는 Pod에 접근하도록 어떻게 정의하나?
        - docker의 경우, -p 옵션을 사용하면 컨테이너를 외부로 노출할 수 있다. (= container 생성과 동시에 외부로 노출되는 방식)
    - **Cluster IP type** : 쿠버네티스 내부에서만 접근 가능. default ![](https://i.stack.imgur.com/48cdO.png)

    - **Node Port type** : 클러스터 외부에서도 접근 가능 (모든 Node의 특정 Port를 개방해 서비스에 접근. Cluster IP 의 기능 포함![](https://i.stack.imgur.com/tTesc.png)
    - **Load Balancer type** : 서비스 생성과 동시에 로드 밸런서를 새롭게 생성해 포드와 연결 → 모든 worker node는 포드에 접근할 수 있는 random port를 개방한다. 
	- Cluster IP의 기능을 포함
	- NodePort의 간접적인 기능을 자동으로 사용 가능하다. ![](https://i.stack.imgur.com/LiUCz.png)
	- **External Name type** : 서비스가 외부 도메인을 가리키도록 설정 가능. (외부 시스템과의 연동이 필요할 때 사용.)
	 ![external name | 500](https://velog.velcdn.com/images/sororiri/post/2fc9320a-63f0-43ac-b291-7ef75701dc0d/image.png)

## **쿠버네티스 Node**
### **Master Node & Worker Node** 로 구성.

==**Master Node**== 🔑 컨테이너 선단을 지휘하는 통제함  
→ 주요 컨트롤 유닛으로서 Worker Nodes를 관리하는 주체

- **APIServer** - 쿠버네티스 [컨트롤 플레인](https://kubernetes.io/ko/docs/reference/glossary/?all=true#term-control-plane)의 핵심. 최종 사용자, 클러스터의 다른 부분 그리고 외부 컴포넌트가 서로 통신할 수 있도록 HTTP API를 제공한다.
- **Scheduler** - **스케줄링**. pod를 적절한 worker node에 배포(할당)하는 component.  
    즉, 새로운 컨테이너가 어느 노드에서 실행될 지 결정하는 component.
- **Controller Manager (CM)** - **컨트롤러 관리**. node의 상태 모니터링(직접 하는 것은 아니고 API Server로부터 state 값을 전달받음), 로그 확인 가능
    - Node Controller
    - Replication Controller
    - Deployment Controller
- **ETCD** - 설정 관리, 서비스 discovery, 스케줄링 등을 위한 데이터를 저장하는 **저장소** 역할 (key-value)

==**Worker Node**== 🔑 파드가 배포 및 실행되는 노드. 파드를 생성하고 관리하는 실질적인 주체
&rarr; 할당된 task를 요청대로 수행하며, master node와 통신하는 시스템
- **kubelet** - 각 노드에서 실행되는 일종의 **agent**. 
- **kube-proxy** - 각 노드에서 실행되는 네트워크 프록시. 
	&rarr; 지속적으로 service, pod의 변경 사항을 확인한다.
	&rarr; worker node로 유입되는 트래픽을 적절한 pod로 라우팅한다.
- **container runtime** - pod에서 실행될 컨테이너 엔진. containerd

## **쿠버네티스 Deployment**

- ==**Rolling update**== - 정해진 비율만큼의 파드만 **점진적**으로 배포. 최소한의 오버헤드와 다운타임  
    ![](https://blog.kakaocdn.net/dn/cr1u0N/btrcHEqx8a9/cCSOcRVlKzBpc89rWjkWY0/img.png)
- ==**Blue/Green**== - ver 1.0 과 ver 2.0 을 구성하고, 트래픽을 ver 2.0 쪽으로 **일제히** 전환하는 방식  
    구 버전(Blue)과 동일한 환경에 신 버전(Green)의 배포를 전부 구축하고, LB를 수정하여 Green 을 한꺼번에 가리키게 한다.  
    _단점_ - 잠시라도 신 버전을 완전히 새 환경에서 구축해야 되기에 일시적이라도 시스템 자원 2배 필요![](https://blog.kakaocdn.net/dn/6Q47k/btrcEv9a1m0/jfXRvfafjlXPYunkKyuink/img.png)
- ==**Canary**== - ver 2.0을 일부 배포하고, 트래픽도 일부만 ver 2.0으로 전환한다. **배포에 문제가 없다면** ver 2.0을 점진적으로 배포 및 트래픽을 전환한다.![](https://blog.kakaocdn.net/dn/ektrH9/btrcGkMSpor/iiPKVFeVexp4TV92nNAtDk/img.png)

---
- ### Ref
- [쿠버네티스란 무엇인가-공식docs](https://kubernetes.io/ko/docs/concepts/overview/)
- [쿠버네티스 네트워킹 이해하기#1: Pods](https://coffeewhale.com/k8s/network/2019/04/19/k8s-network-01/)
- [클라우드 시장의 대세, 쿠버네티스란 무엇인가?](https://www.youtube.com/watch?v=JNc11rxLtmE)
- [책-그림과 실습으로 배우는 도커 & 쿠버네티스](https://www.yes24.com/Product/Goods/108431011)
- [15단계로 배우는 도커와 쿠버네티스](app://obsidian.md/15%EB%8B%A8%EA%B3%84%EB%A1%9C%20%EB%B0%B0%EC%9A%B0%EB%8A%94%20%EB%8F%84%EC%BB%A4%EC%99%80%20%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4)
- [예제로 배우는 쿠버네티스](https://essem-dev.medium.com/%EC%98%88%EC%A0%9C%EB%A1%9C-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-4b9751b23962)
- [End to end devops](https://blog.devops.dev/end-to-end-devsecops-kubernetes-project-4259f90722ef)
- [Service Type Comparison](https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types)
- [K8s deployment update](https://nearhome.tistory.com/106)