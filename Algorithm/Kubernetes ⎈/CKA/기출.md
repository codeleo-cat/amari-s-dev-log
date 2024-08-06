https://taronko.tistory.com/17
결국은 반복학습이고 Mock Exam 과 Killer.sh 를 많이 풀자.

[CKA 기출문제 정리](https://peterica.tistory.com/540)

1. 조건에 맞는 무언가를 만들어라.
	ex) deploy 생성하라
- pod에게 linux의 특정 권한을 부여하면서
- side car container를 옆에 붙여서 로그를 표준 출력으로 보내고
- pod가 node에 고르게 분포되는 것이 보장되어야 하고 (podAntiAffinity)
- 그 node에는 다른 pod들이 들어오면 안된다.  
(k taint no `node name` key=value:NoSchedule)

2. cluster의 특정 정보를 수집하라.
	ex) 특정 ns에 있는 pod들의 container image를 pod 이름 순으로 정렬해서 파일에 저장하라. (-o jsonpath)

3. debugging 
- ingress, Service, Network policy, deploy, pod 모두 살펴봐야 하고 app의 db password까지 짚어봐야 함.
- node에 ssh로 들어가서 kubelet ps log를 확인하고, ssl 인증서의 만료일자를 확인해야 함.

4. k8s upgrade
	1. 특정 pod는 downtime 발생하지 않게하고
	2. 새로운 node도 하나 같이 upgrade해서 cluster에 연결하라.
5. etcd backup


**핵심 유형**
- etcd 스냅샷 생성 및 복구
	- 인증서 (cert) 필요.
- 클러스터 노드 업그레이드
- 파드 간 네트워크 구성
- 로그 스트리밍용 사이드카 컨테이너 구현
- 트러블슈팅
	- 애플리케이션 (pod / container) 구동 문제
	- control plane 구성 요소의 구동 문제
	- 워커 노드의 구동 상태 확인 및 복원 문제
	- 네트워크 문제
	-> examine the cluster
		1. k get no
		2. k get po -A

- ﻿﻿notReady 상태인 Node를 ready 상태로 변경 ( kubelet restart )
- ﻿﻿Pod의 1og 확인 후 특정 경로에 저장하기 (kubectl 1ogs )
- ﻿﻿특정 node를 drain 후 uncordon 하기 ( kubectl drain , kubectl uncordon )
- ﻿﻿run 상태인 특정 파드에 sidecar container 생성 하여 로그 저장하기 (volumes mount)
- ﻿﻿각각 다른 ENV를 가진 multi container pod 생성하기
- ﻿﻿문제에 정의된 상황에 맞는 network policy 생성하기 (namespace selector, pod selector, ports)
- ﻿﻿CPU 가장 많이 할당된 Pod 이름 특정 경로에 저장하기 ( kubectl top )
- ﻿﻿ETCD Backup & Restore
- ﻿﻿kubeadm version 업그레이드 & kubelet, kubectl 업그레이드
- Pod에서 특정 log만 추출하여 파일로 저장
- ServiceAccount 생성, Role 생성, Role Binding 생성 후 확인
- ETCD snapshot save & restore
- Cluster Upgrade (Controlplane Node만 진행)
- Pod에 nodeSelector (disktype=ssd) 추가
- Pod를 생성하고 포트는 80, NodePort는 30020인 서비스를 배포
- Taint가 없는 Node의 개수를 파일로 저장
- Node의 상태가 ready 개수를 파일로 저장
- 사용률이 가장 높은 Pod를 특정 label로만 조회해서 파일로 저장
- 특정 Node를 Pod를 다른 Node로 Reschedule하고 해당 Node는 SchedulingDisabled
- 특정 Node가 NotReady 상태인데 Ready가 되도록 TroubleShooting
- 특정 Deployment에 대해 replicas 수정
- Ingress를 생성해서 이미 생성 되어 있는 서비스와 연결하고 확인
- Networkpolicy를 생성해서 특정 namespace의 Pod만 특정 경로로 연결
- PVC 생성 후 Pod와 PVC 연동 (PV는 이미 존재), PVC 용량 수정
- 기존에 배포된 Pod에 새로운 Container 추가 (Sidecar 패턴)
- 이미지 nginx 1.16으로 Deployment 생성 후 이미지를 nginx 1.17로 업그레이드 하기
