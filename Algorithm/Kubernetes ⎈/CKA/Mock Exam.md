```sh
alias k=kubectl
export do="--dry-run=client -o yaml"
```

# Mock Exam 1
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
```sh
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

----
# Mock Exam 2
1. Take a backup of the etcd cluster and save it to `/opt/etcd-backup.db`.
- docs ) etcdctl 검색 > snapshot save 검색
```sh
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>
```

```sh
controlplane ~ ➜  cd /etc/kubernetes/manifests

controlplane /etc/kubernetes/manifests ➜  cat etcd.yaml | grep trusted-ca-file
    - --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    - --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt

controlplane /etc/kubernetes/manifests ➜  cat etcd.yaml | grep cert-file
    - --cert-file=/etc/kubernetes/pki/etcd/server.crt
    - --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt

controlplane /etc/kubernetes/manifests ➜  cat etcd.yaml | grep key-file
    - --key-file=/etc/kubernetes/pki/etcd/server.key
    - --peer-key-file=/etc/kubernetes/pki/etcd/peer.key

ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot save /opt/etcd-backup.db
```

2. Create a Pod called `redis-storage` with image: `redis:alpine` with a Volume of type `emptyDir` that lasts for the life of the Pod.
- Pod named 'redis-storage' created
- Pod 'redis-storage' uses Volume type of emptyDir
- Pod 'redis-storage' uses volumeMount with mountPath = /data/redis
- docs) emptyDir config 검색
```sh
k run redis-storage --image=redis:alpine --dry-run=client -o yaml > redis-storage.yaml
```
vi redis-storage.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: redis-storage
  name: redis-storage
spec:
  containers:
  - image: redis:alpine
    name: redis-storage
    volumeMounts:
    - mountPath: /data/redis
      name: sample-volume
  volumes:
    - name: sample-volume
      emptyDir: {}
```

```sh
k apply -f redis-storage.yaml
```

3. Create a new pod called `super-user-pod` with image `busybox:1.28`. Allow the pod to be able to set `system_time`.  
The container should sleep for 4800 seconds.

- Pod: super-user-pod
- Container Image: busybox:1.28
- Is SYS_TIME capability set for the container?

- docs) SYS_TIME 검색
- **sleep, 시간 모두 " " 안에 감싸주기.**
- yaml file이므로 tree 구조 잘 확인하기.

```sh
vi super-user-pod.yaml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: super-user-pod
  name: super-user-pod
spec:
  containers:
  - image: busybox:1.28
    name: super-user-pod
    command: ["sleep", "4800"]
    securityContext:
      capabilities:
        add: ["SYS_TIME"]
```
```sh
k apply -f super-user-pod.yaml
```

4. A pod definition file is created at `/root/CKA/use-pv.yaml`. Make use of this manifest file and mount the persistent volume called `pv-1`. Ensure the pod is running and the PV is bound.

- mountPath: `/data`    
- persistentVolumeClaim Name: `my-pvc`

- docs) persistent volume claims 검색
- accessModes 값과 resource.requests.storage가 **필수로** 들어가야 함.
```yaml
# my-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
       storage: 10Mi
```

```
# 순서대로 apply
k apply -f my-pvc.yaml
k apply -f /root/CKA/use-pv.yaml
```

```yaml
# /root/CKA/use-pv.yaml
apiVersion: v1
kind: Pod
metadata:
  name: use-pv
spec:
  volumes:
    - name: my-pvc
      persistentVolumeClaim:
        claimName: my-pvc
  containers:
  - image: nginx
    name: use-pv
    volumeMounts:
    - mountPath: "/data"
      name: my-pvc
```

5. Create a new deployment called `nginx-deploy`, with image `nginx:1.16` and `1` replica. Next upgrade the deployment to version `1.17` using rolling update.

- Deployment : nginx-deploy. Image: nginx:1.16
- Image: nginx:1.16
- Task: Upgrade the version of the deployment to 1:17
- Task: Record the changes for the image upgrade
- docs) upgrade the deploy
```sh
# Step 1: Create the Deployment
k create deploy nginx-deploy --image=nginx:1.16 --replicas=1 --dry-run=client -o yaml > deploy.yaml

k apply -f deploy.yaml --record

k rollout history deployment nginx-deploy

# Step 2: Upgrade the Deployment & Record
k set image deployment/nginx-deploy nginx=nginx:1.17 --record

k rollout history deployment nginx-deploy
```

6. Create a new user called `john`. Grant him access to the cluster. John should have permission to `create, list, get, update and delete pods` in the `development` namespace . The private key exists in the location: `/root/CKA/john.key` and csr at `/root/CKA/john.csr`.  

`Important Note`: As of kubernetes 1.19, the CertificateSigningRequest object expects a `signerName`.  
  Please refer the documentation to see an example. The documentation tab is available at the top right of terminal.

