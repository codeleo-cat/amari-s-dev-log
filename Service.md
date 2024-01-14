#kubernetes 
## Service Type

- Cluster IP
	- 타입을 지정하지 않으면 기본으로 설정
	- 클러스터 **내부**의 파드에서 서비스의 이름을 접근
- Node Port
	- Cluster IP의 접근 범위 뿐만 아니라 K8s 클러스터 **외부에서도** 노드의 IP 주소와 포트번호로 접근
- Load Balancer
	- Node Port의 접근 범위 뿐만 아니라 K8s 클러스터 **외부에서 대표 IP 주소**로 접근
- External Name
	- K8s 클러스터 내부 파드에서 외부 IP 주소에 서비스의 이름으로 접근




gcloud projects add-iam-policy-binding ${PROJECT_ID} \  
	--member=serviceAccount:687360462197-compute@developer.gserviceaccount.com \  
	-role="roles/compute.instanceAdmin.v1"