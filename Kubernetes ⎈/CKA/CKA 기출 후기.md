2024/08/17 시험

CKA 후기.
뭐랄까 어찌되었든 시험은 끝이 났구나 싶다.
확실히 약한 부분은 약한 부분이고, 예상치 못한 변수도 있었다.
일단 모니터 패널이 나갔고 ㅠㅠ 그냥 내 부주의에 대한 값을 치룬다고 생각하자.
etcd backup에서 파일이 없다고 뜬다던지
cluster upgrade 역시 무난하게 할 줄 알았는데..
그래도 CKA가 문제은행 식이라는 것을 확인할 수 있어 다행이었다.
기억이 증발되기 전에 기록해두어야 겠다.
결과도 중요하지만 과정도 중요하다고 느낀 시험 준비 기간이었다.
딱 내가 덜 봤던 부분에서 역시나 헷갈리는 기분이 많이 들었다.
어찌되었든 부담은 이제 없다.

---
준비물
여권을 준비해서 미리 등록해두세요.
가장 확실한 방법입니다.

후기
생각보다 PSI 환경이 그렇게 느리지는 않았습니다.
연결 역시 중간에 1분 정도 끊기기는 했으나 시험에 지장을 줄 정도는 아니었습니다.
그리고 17문제를 푸는데에 120분이 주어지는데 시간은 충분하게 느껴집니다.
저는 약 90분 정도가 걸렸습니다.
다만 시험 환경 세팅 & 감독관의 환경 검사에 약 20분 넘게 소요되었습니다.
방 전체를 웹캠으로 비추고, 귀도 비추고, 양손과 팔도 들어야 하고
투명한 컵에 담긴 물과 마우스 정도만 허용됩니다.
참고로 책상 밑도 깨끗해야 합니다.


준비 과정
- Udemy 강의. Kubernetes 강의의 꽃이죠.
- 30문제 풀이

빠른 취득을 원한다면
- Mock Exam 1~3 은 꼭 풀어보는 것을 추천드리며
- 문제 은행을 외우는 것을 추천드립니다.

---
기억나는 시험 문제 유형
1. cluster내의 master node만(worker node는 x) kubeadm,kubelet,kubectl을 특정 version으로 update
(1.30.0 -> 1.30.1)
- 여기서 어떤 문제가 생겨서 안되었습니다. 

2. cluster내에서 unready 상태인 worker node의 문제 해결하고, ready 상태로 바꾸기
```sh
k get no

# node 상태가 unReady 인 노드로 접속
ssh `node`

systemctl status kubelet # 문제가 있을 것

systemctl start kubelet # 다시 시작시켜준다.
```

**1. systemctl start kubelet**
• **동작**: kubelet 서비스를 시작합니다.
• **설명**: kubelet 서비스가 중지되어 있는 상태에서 이 명령어를 실행하면 kubelet이 시작됩니다. 서비스가 이미 실행 중인 경우에는 변화 X

**2. systemctl restart kubelet**
• **동작**: kubelet 서비스를 재시작합니다. (현재 실행 중인 kubelet 서비스를 중지한 다음, 다시 시작)

3. cluster내에서 schedulable한 node의 갯수 세기(unscheduled taint 된 node 제외하고) > 특정 파일로 저장
k describe no | grep -i taint
Taints:             unscheduled
Taints:             <none>
Taints:             <none>
echo 1 >> 특정 파일

1. [풀이](https://cumulus.tistory.com/106)
- From the pod label name=overloaded-cpu, find pods running high CPU workloads and name of the pod consuming most CPU to the file /var/CKA2024/cpu_load_pod.txt

```sh
k top po -l name=overloaded-cpu --sort-by=cpu
(`A` pod)

echo `A pod name` >> /var/CKA2024/cpu_load_pod.txt
```

k get po --show-labels
k top po
제일 높은 Pod name >> 파일로 저장
    
[풀이](https://www.examtopics.com/discussions/cncf/view/126449-exam-cka-topic-1-question-19-discussion/)
Create a new NetworkPolicy named allow-port-from-namespace in the existing namespace echo.  
  
Ensure that the new NetworkPolicy allows Pods in namespace internal to connect to port 9200/tcp of Pods in namespace echo.  
  
Further ensure that the new NetworkPolicy:  
  
• does not allow access to Pods, which don't listen on port 9200/tcp  
• does not allow access from Pods, which are not in namespace internal

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-port-from-namespace
  namespace: echo
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: internal   
    ports:
    - protocol: TCP
      port: 9200
```
        
3. etcd backup and restore
권한 문제로 backup save가 안됨.
/etc/kubernetes/manifests/etcd.yaml
sudo
    
4. log용 sidecar container 추가
[따배쿠](https://cumulus.tistory.com/99)
[풀이](https://www.examtopics.com/discussions/cncf/view/87619-exam-cka-topic-1-question-13-discussion/)
    
        
5. ingress
    
    - "/hi" prefix에 "hi" service 연결하기
        
6. [풀이](https://cumulus.tistory.com/105)
- Reconfigure the existing deployment front-end and add a port specification named http exposing port 80/tcp of the existing container nginx.
- Create a new service named front-end-svc exposing the container port http
- Configure the new service to also expose the individual Pods visa a NodePort on the nodes on which they are scheduled

k get deploy front-end -o yaml > front-end.yaml
vi front-end.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: front-end
spec:
  replicas: 2
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        name: http
        ports:
        - containerPort: 80
          name: http
```
변경 🔽
```yaml
apiVersion: v1
kind: Service
metadata:
  name: front-end-svc
spec:
  type: NodePort
  selector:
    run: nginx
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: http
```
k delete deploy front-end
k apply -f front-end.yaml


7. serviceaccount 생성, clusterrole, clusterrolebinding 생성
ci-cd token

```sh
k create sa
k create clusterrole
k create clusterrolebinding
k auth can -i
```
    
8. 존재하는 deployment의 replica 개수 증가
    
9. pv 생성(hostpath)
    
10. pvc 생성 후(storage class) 해당 pvc를 사용하는 pod 생성, 그리고 이후에 pvc를 수정해서 capacity 늘리기(10Mi -> 70Mi)
    
11. multi container(한 pod내에 nginx, memcached 두개의 container 추가)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: lab004
spec:
  containers:
  - image: nginx
    name: nginx
  - image: memcached
    name: memcached
```

12. nodeSelector, 특정 label를 지닌 node에 pod가 schedule 될 수 있게 설정
node라벨이 'disk=running'이 있는 node에만 Pod를 schedule하는 문제  
Affinity 설정으로 Pod Schedule 진행
- Schedule a pod as follows:
  . Name : eshop-store
  . Image : nginx
  . Node selector : disk=running

k get no --show-labels
kubectl run eshop-store --image=nginx --dry-run=client -o yaml > eshop-store.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: nginx
      image: nginx:1.14.2
  nodeSelector: disk=running
```

13
- Monitor the logs of pod `custom-app` and: Extract log lines corresponding to error `file not found`
- Write them to /var/CKA2024/pod_log.
```sh
k logs `custom-app`  | grep -i `file not found` >> /var/CKA2024/pod_log
```
여기서 pod name과 error 종류만 달라짐.


14.
Set the node labelled with name=**ek8s-node-1** as unavailable and reschedule all the pods running on it.
```sh
kubectl drain ek8s-node-1 --delete-local-data --ignore-daemonsets --force
```

문제은행입니다.

https://github.com/sandistorm/CKA-Certified-Kubernetes-Administrator-2022/blob/main/CKA%20Exam.md