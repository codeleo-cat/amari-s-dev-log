---
created: 2024-01-30T14:44
updated: 2024-03-18T11:11
---
#Github_Actions #kubernetes 
**updated at 2024-03-18**
# Deploy Actions Runner Controller (ARC) using ArgoCD
### ARC (**A**ctions **R**unner **C**ontroller)
- vm처럼 단일 instance에 Github Actions runner를 설치할 수도 있지만, #ARC 를 사용해 K8s 위에 설치할 수 있다. (= 쿠버네티스 파드로 러너 실행 가능.)
	- 장점 : 비용 효율성 & 확장성 & 속도

1. App Repository > workflow 생성 
	```bash
	git clone `repository url`
	cd 해당 repository 경로
	mkdir -p .github/workflows
    cd .github/workflows
	vi github-actions-demo.yml
	```
## Prerequisites
- A Kubernetes cluster for hosting ARC and ArgoCD
- kubectl configured to manage the above cluster
- Helm CLI

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
```
openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout private.key -out certificate.crt -subj "/CN=sealed-secrets/O=my-org"
```
```bash
kubectl create secret tls sealed-secrets-key --key=private.key --cert=certificate.crt -n kube-system 
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets  \
helm install sealed-secrets sealed-secrets/sealed-secrets -n kube-system --set secretName=sealed-secrets-key --atomic
```
```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64  
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd  
rm argocd-linux-amd64
```
```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
**external IP로 접속** (34.28.176.180)

```bash
ADMIN_PASSWORD=$(argocd admin initial-password -n argocd | head -n 1)
echo $ADMIN_PASSWORD
```
**9kx6wCP8pOsbTaa8**

```bash
argocd login 34.28.176.180 --insecure  --username "admin" --password "${ADMIN_PASSWORD}"
```
```bash
argocd app create actions-runner-controller-apps \
    --dest-namespace argocd \
    --dest-server https://kubernetes.default.svc \
    --repo https://github.com/step-security/code-samples.git \
    --path deploy-arc-using-argocd/github-supported/controller \
    --sync-policy automated --auto-prune --self-heal
```


### Ref
- [Github Actions by ARC](https://tech.buzzvil.com/blog/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4%EC%97%90%EA%B2%8C-github-actions-%EC%84%A4%EC%B9%98%EC%97%90-%EB%8C%80%ED%95%B4-%EB%AC%BB%EB%8B%A4/)
- https://velog.io/@alli-eunbi/Git-EKS-ARC-세팅-방법
- https://ykarma1996.tistory.com/195
- https://trunk.io/blog/how-to-self-host-github-actions-runners
- [Deploy Actions Runner Controller (ARC) using ArgoCD](https://www.stepsecurity.io/blog/deploy-actions-runner-controller-using-argocd)