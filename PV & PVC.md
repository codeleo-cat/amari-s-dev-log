![](https://blog.kakaocdn.net/dn/b17MGq/btrrP4ojece/PxHCyMhiiak9IyRjQ44Gk0/img.png)

Kubernetes Storage

쿠버네티스에서 볼륨(Volume)을 사용하는 구조는 **PV**라고 하는 **퍼시스턴트 볼륨(PersistentVolume)**과 **PVC**라고 하는 **퍼시스턴트 볼륨 클레임(PersistentVolumeClaim)** 2개로 분리되어 있습니다.

## PV/PVC

**PV는 볼륨 자체**를 뜻합니다. 클러스터 안에서 자원으로 다룹니다. 파드와는 별개로 관리되며 별도의 생명 주기가 있습니다. **PVC는 사용자가 PV에 하는 요청**입니다. 사용하고 싶은 용량은 얼마인지, 읽기/쓰기는 어떤 모드로 설정하고 싶은지 등을 정해서 요청합니다.

쿠버네티스 볼륨을 파드에 직접 할당하는 방식이 아니라 중간에 PVC를 두어 파드와 파드가 사용할 스토리지를 분리했습니다. 이런 구조는 파드 각각의 상황에 맞게 다양한 스토리지를 사용할 수 있게 합니다. 

클라우드 서비스를 사용할 때는 본인이 사용하는 클라우드 서비스에서 제공해주는 볼륨 서비스를 사용할 수도 있고, 직접 구축한 스토리지를 사용할 수도 있습니다. 이렇게 다양한 스토리지를 PV로 사용할 수 있지만 파드에 직접 연결하는 것이 아니라 PVC를 거쳐서 사용하므로 파드는 어떤 스토리지를 사용하는지 신경 쓰지 않아도 됩니다.

PV와 PVC는 다음과 같은 생명주기가 있습니다.

![](https://blog.kakaocdn.net/dn/cGG4N9/btrrKIfEPPV/chFy6chDn2PC23MbvC13t1/img.png)

PV/PVC의 생명 주기

### 1. Provisioning(프로비저닝)

**PV를 만드는 단계를 프로비저닝(Provisioning)이라고 합니다.** 프로비저닝 방법에는 두 가지가 있는데**, PV를 미리 만들어 두고 사용하는 정적(static) 방법**과 **요청이 있을 때 마다 PV를 만드는 동적(dynamic)** 방법입니다.

#### 정적(static) 프로비저닝

정적으로 PV를 프로비저닝할 때는 클러스터 관리자가 미리 적정 용량의 PV를 만들어 두고 사용자의 요청이 있으면 미리 만들어둔 PV를 할당합니다. **사용할 수 있는 스토리지 용량에 제한이 있을 때 유용합니다.** 사용하도록 미리 만들어 둔 PV의 용량이 100GB라면 150GB를 사용하려는 요청들은 실패합니다. 1TB 스토리지를 사용하더라도 미리 만들어 둔 PV 용량이 150GB 이상인 것이 없으면 요청이 실패합니다.

#### 동적(dynamic) 프로비저닝

동적으로 프로비저닝 할 때는 사용자가 PVC를 거쳐서 PV를 요청했을 때 생성해 제공합니다. 쿠버네티스 클러스터에 사용할 1TB 스토리지가 있다면 **사용자가 원하는 용량만큼을 생성해서 사용할 수 있습니다.** 정적 프로비저닝과 달리 필요하다면 한번에 200GB PV도 만들 수 있습니다. PVC는 동적 프로비저닝할 때 여러가지 스토리지 중 원하는 스토리지를 정의하는 스토리지 클래스(Storage Class)로 PV를 생성합니다.

### 2. Binding(바인딩)

**바인딩(Binding)은 프로비저닝으로 만든 PV를 PVC와 연결하는 단계입니다.** PVC에서 원하는 스토리지의 용량과 접근방법을 명시해서 요청하면 거기에 맞는 PV가 할당 됩니다. 이 때 PVC에서 원하는 PV가 없다면 요청은 실패합니다. 하지만 PVC는 원하는 PV가 생길 때까지 대기하다가 바인딩합니다. 

**PV와 PVC의 매핑은 1대1 관계 입니다**. PVC 하나가 여러 개 PV에 할당될 수 없습니다.

### 3. Using(사용)

**PVC는 파드에 설정되고 파드는 PVC를 볼륨으로 인식해서 사용합니다.**

할당된 PVC는 파드를 유지하는 동안 계속 사용하며 시스템에서 임의로 삭제할 수 없습니다. 이 기능을 **'Storage Object in Use Protection' (사용 중인 스토리지 오브젝트 보호)**라고 합니다. 사용 중인 데이터 스토리지를 임의로 삭제하는 경우 치명적인 결과가 발생할 수 있으므로 이런 보호 기능을 사용하는 것입니다.

### 4. Reclaiming(반환)

사용이 끝난 PVC는 삭제되고 PVC를 사용하던 **PV를 초기화(reclaim)하는 과정**을 거칩니다**. 이를 Reclaiming(반환)이라고 합니다.** 

초기화 정책에는 **Retain, Delete, Recycle**이 있습니다.

#### Retain

**Retain은 PV를 그대로 보존합니다.** PVC가 삭제되면 사용 중이던 PV는 해제(released) 상태여서 다른 PVC가 재사용할 수 없습니다. 단순히 사용 해제 상태이므로 PV 안의 데이터는 그대로 남아있습니다. 이 PV를 재사용하려면 관리자가 다음 순서대로 직접 초기화해줘야합니다.

1. PV 삭제. 만약 PV가 외부 스토리지와 연결되었다면 PV는 삭제되더라도 외부 스토리지의 볼륨은 그대로 남아 있습니다.
2. 스토리지에 남은 데이터를 직접 정리
3. 남은 스토리지의 볼륨을 삭제하거나 재사용하려면 해당 볼륨을 이용하는 PV 재생성

#### Delete

**PV를 삭제하고 연결된 외부 스토리지 쪽의 볼륨도 삭제합니다.** 프로비저닝할 때 동적 볼륨 할당 정책으로 생성된 PV들은 기본 반환 정책(Reclaim Policy)이 Delete 입니다. 상황에 따라 처음에 Delete로 설정된 PV의 반환 정책을 수정해서 사용해야 합니다.

#### Recycle

**Recycle은 PV의 데이터들을 삭제하고 다시 새로운 PVC에서 PV를 사용할 수 있도록 합니다.** 특별한 파드를 만들어 두고 데이터를 초기화하는 기능도 있지만 동적 볼륨 할당을 기본 사용할 것을 추천

## PV/PVC 예시

### PV 예시

|   |   |   |
|---|---|---|
1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14|
apiVersion: v1<br><br>kind: PersistentVolume<br><br>metadata:<br><br>  name: pv-hostpath<br><br>spec:<br><br>  capacity:<br><br>    storage: 2Gi # 스토리지 용량 2GB<br><br>  volumeMode: Filesystem # 파일 시스템 형식<br><br>  accessModes: # 읽기/쓰기 옵션<br><br>  - ReadWriteOnce<br><br>  storageClassName: manual<br><br>  persistentVolumeReclaimPolicy: Delete<br><br>  hostPath:<br><br>    path: /tmp/k8s-pv # 스토리지를 연결할 Path|
[cs](http://colorscripter.com/info#e)|

**8번 라인의** .spec.volumeMode 필드는 쿠버네티스 1.9버전 부터 추가된 필드입니다. 기본 필드 값은 FileSystem 으로 볼륨을 **파일 시스템 형식**으로 사용합니다. 추가로 raw 라는 필드 값을 설정할 수 있습니다. 볼륨을  **RAW 블록 디바이스 형식**으로 설정해서 사용합니다. RAW 블록 디바이스를 지원하는 스토리지 플로그인은 awsElasticBlockStore, azureDisc, fc(fibre channel), gcePersistentDisk, iscsi, local volume, Ceph Block Device의 rbd 등이 있습니다.

**9번 라인**의 .spec.accessModes 필드는 볼륨의 읽기/쓰기 옵션을 설정합니다. 볼륨은 한 번에  accessModes 필드를 하나만 설정할 수 있으며 필드 값은 세 가지가 있습니다. 

- ReadWriteOnce: 노드 하나에만 볼륨을 읽기/쓰기하도록 마운트할 수 있음
- ReadOnlyMany: 여러 개 노드에서 읽기 전용으로 마운트할 수 있음
- ReadWriteMany: 여러 개 노드에서 읽기/쓰기 가능하도록 마운트할 수 있음

**11번 라인**의 .spec.storageClassName 은 **스토리지 클래스(StorageClass)를 설정하는 필드**입니다. **특정 스토리지 클래스가 있는 PV는 해당 스토리지 클래스에 맞는 PVC만 연결됩니다.** PV에  .spec.storageClassName 필드 설정이 없으면  .spec.storageClassName 필드 설정이 없는 PVC와만 연결됩니다.

**12번 라인**의 .spec.persistentVolumeReclaimPolicy 필드는 PV가 해제되었을 때 초기화 옵션을 설정합니다. 앞에서 살펴본 것 처럼 Retain, Recycle, Delete 정책 중 하나입니다.

**14번 라인**의 .spec.hostPath 필드는 **해당 PV의 볼륨 플러그인을 명시**합니다. 필드 값을 hostPath로 설정했기 때문에 하위의 .spec.hostPath.path 필드는 마운트시킬 로컬 서버의 경로를 설정합니다.

위의 설정을 **pv-hostpath.yaml** 로 저장하고 kubectl apply -f pv-hostpath.yaml 명령으로 클러스터에 적용합니다.  
그 후 kubectl get pv 명령어로 PV의 상태를 확인합니다.

|   |   |   |
|---|---|---|
1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7|
[root@k8s-master jh]# kubectl apply -f pv-hostname.yaml <br><br>persistentvolume/pv-hostpath created<br><br>[root@k8s-master jh]# kubectl get pv<br><br>NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE<br><br>pv-hostpath   2Gi        RWO            Delete           Available           manual                  16m<br><br>[Colored by Color Scripter](http://colorscripter.com/info#e)|
[cs](http://colorscripter.com/info#e)|

STATUS 항목이 Available이면 정상적으로 설정된 것입니다. PV의 STATUS 값은 **PVC에서 사용할 수 있도록 준비된** Available, **특정 PVC에 연결된** Bound**, PVC는 삭제되었고 PV는 아직 초기화 되지 않은** Released**, 자동 초기화를 실패한** Failed 등이 있습니다.

### PVC 예시

|   |   |   |
|---|---|---|
1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12|
apiVersion: v1<br><br>kind: PersistentVolumeClaim<br><br>metadata:<br><br>  name: pvc-hostpath<br><br>spec:<br><br>  accessModes: # AccessModes<br><br>  - ReadWriteOnce<br><br>  volumeMode: Filesystem # 파일  시스템 형식<br><br>  resources:<br><br>    requests:<br><br>      storage: 1Gi # 1GB 요청<br><br>  storageClassName: manual # 스토리지 클래스 명|
[cs](http://colorscripter.com/info#e)|

주요 필드는 PV에서 설정한 필드와 유사합니다.

9~11번 라인의 .spec.resources.requests.storage 필드는 **자원을 얼마나 사용할 것인지 요청(request)합니다.** 여기서는 필드 값으로 1Gi(1GB)를 설정했습니다. 필드 값을 설정할 때는 앞에서 만든 PV의 용량을 초과하면 안됩니다. 만약 초과하는 경우 사용할 수 있는 PV가 없으므로 PVC를 생성할 수 없는 Pending 상태가 됩니다.

또한  .spec.storageClassName 필드를 위에서 생성한 PV와 동일하게 생성하여 위의 PV에 정상적으로 연결될 수 있도록 합니다.

위의 설정을 **pvc-hostpath.yaml** 로 저장하고 kubectl apply -f pvc-hostpath.yaml 명령으로 클러스터에 적용합니다.  
그 후 kubectl get pvc 와 kubectl get pv 명령어로 PVC와 PV 상태를 확인합니다.

|   |   |   |
|---|---|---|
1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12|
[root@k8s-master jh]# kubectl apply -f pvc-hostpath.yaml <br><br>persistentvolumeclaim/pvc-hostpath created<br><br>[root@k8s-master jh]# <br><br>[root@k8s-master jh]# <br><br>[root@k8s-master jh]# kubectl get pvc<br><br>NAME           STATUS   VOLUME        CAPACITY   ACCESS MODES   STORAGECLASS   AGE<br><br>pvc-hostpath   Bound    pv-hostpath   2Gi        RWO            manual         5s<br><br>[root@k8s-master jh]# <br><br>[root@k8s-master jh]# kubectl get pv<br><br>NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                  STORAGECLASS   REASON   AGE<br><br>pv-hostpath   2Gi        RWO            Delete           Bound    default/pvc-hostpath   manual                  32m<br><br>[Colored by Color Scripter](http://colorscripter.com/info#e)|
[cs](http://colorscripter.com/info#e)|

**7번 라인의** STATUS 항목은 Bound이고 VOLUME 항목은 pv-hostpath 입니다. 이전에 생성한 PV인 pv-hostpath에 연결되었다는 의미입니다.

또한 11번 라인에서 PV 또한 STATUS가 Bound로 바뀐 것을 확인할 수 있습니다.
