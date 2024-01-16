# 단일 클러스터 내에서 리소스 그룹 격리 메커니즘을 제공한다.

초기 네임 스페이스
- default (기본)
- kube-node-lease (kubelet이 하트비트를 보내서 컨트롤 플레인이 노드의 장애 탐지할 수 있도록 함.)
- kube-public (모든 클라이언트가 읽기 권한으로 접근 가능)
- kube-system (쿠버네티스 시스템에서 생성한 object를 위한 네임스페이스)

Not to do !
- prefix with *kube-*  (쿠버네티스 시스템용으로 예약되어 있음.)

kubectl
- kubectl get namespace 
사용 중인 클러스터의 현재 네임스페이스를 나열

## 네임스페이스와 DNS

## 모든 오브젝트가 네임스페이스에 속하지는 않음

## 자동 레이블링



### Ref
[네임스페이스](https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/namespaces)