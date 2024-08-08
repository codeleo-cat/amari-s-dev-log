```sh
export do="--dry-run=client -o yaml"
```
#CKA 

[[Mock Exam]]
# Mock Exam 1
1-7. static pod 위치는 **/etc/kubernetes/manifests**

1-10. expose (존재하는 pod등의 resource를 기반으로) 한 뒤 ports section에 **nodePort를 추후 기술**하라.
Now, in generated service definition file add the `nodePort` field with the given port number under the `ports` section and create a service.
```sh
kubectl expose pod messaging --name=messaging-service --port=.....
```

1-12. **hostPath** typed PV
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

# Mock Exam 2
**2-1. ETCD back up**

1. vi backup
2. docs에서 붙여넣는다
```shell
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>
```
3. cat /etc/kubernetes/manifests/etcd.yaml | grep -E "url|trusted|cert|key"
4. cat backup
5. 실제 값으로 바꾼다.

2-3. capability 문제를 잘 읽는다.

**2-4. pv & pvc & pod**
indent 문제라.
```yaml
apiVersion: v1
spec:
  accessModes:
  - ReadWriteOnce
  # volumeMode: Filesystem
  resources:
    requests:
       storage: 8Gi
```

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