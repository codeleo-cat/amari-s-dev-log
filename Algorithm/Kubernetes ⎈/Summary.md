---
created: 2024-03-18T12:09
updated: 2024-03-18T12:12
---
#kubernetes 🗓 *updated at* 2024.07.14
### Concept of K8s

**Kubernetes**
> K8s는 **container orchestration** platform 입니다.
> container화된 워크로드를 자동으로 배포, 확장, 관리하며 대규모의 MSA 관리에 필수적. 
> orchestration은 네트워킹, 확장성, 배포 등을 포함한 resource의 auto-managing을 의미합니다.


**Node (노드)** : **host**
컨테이너 runtime을 탑재하고 있고 쿠버네티스 컴포넌트들이 들어있는 **host** 시스템.
클러스터를 제어하고 관리하는 역할의 **master node**와  
실제 앱을 실행하고 관리하는 역할의 **worker node**로 구성된다.  

**Pod (파드)** : **container 집합**
하나 이상의 container를 포함하는 집합. 

**Kubernetes's container**
Application의 dependency, library, 구성 file 등을 모두 **packaging**한 것.

**Service (서비스)** : **통신** 담당
pod 간의 **통신**을 담당하는 추상화된 layer.  
pod에 endpoint를 제공하고, LB 및 Service 검색을 가능하게 한다.
**Cluster IP** : Cluster 내부에서만 접근 가능한 default service type으로, 내부에서만 접근 가능한 IP주소를 할당한다.  
**NodePort** : Cluster의 각 node에서 특정 port를 열어, cluster 외부에서 접근할 수 있도록 한다.  
**LoadBalancer** : LB를 사용하여 외부 트래픽을 서비스로 라우팅한다.  
**External Name** : DNS 이름을 사용하여 외부 서비스로 트래픽을 redirect한다.

**ReplicaSet** : **필요한 수**의 pod replica가 항상 **작동하도록 보장**

**Namespace** : 공간의 **격리**

**ETCD** : cluster data를 저장하는 **저장소(key-value 형태)**

**Taint & Toleration** : Node가 Pod를 **제외/허용**

**Drain & Uncordon** : Node를 비우기/스케쥴링 가능한 상태로 전환


**Headless 서비스**
클러스터 내의 pod들을 **직접 노출**시키는 데 사용된다. LB 기능을 제공하지 않으며 pod의 IP 주소를 직접 반환한다.  
`clusterIP: None`으로 설정한다.  
StatefulSet과 함께 사용된다.

**kube-API server의 기능**
kubernetes API를 노출한다.  
모든 구성 요소가 이를 통해 소통한다.  
REST 작업과 Cluster의 interface를 제공

**kube-scheduler**의 기능
새로 생성된 pod (node가 할당되지 않은 pod)를 감지하고 node를 배정해주는 역할 

| **Kubernetes**             | **Docker Swarm** |
| :------------------------- | :--------------- |
| dash board 형태의 GUI 제공.     | X                |
| 로깅 및 모니터링을 위한 기본 제공 기능이 포함 | X                |
| 설정 어려움.                    | 설치 쉬움.           |
| 강력한 클러스터를 보장               | 안정적인 클러스터 존재 X   |
**API 보안을 위한 Solution**
> API 인증 수단을 통해 (Node / RBAC)
> Kube의 최신 release 사용

