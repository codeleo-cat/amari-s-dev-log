```sh
export do="--dry-run=client -o yaml"
```
#CKA 

[[Mock Exam]]
# Mock Exam 1
1-7. static pod 위치는 **/etc/kubernetes/manifests**
1-12. **hostPath** typed PV

# Mock Exam 2
**2-1. ETCD back up**

1. vi backup
2. docs에서 붙여넣는다
```shell
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>
```
3. cat /etc/kubernetes/manifests/etcd.yaml | **grep -E "url|trusted|cert|key"**
4. cat backup
5. 실제 값으로 바꾼다.

2-3. capability 문제를 잘 읽는다.
2-5. k **set image** deploy/nginx-deploy nginx=nginx:1.17

**2-6. CSR**
인증서 sign 요청
1. key, csr 확인
2. 포맷 변경하고 cat ~ 값을 변경해서 terminal에서 바로 실행.
3. k certificate approve 해당 csr
4. k create role
5. k create rolebinding
namespace를 잊지 말 것.

2-7. Create a nginx pod called `nginx-resolver` using image `nginx`, expose it internally with a service called `nginx-resolver-service`. Test that you are able to look up the service and pod names from within the cluster. Use the image: `busybox:1.28` for dns lookup. Record results in `/root/CKA/nginx.svc` and `/root/CKA/nginx.pod`
[solve](https://medium.com/@ratchaphon/use-the-command-kubectl-run-and-create-a-nginx-pod-and-busybox-pod-2dde81ef9405)


----
[# CKA Practice Exam (mock questions) - Real 30 Practice questions and Answers](https://www.youtube.com/watch?v=Zm5sy6otLGc)
mock exam 1,2,3 / lightning lab / killer sh
[CKA 기출문제 17선](https://m.blog.naver.com/PostView.naver?blogId=wsxedc000&logNo=223415187424&proxyReferer=https:%2F%2Fm.blog.naver.com%2Fwsxedc000%2F223429877533&trackingCode=blog_postview)

- Gather logs
k get deploy -n management
k edit deploy collect-data -n management
k -n management logs deploy/collect-data -c nginx >> /root/logs.log

alias k=kubectl

1번) pod를 생성하되 worker node가 아닌 master node에 스케줄링 되도록 하라.
- pod 생성 시 spec 아래에 **nodeName: controlplane** 을 지정

2번) existing pod 기반으로 service에 expose하고 svc name = nginxsvc, pod port를 지정
**k expose po nginxpod --name=nginxsvc --port=80**   
curl 해당 clusterIP

3번) existing pod 기반으로 service에 expose하고 svc name = nginxnodeportsvc 여야 하고, svc는 **Nodeport**를 통해 접근 가능해야 함.
k expose po nginxpod --name=nginxnodeportsvc --port=80 **--type=NodePort**
k edit svc nginxnodeportsvc
nodePort: 30200 으로 변경

4번) existing deployment frontend in production ns, scale down replicas to 2, change image to nginx:1.25
k -n production edit deploy frontend 에서 바로 yaml 수정하여 변경. 
**OR**
k -n production **scale** deploy frontend --replicas=2 
k -n production **set image** deploy frontend nginx=nginx1.25

5번) Auto scale the existing deployment frontend in production ns at 80% of pod CPU usage, set minimum replicas=3 & maximum =5
k -n production **autoscale** deploy frontend --min=3 --max=5 --cpu-percent=80
k -n production get **hpa**

6번) Expose existing deploy in production ns named as frontend through Nodeport and Nodeport svc name=frontendsvc
k -n production **expose** deploy frontend --name=frontendsvc --port=80 --type=NodePort
curl internal-ip

7번) You can find a pod named task-pv-pod in the default ns, plz check the status of the pod and troubleshoot, you can recreate the pod if you want.
k **describe** po task-pv-pod
k get pv
k get pvc
k edit po task-pv-pod (오타가 있었네. task-pv-claimm >  task-pv-claim)
k replace -f 해당 file --force

8번) **deploy a pod**
k run web-pod --image=httpd --dry-run=client -o yaml > 1.yaml
k apply -f 1.yaml
k get po (status가 pending)
k describe po web-pod
>untolerated taint {node-role.... } 이 있다.

k get no node01 -o wide | grep -i taints
```yaml
spec 아래에
	toleration:
		-key: "node-role.... "
		 operation: "Exists"
		 effect: "NoSchedule"
```

8번) pv, pvc, deployment에 mount volume
pv
pvc
deployment
기억할 건, **namespace** 적어주는 것. 그리고 storage 용량이 pv와 **같아야** 한다.
최종적으로 pvc status의 bound를 확인해주어야 함.

9번) create pod with image, sleep command, **verify pod is running on node01**
k run my-busybox --image=busybox:1.31.1 ==--command sleep 4800==
k get no > node01이 scheduling disabled. 
**k uncordon node01** (node가 scheduling 불가능한 상태를 해제한다.)

10번) backend tier >>> application tier (ingress from tier=application)
k exec frontend... --sh -c "apt-get install telnet"
k exec frontend... telnet 192.168...

k exec -it application... apt update
k exec -it application... --sh -c "apt-get install -y telnet"
**network policy**
k get po --show-labels
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy 
metadata:
  name: backend-np-policy 
  namespace: default
spec:
  podSelector: 
    matchLabels:
      tier: backend 
  policyTypes:
    - Ingress
  ingress:
    - from:
      - podSelector:
          matchLabels:
            tier: application
      ports:
        - protocol: TCP
           port:3306
```

11번. db pods on Project-a ns only accessible from the svc pod that are running in Project-b ns
#### ns에 label을 추가한다.
**network policy는 label을 기반으로 동작한다.**
k label ns project-a namespace=project-a
k label ns project-b namespace=project-b
k get po --show-labels

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy 
metadata:
  name: test-np-policy 
  namespace: project-a
spec:
  podSelector: 
    matchLabels:
      app: db                   # pod
  policyTypes:
    - Ingress
  ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            namespace: project-b            # ns
      - podSelector:
          matchLabels:
            app: service                            # pod
```
-- ping

12번. need to insert side car for logging. multi-pod

pod config yaml 에서 한 container를 side car 명시 조건에 맞게 변경.
+volume mount path 또한 변경
command: ['sh', '-c', 'tail -f /var/busybox/.....' ]
pod 삭제하고 apply -f

13번. cronjob
분 시 일 월 요일
```plain
*/2
```
-> 2분 마다의 의미.
k get po --watch
k logs my-job.... > /root/logs.txt

14번. Find the schedulable nodes in the cluster and save the name and count in to the below file 
file path: /root/nodes.txt
```plain
k get no -o jsonpath='{.items[*].spec.taints}'
```
 답 기입.
 Node_Name= [node01]
 Node_Count= [1]

15번. Deploy a pod
k run web-pod --image=nginx --dry-run=client -o yaml > 15.yaml
k apply -f 15.yaml
k get po > Status = pending
**k get no > Status = NotReady** 
ssh node01
systemctl status kubelet
1) 일단 재 시작해볼까
systemctl start kubelet
2) cat /usr/bin/locale -> 없네....
vi /etc/systemd .... config 파일
locale 부분을 지운다.
system daemon-reload
**systemctl start kubelet**
exit
k get po

---
1h 42m 47s
16번. Please join node01 worker node to the cluster, and you have to deploy a pod in the node01, pod name should be web and image should be nginx.


