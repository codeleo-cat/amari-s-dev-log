**Master Node**에는 4개의 구성요소가 있다. (**C.A.S.E**)
c-m (kube-controller) - cluster의 상태를 지속적으로 **모니터링**
api server 👑 - cluster의 관문 역할을 하며, **모든 요청을 처리**
scheduler - cluster의 리소스 사용량, 파드의 요구 사항, 노드의 상태 등을 고려하여 가장 적합한 노드를 **선택**
etcd - cluster의 상태정보를 **저장**.

**Worker Node**에는
kubelet
kube-proxy

![](https://kubernetes.io/images/docs/kubernetes-cluster-architecture.svg)

### kubectl command -> k run `sample pod` 하면 생기는 일

1. 사용자가 kubectl run samplepod 명령어를 입력 -> kubectl client가 시작되며 해당 요청이 API 서버로 전송된다.
---
(Master Node)
2. API 서버는 요청을 수신하고, 이를 etcd 데이터 저장소에 기록함. 이 시점에서 Pod의 생성 요청이 공식적으로 클러스터에 등록됩니다. (**API 서버에 요청이 수신되고 etcd에 저장되는 시점**)
3. Controller Manager는 etcd에 저장된 Pod 생성 요청을 감지하고, Scheduler에게 이를 알립니다.
4. Scheduler는 클러스터 내의 적절한 노드를 선택하여 Pod를 할당합니다.
---
(Worker Node)
5. 선택된 노드의 Kubelet은 API 서버로부터 해당 Pod의 생성 명령을 받고, 실제로 노드에서 Pod를 생성하여 실행한다. (by container runtimes such as [containerd](https://containerd.io/docs/), [CRI-O](https://cri-o.io/#what-is-cri-o), and any other implementation of the [Kubernetes CRI (Container Runtime Interface)](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md).)

Pod가 생성되는 최초의 시점은 API 서버가 요청을 수신하고 etcd에 기록할 때이며, 
Pod가 실제로 존재하고 동작하기 시작하는 시점은 Kubelet이 이를 노드에서 생성하여 실행할 때입니다.

