
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
## 1. Create a new service account & cluster role binding
k create serviceaccount pvviewer
k create clusterrole pvviewer-role --verb=list --resource=pv
k create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer
k run pvviewer --image=redis --dry-run=client -o yaml > pvviewer.yaml
vi pvviewer.yaml
```yaml
serviceAccountName: pvviewer
```
k apply -f pvviewer.yaml

## 2. List the `InternalIp` of all nodes in the cluster and save to a file.

```sh
k get no -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}' > /root/CKA/node_ips

# [?(@.type=="InternalIP")]
# 주소 배열에서 type이 "InternalIP"인 요소만 선택합니다.
# 여기서 @는 현재 요소를 나타내며, ?()는 필터링 조건입니다.
controlplane ~ ➜  cat CKA/node_ips 
192.17.58.3 192.17.58.6
```

## 3. Create a pod called `multi-pod` with two containers.
- "sleep", "시간"
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-pod
spec:
  containers:
  - image: nginx
    name: alpha
    env:
    - name: name
      value: alpha
  - image: busybox
    name: beta
    command: ["sleep", "4800"]
    env:
    - name: name
      value: beta
```

## 4. Create a Pod called `non-root-pod`
runAsUser 를 search.
container name은 무엇을 지정하든 괜찮음. 특별한 명시가 없었기 때문에

vi non-root-pod.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: non-root-pod
spec:
  containers:
  - name: reids
    image: redis:alpine
  securityContext:
	runAsUser: 1000
	fsGroup: 2000
```
k apply -f non-root-pod.yaml 

## 5. Pod & Service incoming connections are not working. Create NetworkPolicy.
k describe svc np-test-service
k describe po np-test-1
vi ingress-to-nptest.yaml
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-to-nptest
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: np-test-1
  policyTypes:
  - Ingress
  ingress:
  - ports:
    - protocol: TCP
      port: 80
```
k apply -f ingress-to-nptest.yaml
## 6. Taint the worker node `node01` to be Unschedulable.
k taint no node01 env_type=production:NoSchedule 
k run dev-redis --image=redis:alpine
vi prod-redis.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: prod-redis
  name: prod-redis
spec:
  containers:
  - image: redis:alpine
    name: prod-redis
  tolerations:
  - key: "env_type"
    operator: "Equal"
    value: "production"
    effect: "NoSchedule"
```
k apply -f prod-redis.yaml 
## 7. Create a pod called `hr-pod` in `hr` namespace belonging to the `production` environment and `frontend` tier .  
image: `redis:alpine`

Use appropriate labels and create all the required objects if it does not exist in the system already.

k get ns
```plain
NAME                     STATUS   AGE
default                    Active      19m
kube-node-lease   Active      19m
kube-public            Active      19m
kube-system          Active      19m
```
(없으니 새로 생성)
k create ns hr
k run hr-pod --image=redis:alpine -n hr --labels=environment=production,tier=frontend
k get po -n hr

## 8. A kubeconfig file called `super.kubeconfig` has been created under `/root/CKA`. There is something wrong with the configuration. Troubleshoot and fix it.

k cluster-info --kubeconfig=/root/CKA/super.kubeconfig
```plain
E0726 10:04:45.003079   14273 memcache.go:265] couldn't get current server API group list: Get "https://controlplane:9999/api?timeout=32s": dial tcp 192.18.91.3:9999: connect: connection refused
E0726 10:04:45.003591   14273 memcache.go:265] couldn't get current server API group list: Get "https://controlplane:9999/api?timeout=32s": dial tcp 192.18.91.3:9999: connect: connection refused
E0726 10:04:45.004973   14273 memcache.go:265] couldn't get current server API group list: Get "https://controlplane:9999/api?timeout=32s": dial tcp 192.18.91.3:9999: connect: connection refused
E0726 10:04:45.005155   14273 memcache.go:265] couldn't get current server API group list: Get "https://controlplane:9999/api?timeout=32s": dial tcp 192.18.91.3:9999: connect: connection refused
E0726 10:04:45.006523   14273 memcache.go:265] couldn't get current server API group list: Get "https://controlplane:9999/api?timeout=32s": dial tcp 192.18.91.3:9999: connect: connection refused
```
k config view
- kubectl client에서 사용하는 구성 파일을 표시한다.
```plain
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://controlplane:6443
  name: kubernetes
```
vi /root/CKA/super.kubeconfig
9999 -> 6443

## 9. We have created a new deployment called `nginx-deploy`. scale the deployment to 3 replicas.
- etc/kubernetes/manifests/kube-controller-manager.yaml (**ekmk**)
kubectl scale deploy nginx-deploy --replicas=3

The `controller-manager` is responsible for scaling up pods of a replicaset. If you inspect the control plane components in the `kube-system` namespace, you will see that the `controller-manager` is not running.
k scale deploy nginx-deploy --replicas=3
```plain
controlplane ~ ➜  k get deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deploy   1/3                1                     1           2m4s
```
k get po -n kube-system
(이름이 잘못된 게 있네.)
```plain
kube-contro1ler-manager-controlplane   0/1     ImagePullBackOff   0             22s
```
sed -i 's/kube-contro1ler-manager/kube-controller-manager/g' /etc/kubernetes/manifests/kube-controller-manager.yaml
```plain
controlplane ~ ➜  k get deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deploy   1/3             3                       1           2m59s
```