- CSR: john-developer, Status:Approved
- Role Name: developer, namespace: development, Resource: Pods
- Access: User 'john' has appropriate permissions
- docs) [Create a CertificateSigningRequest](https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#create-certificatessigningrequest)


```sh
controlplane ~/CKA ➜  cat john.csr | base64 -w 0
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZEQ0NBVHdDQVFBd0R6RU5NQXNHQTFVRUF3d0VhbTlvYmpDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRApnZ0VQQURDQ0FRb0NnZ0VCQUxCUm9hY25seTYxNnZoeklpcXVUTER2VFV1bEVyM2JHd1NlcCsxVHhxVU1kRVpGCnRHZzZISnluUTl1UThTZExJakV2NjF4VGYxRHp0WDdxVVI0M0ZFZTFPNEYrc1lYeVhUNC9oSi9hWlUxSWRhaUIKU2hmWW9rMmlaWmt5SEU4NU1xdDVSN2p2eS9FdjBpTHl4QTRPRWdYd2UxUncydk53U2p1ZEZNS3lpVVRCVlNqUgpCQlU3L0YwOVB5T1RSRldibnU4dkNMRXJqNCtxckNHVVlNZm1CcFJad25tWjVCZ082aVJGNktoQ2s2dHk4NEF1Ckl1OXVXWlhyQTIwTG4ycWFIQkFZSFl5U3Q5ZWhYbitiaWtXN2tWN0ZwNzZ5NlZ4aDkwNXdpUWc0OXZlcDY3VFcKeG5jWlBJcDlyeXdLK2RMcDNROUlPWEpocGNISnJpTlJ4Y0QxUVc4Q0F3RUFBYUFBTUEwR0NTcUdTSWIzRFFFQgpDd1VBQTRJQkFRQ3N1dk9JUjZzMmpPWXJxZlRsL0Noc1FDN0J0ODVHVlRIdUc1eTcyUlpTTzUyR0FyczJ2QTB6ClRVT1pFWU9yT1dlRTFCM1hDdUpBTFpmdVNZM2pqcUlZUGIzQ2lOZW55Lzk4SUt0Z2dtckdwdEhKdC84Z0JDa0sKcGhZWGgwc3NoL29FYXkrSDlqc3B5QkxNQThmRVF3TkJBZDFXR3lnWEVpWlZucXZ5UEpNZ1JOT1h4eENtOXVjYwo3V0ROSTBsOW1xM1VPMGE0N1UrL1FDcXphWmZ6Mis4Tkc3citHL2pwTmxZYTNBQlU5cWFLS1lMeFl6YVR5dWpXCkVhdFhJRjRWRktSbzg4ZkFYcVA1Z0NRdkpOSkYyMkd5V2YxeDg0OUl4a3Y5Uy9vdXZ3YTVxOStseEhCNVgxSk8KWUo5OEUzZzdIb1JmemJ6WDh3T1FPK1JpVnJveG0yNHkKLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
```
```yaml
# john-developer.yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: john-developer
spec:
  signerName: kubernetes.io/kube-apiserver-client
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZEQ0NBVHdDQVFBd0R6RU5NQXNHQTFVRUF3d0VhbTlvYmpDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRApnZ0VQQURDQ0FRb0NnZ0VCQUt2Um1tQ0h2ZjBrTHNldlF3aWVKSzcrVVdRck04ZGtkdzkyYUJTdG1uUVNhMGFPCjV3c3cwbVZyNkNjcEJFRmVreHk5NUVydkgyTHhqQTNiSHVsTVVub2ZkUU9rbjYra1NNY2o3TzdWYlBld2k2OEIKa3JoM2prRFNuZGFvV1NPWXBKOFg1WUZ5c2ZvNUpxby82YU92czFGcEc3bm5SMG1JYWpySTlNVVFEdTVncGw4bgpjakY0TG4vQ3NEb3o3QXNadEgwcVpwc0dXYVpURTBKOWNrQmswZWhiV2tMeDJUK3pEYzlmaDVIMjZsSE4zbHM4CktiSlRuSnY3WDFsNndCeTN5WUFUSXRNclpUR28wZ2c1QS9uREZ4SXdHcXNlMTdLZDRaa1k3RDJIZ3R4UytkMEMKMTNBeHNVdzQyWVZ6ZzhkYXJzVGRMZzcxQ2NaanRxdS9YSmlyQmxVQ0F3RUFBYUFBTUEwR0NTcUdTSWIzRFFFQgpDd1VBQTRJQkFRQ1VKTnNMelBKczB2czlGTTVpUzJ0akMyaVYvdXptcmwxTGNUTStsbXpSODNsS09uL0NoMTZlClNLNHplRlFtbGF0c0hCOGZBU2ZhQnRaOUJ2UnVlMUZnbHk1b2VuTk5LaW9FMnc3TUx1a0oyODBWRWFxUjN2SSsKNzRiNnduNkhYclJsYVhaM25VMTFQVTlsT3RBSGxQeDNYVWpCVk5QaGhlUlBmR3p3TTRselZuQW5mNm96bEtxSgpvT3RORStlZ2FYWDdvc3BvZmdWZWVqc25Yd0RjZ05pSFFTbDgzSkljUCtjOVBHMDJtNyt0NmpJU3VoRllTVjZtCmlqblNucHBKZWhFUGxPMkFNcmJzU0VpaFB1N294Wm9iZDFtdWF4bWtVa0NoSzZLeGV0RjVEdWhRMi80NEMvSDIKOWk1bnpMMlRST3RndGRJZjAveUF5N05COHlOY3FPR0QKLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
  usages:
  - digital signature
  - key encipherment
  - client auth
```

```sh
k apply -f john-developer.yaml

k certificate approve john-developer

k create role developer --resource=pods --verb=create,list,get,update,delete -n=development

k create rolebinding developer-role-binding --role=developer --user=john -n=development

k auth can-i update pods --as=john -n=development
# yes
```

7. Create a nginx pod called `nginx-resolver` using image `nginx`, expose it internally with a service called `nginx-resolver-service`. 
Test that you are able to look up the service and pod names from within the cluster. 
Use the image: `busybox:1.28` for dns lookup. Record results in `/root/CKA/nginx.svc` and `/root/CKA/nginx.pod`

- Pod: nginx-resolver created
- Service DNS Resolution recorded correctly
- Pod DNS resolution recorded correctly

- pod를 생성하고 노출시켜라. (k run > k expose)

```sh
kubectl run nginx-resolver --image=nginx
kubectl expose pod nginx-resolver --name=nginx-resolver-service --port=80 --target-port=80 --type=ClusterIP

kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service > /root/CKA/nginx.svc

kubectl get pod nginx-resolver -o wide
kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup <P-O-D-I-P.default.pod> > /root/CKA/nginx.pod
```

8. Create a static pod on `node01` called `nginx-critical` with image `nginx` and make sure that it is recreated/restarted automatically in case of a failure. 
Use `/etc/kubernetes/manifests` as the Static Pod path for example.

- static pod configured under /etc/kubernetes/manifests ?
- Pod nginx-critical-node01 is up and running

```sh
controlplane ~ ➜ k run nginx-critical --image=nginx-critical --dry-run=client -o yaml > static.yaml

controlplane ~ ➜ scp static.yaml node01:/root/

ssh node01

# staticPodPath 위치 확인
node01 ~ ➜ cat /var/lib/kubelet/config.yaml | grep -i staticpodpath

node01 ~ ➜ cp /root/static.yaml /etc/kubernetes/manifests/

node01 ~ ➜ exit

controlplane ~ ➜ k get po
```

---
### static pod 
[따배씨 03. Static Pod 생성하기](https://www.youtube.com/watch?v=u0Wg0HBVmmk)
_Static Pods_ are managed directly by the kubelet daemon on a specific node, without the [API server](https://kubernetes.io/docs/concepts/overview/components/#kube-apiserver) observing them.
- 일반적인 Pod의 실행 흐름 : 
	- kubectl -> Master Node의 etcd -> scheduler -> API -> Worker Node의 kubelet -> container engine -> kubelet
- API의 도움을 받지 않는다. **Worker Node에 kubelet에게 요청한다.** kubelet은 daemon. /var/lib/confgi.yaml 을 기반으로 Kubelet이 동작을 한다. 구성 정보이고, **static pod의 위치 정보**가 들어있다. 기본적으로 etcd/kubernetes/manifest

---

- rolling update + record
- dns resolver

---
# Mock Exam 3
1. Create a new service account & cluster role binding
```sh
k create serviceaccount pvviewer
k create clusterrole pvviewer-role --verb=list --resource=pv
k create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer
k run pvviewer --image=redis --dry-run=client -o yaml > pvviewer.yaml
vi pvviewer.yaml
```

```yaml
serviceAccountName: pvviewer
```

```sh
k apply -f pvviewer.yaml
```

2. List the `InternalIp` of all nodes in the cluster and save to a file.

```sh
k get no -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}' > /root/CKA/node_ips

# [?(@.type=="InternalIP")]
# 주소 배열에서 type이 "InternalIP"인 요소만 선택합니다.
# 여기서 @는 현재 요소를 나타내며, ?()는 필터링 조건입니다.
controlplane ~ ➜  cat CKA/node_ips 
192.17.58.3 192.17.58.6
```

3. Create a pod called `multi-pod` with two containers.
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

4. Create a Pod called `non-root-pod`
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

5. Pod & Service incoming connections are not working. Create NetworkPolicy.

```sh
k describe svc np-test-service
k describe po np-test-1
vi ingress-to-nptest.yaml
```
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
```sh
k apply -f ingress-to-nptest.yaml
```

6. Taint the worker node `node01` to be Unschedulable.
```sh
k taint no node01 env_type=production:NoSchedule 
k run dev-redis --image=redis:alpine
vi prod-redis.yaml
```

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
```sh
k apply -f prod-redis.yaml 
```
7. Create a pod called `hr-pod` in `hr` namespace belonging to the `production` environment and `frontend` tier .  
>image: `redis:alpine`

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
```sh
k create ns hr
k run hr-pod --image=redis:alpine -n hr --labels=environment=production,tier=frontend
k get po -n hr
```

8. A kubeconfig file called `super.kubeconfig` has been created under `/root/CKA`. There is something wrong with the configuration. Troubleshoot and fix it.

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

9. We have created a new deployment called `nginx-deploy`. scale the deployment to 3 replicas.
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