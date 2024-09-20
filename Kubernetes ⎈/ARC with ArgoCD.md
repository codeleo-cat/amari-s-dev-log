---
created: 2024-02-06T11:30
updated: 2024-02-07T12:02
---
## Create Github App for Organization & Personal User

**Owned by:** [@hnyeom](https://github.com/hnyeom)
**App ID:** 819851
**Client ID:** Iv1.318404186a5de9ec
**INSTALLATION_ID** : 47016647

APP_ID=819851
INSTALLATION_ID=47016647
PRIVATE_KEY_FILE_PATH=
kubectl create secret generic controller-manager \
    -n actions-runner-system \
    --from-literal=github_app_id=${APP_ID} \
    --from-literal=github_app_installation_id=${INSTALLATION_ID} \
    --from-file=github_app_private_key=${PRIVATE_KEY_FILE_PATH}

mkdir arc && cd arc
## Installing ARC

*Install cert manager* 
(https://cert-manager.io/docs/installation/helm/)

kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.2/cert-manager.yaml

helm repo add actions-runner-controller https://actions-runner-controller.github.io/actions-runner-controller
helm upgrade --install --namespace actions-runner-system --create-namespace \
             --wait actions-runner-controller actions-runner-controller/actions-runner-controller

확인
helm repo list
helm list -A

### [Install ARC](https://github.com/actions/actions-runner-controller/blob/master/docs/installing-arc.md)
- (default) / install cert manager
- install ARC by **Kubectl**
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.2/cert-manager.yaml

kubectl create -f https://github.com/actions/actions-runner-controller/releases/download/v0.25.2/actions-runner-controller.yaml
```
### Authenticating to the GitHub API - [Deploying Using GitHub App Authentication](https://github.com/actions/actions-runner-controller/blob/master/docs/authenticating-to-the-github-api.md)

### [choosing-runner-destination](https://github.com/actions/actions-runner-controller/blob/master/docs/choosing-runner-destination.md)
runnerdeployment.yaml
```yaml
apiVersion: actions.summerwind.dev/v1alpha1
kind: RunnerDeployment
metadata:
  name: example-runnerdeploy
spec:
  # This will deploy 2 runners now
  replicas: 2
  template:
    spec:
      repository: everchloe97/argocd-example-apps
```
*note:- Replace “mumoshu/actions-runner-controller-ci” with your repository name.

kubectl apply -f runnerdeployment.yaml


helm upgrade --install --namespace actions-runner-system \
--create-namespace --wait actions-runner-controller \
actions-runner-controller/actions-runner-controller -f values.yaml

```shell
./run.sh --check --url https://github.com/everchloe97/argocd-example-apps --pat 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
```


kubectl get pod -n actions-runner | grep -i "k8s-action-runner"


kubectl create secret generic controller-manager -n actions-runner-system --from-literal=github_token=$GITHUB_TOKEN



https://medium.com/mossfinance/github-self-hosted-runners-on-kubernetes-with-actions-runner-controller-41e30c4cb76e




```bash
> mkdir actions-runner && cd actions-runner
> curl -O -L https://github.com/actions/runner/releases/download/v2.312.0/actions-runner-linux-x64-2.312.2.tar.gz
> tar xzf ./actions-runner-linux-x64-2.312.2.tar.gz

