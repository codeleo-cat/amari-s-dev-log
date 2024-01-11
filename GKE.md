### GKE (Google Kubernetes Engine)

- 기본적으로 API 활성화가 필요함.
Kubernetes Engine API
```shell
$ gcloud services enable compute.googleapis.com container.googleapis.com
```

[GKE autopilot](https://cloud.google.com/kubernetes-engine/docs/resources/autopilot-standard-feature-comparison?hl=ko) + Google Cloud Run > EKS와 Fargate의 조합과 유사
- GKE autopiloat
	- 자동 관리 (Node Scaling)
	- 리소스 최적화
- Google Cloud Run
	- 서버리스 컴퓨팅
	- auto scaling
	- 간편한 배포
	- 보안 및 격리