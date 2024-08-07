## Mock Exam 1
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
#storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

**2-1. ETCD back up**

1. vi backup
2. docs에서 붙여넣는다
```shell
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>
```
3. cat /etc/kubernetes/manifests/etcd.yaml | grep -E "url|ca|cert-file|key"
4. cat backup
5. sed를 활용해서 실제 값으로 바꾼다.
```sh
sed -i 's|<trusted-ca-file>|/etc/kubernetes/pki/etcd/ca.crt|g' /opt/etcd-backup.db
...
```

**2-4. pv & pvc & pod - 다시 풀기.**
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
# volumeMode: Filesystem 안 쓰기.
  resources:
    requests:
      storage: 8Gi
```

pod에 volume - mountPath: "/data" 
쌍 따옴표 잊지 말기.