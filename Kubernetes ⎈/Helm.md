---
created: 2024-01-04T11:15
updated: 2024-01-19T16:07
---
#kubernetes #helm 
# [[Kubernetes 동작]]에서 애플리케이션을 쉽게 배포하기 위해 사용되는 대표적인 패키지 매니저

### Function as a **Template Engine**
- Micro service 배포 시 공통된 하나의 template *(yaml)* 을 정의해두고, 그 안의 value만 수정하여 서비스 배포 시 재 사용이 가능하다.

### Chart

- Helm package. 
- K8s 클러스터 내에서 애플리케이션, 도구, 서비스 구동하는데 필요한 모든 리소스 정의가 포함되어 있다.

```yaml
apiVersion: The chart API version (required)
name: The name of the chart (required)
version: A SemVer 2 version (required)
kubeVersion: A SemVer range of compatible Kubernetes versions (optional)
description: A single-sentence description of this project (optional)
type: The type of the chart (optional)
keywords:
  - A list of keywords about this project (optional)
home: The URL of this projects home page (optional)
sources:
  - A list of URLs to source code for this project (optional)
dependencies: # A list of the chart requirements (optional)
  - name: The name of the chart (nginx)
    version: The version of the chart ("1.2.3")
    repository: (optional) The repository URL ("https://example.com/charts") or alias ("@repo-name")
    condition: (optional) A yaml path that resolves to a boolean, used for enabling/disabling charts (e.g. subchart1.enabled )
    tags: # (optional)
      - Tags can be used to group charts for enabling/disabling together
    import-values: # (optional)
      - ImportValues holds the mapping of source values to parent key to be imported. Each item can be a string or pair of child/parent sublist items.
    alias: (optional) Alias to be used for the chart. Useful when you have to add the same chart multiple times
maintainers: # (optional)
  - name: The maintainers name (required for each maintainer)
    email: The maintainers email (optional for each maintainer)
    url: A URL for the maintainer (optional for each maintainer)
icon: A URL to an SVG or PNG image to be used as an icon (optional).
appVersion: The version of the app that this contains (optional). Needn't be SemVer. Quotes recommended.
deprecated: Whether this chart is deprecated (optional, boolean)
annotations:
  example: A list of annotations keyed by name (optional).

```
- type 
	- application
	- library
- dependencies : 하나의 차트가 여러 차트에 종속될 수 있음.

### Repository 

- 차트를 모아두고 공유하는 장소
### Release

- K8s 클러스터에서 구동되는 차트의 인스턴스.

### Helm CLI

- Helm get manifest `Release Name`
- Helm status `Release Name`
- Helm install  `Release Name`
- Helm uninstall  `Release Name`
- Helm repo add `Release Name`
- Helm repo update
- Helm repo list

### Helm upgrade & Helm rollback

- **upgrade** - 새로운 버전의 차트가 release 되었을 때, release의 구성을 변경하고자 할 때
- **rollback**

### Helm으로 ArgoCD install
[Argo-Helm](https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd)
```
brew install minikube
minikube start

kubectl create ns argocd
helm repo add argo https://argoproj.github.io/argo-helm

```


Helm v3 vs v2
- Tiller가 v3에서는 사라졌다.

helm은 K8s 내부에 charts를 설치하고, 각 설치에 대해 새로운 release를 생성한다. <br/> 새로운 chart를 찾기 위해 Helm Chart Repository를 검색할 수 있다.

### Ref
[Helm 공식 Docs](https://helm.sh/)
[Helm 실습](https://github.com/mincloud1501/Helm)

