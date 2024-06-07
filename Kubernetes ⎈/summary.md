---
created: 2024-03-18T12:09
updated: 2024-03-18T12:12
---
#kubernetes 🗓 *updated at* 2024/04/15

- Kubernetes
> container orchestration platform.
> container화된 워크로드를 자동으로 배포, 확장, 관리할 수 있으며, 대규모의 Micro Service Architecture 관리에 필수적이다.

- node
> 컨테이너 런타임을 탑재하고 있고 쿠버네티스 컴포넌트들이 들어있는 host 시스템.
> 클러스터를 제어하고 관리하는 역할의 **master node** 와 <br/> 
> 실제 앱을 실행하고 관리하는 역할의 **worker node**로 구성된다.

- pod가 다른 pod와 어떻게 통신하나?
> **service**라는 추상화된 layer를 통해서 통신한다.


[K8s Tools](https://overcast.blog/13-kubernetes-tools-your-should-know-in-2024-4e857124c176)
back links  [[ArgoCD]] [[Helm]] [[Kubernetes 동작]]

- ArgoCD
> K8s용 선언적, GitOps CD 도구.
> 앱 배포를 자동화하여 실시간 상태가 Git 저장소에 저장된 구성과 일치하는지 확인한다.

- Helm
> K8s용 package manager.
> 개발/운영에 있어 K8s 클러스터에 앱을 쉽게 패키징, 구성 및 배포할 수 있도록 한다.

- Kustomize
> K8s 자체 구성 관리 기능을 향상시키는 K8s 기반 구성 관리 도구.
> 다양한 환경이나 배포 시나리오와 같이 동일한 애플리케이션에 대해 조금씩 다른 여러 구성을 유지해야 하는 시나리오에 유용하다.

- Prometheus
> open source 모니터링 시스템.
> metric을 시계열 데이터로 수집하고 저장한다. (K8s 클러스터의 성능 측정 및 앱 상태에 대한 insight를 제공한다.)

- Istio
> 앱 트래픽 관리, 보안 정책 및 세밀한 서비스 모니터링을 가능하게 하는 service mesh.
> 코드 변경 없이 서비스를 보다 효과적으로 보호, 연결 및 모니터링할 수 있는 infra layer를 제공한다.


#kubernetes 

**kubernetes가 pod를 삭제하는 법** [](https://leehosu.github.io/kubernetes-delete-pod)
1. k delete po $po
2. k8s control plane 내의 **api-server**가 Pod 삭제 요청을 받고, 해당 요청을 cluster 내의 모든 control plane 구성 요소에 전달한다.
3. k8s control plane 내의 **kube-controller-manager**는 pod가 ReplicaSet / deployment와 같은 object의 소유 Pod인지 확인한다.
4. api-server가 해당 Node의 kubelet에게 pod 삭제 요청을 전달한다.
5. pod를 삭제하기 전에 pod 내의 컨테이너에 signal을 보내고 일정 시간 대기한다. 
6. pod의 모든 container가 종료되면 kubelet은 pod를 완전히 삭제하고, 상태 update를 kube-api-server로 보낸다.
7. kube-api-server는 pod 삭제 완료를 확인 후, 상태 update 후 요청 클라이언트에 응답한다.

