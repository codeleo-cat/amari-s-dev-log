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


helm upgrade --install --namespace actions-runner-system \
--create-namespace --wait actions-runner-controller \
actions-runner-controller/actions-runner-controller -f values.yaml

```shell
./run.sh --check --url https://github.com/everchloe97/argocd-example-apps --pat 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
```


kubectl get pod -n actions-runner | grep -i "k8s-action-runner"


kubectl create secret generic controller-manager -n actions-runner-system --from-literal=github_token=$GITHUB_TOKEN

 actions-runner-controller
LAST DEPLOYED: Tue Feb  6 06:18:26 2024
NAMESPACE: actions-runner-system
STATUS: deployed
REVISION: 2
TEST SUITE: None
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace actions-runner-system -l "app.kubernetes.io/name=actions-runner-controller,app.kubernetes.io/instance=actions-runner-controller" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace actions-runner-system $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace actions-runner-system port-forward $POD_NAME 8080:$CONTAINER_PORT







```bash
> mkdir actions-runner && cd actions-runner
> curl -O -L https://github.com/actions/runner/releases/download/v2.312.0/actions-runner-linux-x64-2.312.2.tar.gz
> tar xzf ./actions-runner-linux-x64-2.312.2.tar.gz

./config.sh --url https://github.com/everchloe97/argocd-example-apps --token ANYIA6EQ75WOAV32EDF3G7TFYHUVO