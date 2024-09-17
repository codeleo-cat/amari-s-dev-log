---
created: 2024-01-10T17:04
updated: 2024-01-18T11:21
---
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

### Using GKE with Terraform
- [Terraform Docs](https://registry.terraform.io/providers/hashicorp/google/latest/docs/guides/using_gke_with_terraform)

### Ref
[컨테이너화된 웹 애플리케이션 배포](https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app?hl=ko#cloud-shell)