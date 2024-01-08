#kubernetes #ArgoCD
# ArgoCD
- 쿠버네티스를 위한 GitOps CD tool


Mac OS 기준
- 2개의 클러스터 생성할 건데, 하나는 for ArgoCD, 다른 하나는 for application push & run


```bash
$ brew install minikube
$ minikube start -p target-k8s
$ minikube start -p argocd-k8s

$ kubectl create namespace argocd
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
$ watch kubectl get pods -n argocd
```
watch가 없다면 설치한다.
watch는 특정 실행 명령어를 지속적으로 모니터링이 가능하다.
```bash
$ brew install watch
```

클러스터 외부에서는 액세스할 수 없습니다. 데모 환경이므로 포트포워딩을 해서 포트를 서비스에 노출시킨다.
```bash
$ kubectl port-forward svc/argocd-server -n argocd 8080:443
```
https://localhost:8080 에 접속하면 다음과 같은 UI를 확인 가능하다.
Argo CD는 최초 `admin` account의 초기 password를 K8s 의 secret 으로 저장해 놓는다.
```bash
$ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
ArgoCD CLI를 설치한 후 로그인을 한다.
**참고** : UI와 마찬가지로 서버 인증서 오류를 수락해야 한다.

```bash
$ brew tap argoproj/tap && brew install argoproj/tap/argocd
$ argocd login localhost:8080

```
https://localhost:8080/applications
로 접속해보면 UI가 정상적으로 뜬다.

(선택) 원한다면 password를 변경할 수도 있다.
```bash
$ argocd account update-password
```


- 클러스터 정보 확인
	kubectl config get-contexts

- 특정 클러스터 context 삭제
	kubectl config delete-context <context-name>




[Tanzu Developer Center](https://tanzu.vmware.com/developer/guides/argocd-gs/)