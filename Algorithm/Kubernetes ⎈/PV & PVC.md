---
created: 2024-01-25T10:57
updated: 2024-01-25T10:57
---
#kubernetes 

Kubernetes Storage Options — Persistent Volumes (PV), Persistent Volume Claims (PVC), Storage Classes (SC).

쿠버네티스에서 볼륨(Volume)을 사용하는 구조는 
**PV**라고 하는 PersistentVolume과 
**PVC**라고 하는 PersistentVolumeClaim 2개로 분리되어 있습니다.

![[persistent_volume.excalidraw]]


# Volume
- pod와 수명을 함께함. 동일한 Pod가 다른 Node에 재 배포된다면 기존 volume을 사용할 수 없음.

# PV/PVC
- volume과 달리 cluster와 수명을 함께함. (**cluster level**)
## **pv : cluster의 storage**
- provisioning 방법
	- static
	- dynamic - static pv가 사용자의 pvc와 일치하지 **않으면** cluster가 특별히 volume을 동적으로 provisioning하려고 시도할 수 있다.
## **pvc : 사용자의 storage에 대한 요청**

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0003
spec:
  capacity:      # PV 용량
    storage: 5Gi
  volumeMode: Filesystem    # host의 directory에 mount
  accessModes:
    - ReadWriteOnce # 단일 pod에서 w,r 가능
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: slow
  mountOptions:
    - hard
    - nfsvers=4.1
  nfs:
    path: /tmp
    server: 172.17.0.2
```

**accessModes**
- ReadWriteOnce
하나의 노드에서 해당 볼륨이 읽기-쓰기로 마운트 될 수 있다. ReadWriteOnce 접근 모드에서도 파드가 동일 노드에서 구동되는 경우에는 복수의 파드에서 볼륨에 접근할 수 있다.
- ReadOnlyMany
볼륨이 다수의 노드에서 읽기 전용으로 마운트 될 수 있다.
- ReadWriteMany
볼륨이 다수의 노드에서 읽기-쓰기로 마운트 될 수 있다.

**claimRef**
- 이 조건에 맞는 PVC만 해당 PV를 사용할 수 있다.

**REF**
- [# Kubernetes Persistent Volumes and Persistent Volume Claim](https://kamsjec.medium.com/kubernetes-persistent-volumes-and-persistent-volume-claim-5148338120e4)
- [# 엑셈 웨비나 Kubernetes 고급 (CKA 자격증 취득) - Storage](https://www.youtube.com/watch?v=DRjqUogf6I4)