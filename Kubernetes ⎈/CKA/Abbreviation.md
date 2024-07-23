#kubernetes 

| Resource type         | Abbreviation | Description                     |
| --------------------- | ------------ | ------------------------------- |
| Pod                   | po           | Container의 기본 실행 단위. 집합         |
| Service               | svc          | pod를 외부/내부에 노출. 통신 담당           |
| Deployment            | deploy       | App의 배포/update 관리               |
| Namespace             | ns           | Cluster resource를 논리적으로 분리. 격리  |
| ConfigMap             | cm           | 설정 데이터를 저장 및 관리                 |
| Secret                | secret       | 민감한 데이터를 저장 및 관리                |
| PersistentVolume      | pv           | 독립적인 storage 자원을 정의             |
| PersistentVolumeClaim | pvc          | PV를 요청하는 storage 요청             |
| ReplicaSet            | rs           | 일정 수의 Pod 복제본을 유지               |
| DaemonSet             | ds           | 모든/선택된 Node에서 pod를 실행           |
| StatefulSet           | sts          | 상태가 있는 App의 배포 및 스케일링 관리        |
| Job                   | job          | 일회성 작업을 실행하고 완료를 보장             |
| CronJob               | cj           | 주기적으로 반복되는 작업을 스케줄링             |
| Node                  | no           | container 런타임이 탑재된 host system  |
| Ingress               | ing          | http / https 경로를 cluster 외부로 호출 |
| NetworkPolicy         | netpol       | 네트워크 트래픽을 제어하는 규칙을 정의.          |
| ClusterRole           | ==           | 클러스터 전체에서의 접근 권한을 정의.           |
| ClusterRoleBinding    | ==           | ClusterRole을 사용자/그룹에 바인딩.       |
| Role                  | ==           | 특정 네임스페이스 내에서의 접근 권한을 정의.       |
| Rolebinding           | ==           | Role을 사용자/그룹에 바인딩.              |
| ServiceAccount        | sa           | pod에서 실행되는 process에 대한 권한을 부여   |