#### Reference
- [30개 이상의 Kubernetes 질문](https://hashdork.com/ko/kubernetes-interview-questions/)


### Tools using K8s

**ArgoCD**
K8s용 **Declarative, GitOps CD 도구**.
> 앱 배포를 자동화하여 실시간 상태가 Git 저장소에 저장된 구성과 일치하는지 확인한다.

**Helm**
K8s용 **package manager**.
> 개발/운영에 있어 K8s 클러스터에 앱을 쉽게 패키징, 구성 및 배포할 수 있도록 한다.

**Kustomize**
K8s 자체 구성 관리 기능을 향상시키는 K8s 기반 구성 관리 도구.
> 다양한 환경이나 배포 시나리오와 같이 동일한 애플리케이션에 대해 조금씩 다른 여러 구성을 유지해야 하는 시나리오에 유용하다.

**Prometheus**
open source **모니터링 시스템**.
> metric을 시계열 데이터로 수집하고 저장. 
> K8s 클러스터의 성능 측정 및 앱 상태에 대한 insight를 제공함.

**Istio**
앱 트래픽 관리, 보안 정책 및 세밀한 서비스 모니터링을 가능하게 하는 **service mesh**.
> 코드 변경 없이 서비스를 보다 효과적으로 보호, 연결 및 모니터링할 수 있는 infra layer를 제공한다.

#### Reference
- [K8s Tools](https://overcast.blog/13-kubernetes-tools-your-should-know-in-2024-4e857124c176)
- [[ArgoCD]] [[Helm]] [[Kubernetes 동작]]


### [[Kubernetes가 pod를 실행하면 발생하는 일]]
### [kubernetes가 pod를 삭제하는 법](https://leehosu.github.io/kubernetes-delete-pod)
1. k delete po $po
2. k8s control plane 내의 **api-server**가 Pod 삭제 요청을 받고, 해당 요청을 cluster 내의 모든 control plane 구성 요소에 전달한다.
3. k8s control plane 내의 **kube-controller-manager**는 pod가 ReplicaSet / deployment와 같은 object의 소유 Pod인지 확인한다.
4. api-server가 해당 Node의 kubelet에게 pod 삭제 요청을 전달한다.
5. pod를 삭제하기 전에 pod 내의 컨테이너에 signal을 보내고 일정 시간 대기한다. 
6. pod의 모든 container가 종료되면 kubelet은 pod를 완전히 삭제하고, 상태 update를 kube-api-server로 보낸다.
7. kube-api-server는 pod 삭제 완료를 확인 후, 상태 update 후 요청 클라이언트에 응답한다.

---
Q. kubelet과 docker를 다시 시작하는 방법  
kubelet은 pod와 container의 상태를 관리합니다. 
```sh
sudo systemctl restart kubelet 
sudo systemctl restart docker
```
와 같은 명령어를 날려서 재 시작합니다.

Q. Sidecar pattern을 활용한 log mgt & multi container에 대해 설명해주세요.
1) side car 패턴은 주요 application container를 **보조**하는 역할의 container를 배치하는 패턴입니다. 예를 들어 ( )을 활용해서 application의 로그를 중앙화된 로그 server로 전송할 수 있습니다.
2) multi container pod는 서로 다른 컨테이너가 동일한 pod내에서 실행되며, 다양한 서비스를 함께 제공하거나 보조적인 기능을 수행할 수 있다는 장점을 가집니다. 같은 ns를 공유하고, local host file system을 공유할 수 있습니다.

Q. ETCD는 cluster data를 저장하는 key-value 형식의 저장소이고, ETCD backup을 수행하려면 etcd cluster에서 **snapshot을 생성**하여 back up 파일로 저장하면 됩니다.
```sh
ETCDCTL_API=3 etcdctl snapshot save /path/to/backup.db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/path/to/ca.crt \
  --cert=/path/to/etcd.crt \
  --key=/path/to/etcd.key
```

Q. K8s cluster upgrade 과정을 설명해주세요.
1) Master Node를 upgrade 한 후 
```sh
sudo apt-get update && sudo apt-get install -y kubeadm=1.X.X-00
sudo kubeadm upgrade apply v1.X.X
sudo apt-get install -y kubelet=1.X.X-00 kubectl=1.X.X-00
sudo systemctl restart kubelet
```
2) 각 Worker Node를 upgrade 합니다.
```sh
sudo apt-get update && sudo apt-get install -y kubeadm=1.X.X-00
sudo kubeadm upgrade node
sudo apt-get install -y kubelet=1.X.X-00 kubectl=1.X.X-00
sudo systemctl restart kubelet
```

Q. kubectl의 drain과 uncordon 명령어의 역할과 사용법을 설명해주세요.

drain은 **node를 비우는데** 사용됩니다. 실행 중인 모든 pod를 안전하게 다른 node로 이동시키고, 해당 node를 유지보수 모드로 전환합니다.
```sh
k drain `node name` --ignore-daemonsets
```

uncordon 명령어는 **node를 다시 scheduling 가능 상태로 전환**합니다.
-> Node를 scheduling 하는 것은 kube-scheduler의 역할.
```sh
k uncordon `node name`
```

Q. toleration의 역할과 적용 방법을 설명해주세요.  

toleration은 특정 node에 적용된 ==*taint*==를 pod가 **허용**할 수 있게 해주는 설정입니다. (즉, taint를 무시) 특정 node에만 특정 pod가 scheduling 되도록 제어 가능합니다.  
==*taint*== - node가 pod를 **제외**시킬수 있는 속성.
```yaml
tolerations:
- key: "key1"
  operator: "Equal"
  value: "value1"
  effect: "NoSchedule"
```

Q. deployment를 생성/확장/서비스를 통해 노출시키는 명령어는 무엇인가요?  

kubectl create / scale / expose deploy `deployment name`
```sh
k create deploy `deployment name` --image=`image name`
ex) k create deploy nginx-deployment --image=nginx

k scale deploy `deployment name` --replicas=`replica number`
ex) k scale deploy nginx-deployment --replicas=5

k expose deploy `deployment name` --port=`port` --target-port=`target port` --type=`serivce type`
ex) k expose deploy nginx-deployment --port=80 --target-port=80 --type=NodePort
```

Q. NodePort와 Ingress의 차이점과 사용 예를 설명해 주세요.  

**NodePort**는 클러스터 외부에서 특정 포트로 **접근**할 수 있게 해주는 Service type 입니다.
**Ingress**는 HTTP 및 HTTPS 라우팅을 관리하는 리소스로, 외부 요청을 클러스터 내 서비스로 **라우팅**할 수 있습니다.

Q. Network Policy는 Pod 간의 **네트워크 트래픽을 제어**합니다.