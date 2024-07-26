
```bash
alias k=kubectl
```
1) Deploy a pod named `nginx-pod` using the `nginx:alpine` image.
```bash
k run nginx-pod --image=nginx:alpine
```
2) Deploy a `messaging` pod using the `redis:alpine` image with the labels set to `tier=msg`.
```bash
k run messaging --image=redis:alpine --labels=tier=msg
```
3) Create a namespace name `apx-x9984574`
```bash
k create ns apx-x9984574
```
4) Get the list of nodes in JSON format and store it in a file at `/opt/outputs/nodes-z3444kd9.json`.
```bash
k get no -o json > /opt/outputs/nodes-z3444kd9.json
```
5) Create a service `messaging-service` to expose the `messaging` application within the cluster on port `6379`.
```bash
k expose po messaging --name messaging-service --port=6379
```
	service와 함께 application을 expose하라 = pod를 노출시켜라.
	default type 은 ClusterIp이다.

6) Create a deployment named `hr-web-app` using the image `kodekloud/webapp-color` with `2` replicas.
```bash
k create deploy hr-web-app --image=kodekloud/webapp-color --replicas=2
```
	deploy 생성과 조회는 쌍으로 기억.
	k create deploy & k get deploy


7. Create a static pod named `static-busybox` on the controlplane node that uses the `busybox` image and the command `sleep 1000`.
```bash
k run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
```
static pod의 manifest file 위치는 etc/kubernetes/manifests
그리고 restart=Never 옵션이 필요하다.

8) Create a POD in the `finance` namespace named `temp-bus` with the image `redis:alpine`.
```bash
k run temp-bus --image=redis:alpine -n finance
```

9) A new application `orange` is deployed. There is something wrong with it. Identify and fix the issue.
```bash
k describe po orange
k get pod orange -o yaml > orange.yaml

vi orange.yaml
# 오타 등을 찾아서 수정 후 저장
sleeeep 2; > sleep 2;

# 기존 pod delete 후 replace
k replace -f orange.yaml --force 
```

10) Expose the `hr-web-app` as service `hr-web-app-service` application on port `30082` on the nodes on the cluster. The web application listens on port `8080`.
```bash
k expose deploy hr-web-app --type=NodePort --port=8080 --name=hr-web-app-service --dry-run=client -o yaml > hr-web-app-service.yaml

vi hr-web-app-service.yaml
```
```yaml
# hr-web-app-service.yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: hr-web-app
  name: hr-web-app-service
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
    nodePort: 30080       # add this line !!
  selector:
    app: hr-web-app
  type: NodePort
status:
  loadBalancer: {}
```
```bash
k apply -f hr-web-app-service.yaml 
```
11) Use JSON PATH query to retrieve the `osImage`s of all the nodes and store it in a file `/opt/outputs/nodes_os_x43kj56.txt`. The `osImages` are under the `nodeInfo` section under `status` of each node.
```bash
k get no -o jsonpath='{.items[*].status.nodeInfo.osImage}' > /opt/outputs/nodes_os_x43kj56.txt
```
12) Create a `Persistent Volume` with the given specification - 
	**Volume name**: `pv-analytics`  
	**Storage**: `100Mi`  
	**Access mode**: `ReadWriteMany`  
	**Host path**: `/pv/data-analytics`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-analytics
spec:
  capacity:
    storage: 100Mi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  hostPath:
      path: /pv/data-analytics
```

# Mock Exam 3
## 1. service account & cluster role binding
k create serviceaccount pvviewer
k create clusterrole pvviewer-role --verb=list --resource=pv
k create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer
k run pvviewer --image=redis --dry-run=client -o yaml > pvvie
wer.yaml
```yaml
serviceAccountName: pvviewer
```
k apply -f pvviewer.yaml