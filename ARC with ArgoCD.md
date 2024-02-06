**Owned by:** [@everchloe97](https://github.com/everchloe97)
**App ID:** 813630
**Client ID:** Iv1.f8405baecba4d4e1

mkdir github-key && cd github-key
vi k8s-test-app.pem
### [Install ARC](https://github.com/actions/actions-runner-controller/blob/master/docs/installing-arc.md)
- (default) / install cert manager
- install ARC by **Kubectl**
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.2/cert-manager.yaml

kubectl create -f https://github.com/actions/actions-runner-controller/releases/download/v0.25.2/actions-runner-controller.yaml
```
### Authenticating to the GitHub API - [Deploying Using GitHub App Authentication](https://github.com/actions/actions-runner-controller/blob/master/docs/authenticating-to-the-github-api.md)
APP_ID=813630
INSTALLATION_ID=46831345
PRIVATE_KEY_FILE_PATH=github-key/k8s-test-app.pem
```bash
$ kubectl create secret generic controller-manager \
    -n actions-runner-system \
    --from-literal=github_app_id=${APP_ID} \
    --from-literal=github_app_installation_id=${INSTALLATION_ID} \
    --from-file=github_app_private_key=${PRIVATE_KEY_FILE_PATH}
```

$ kubectl create secret generic controller-manager \
    --from-literal=github_app_id=${APP_ID} \
    --from-literal=github_app_installation_id=${INSTALLATION_ID} \
    --from-file=github_app_private_key=${PRIVATE_KEY_FILE_PATH}

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