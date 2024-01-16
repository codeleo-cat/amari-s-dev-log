#kubernetes 
# Pod 집합에서 실행 중인 애플리케이션을 network 서비스로 노출하는 추상화 방법
## Service Type
서비스를 클러스터 밖에 위치한 외부 IP 주소에 노출하고 싶을 때.
Nested 기능으로 설계되어, 각 단계는 이전 단계에 더해지는 형태이다.

- Cluster IP 
	- 서비스를 클러스터-내부 IP에 노출
	- 타입을 지정하지 않으면 기본으로 설정
	- 클러스터 **내부**의 파드에서 서비스의 이름을 접근
- Node Port 
	- 고정 포트 `NodePort`로 각 노드의 IP에 서비스를 노출
	- Cluster IP의 접근 범위 뿐만 아니라 K8s 클러스터 **외부에서도** 노드의 IP 주소와 포트번호로 접근
- Load Balancer
	- 로드 밸런서를 사용하여 서비스를 외부에 노출
	- Node Port의 접근 범위 뿐만 아니라 K8s 클러스터 **외부에서 대표 IP 주소**로 접근
- External Name
	- K8s 클러스터 내부 파드에서 외부 IP 주소에 서비스의 이름으로 접근


