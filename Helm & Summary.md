---
created: 2024-01-25T10:34
updated: 2024-01-30T11:55
---

- 어플리케이션 코드가 올라가 있는 [App repository](https://github.com/iamchoiz/demo-app-gitaction-argocd)
	코드를 컨테이너로 빌드하여 Artifact Repository로 PUSH
- 어플리케이션 Manifest(k8 POD를 만들기 위한)가 존재하는 Config Repo. k8 Resource 배포를 위한 Kustomize 형식의 yaml 파일들입니다.

## Advanced Helm chart 만들기 - chart hook편

Helm chart 의 Life cycle 을 이해하면 조금 더 고급스러운 chart 를 만들 수 있다. 예를 들면 이전 chart 의 애플리케이션에서 postgresql DB를 사용하기 때문에 사용자, 데이터베이스, 테이블을 생성해야 하는 경우를 생각해 보자. 차트가 배포될 때 애플리케이션이 실행되기 이전에 해당 작업들을 할 수 있다면 chart 배포 한 번으로 모든 배포를 끝낼 수 있으니 멋진 일이다.

#### Helm 에서는 chart 가 실행되기 이전에 혹은, 실행된 이후에 작업을 정의할 수 있도록 hook 기능을 제공한다.

이러한 hook 의 종류는 아래와 같다.

|Annotation 값|설 명|
|---|---|
|`pre-install`|Executes after templates are rendered, but before any resources are created in Kubernetes|
|`post-install`|Executes after all resources are loaded into Kubernetes|
|`pre-delete`|Executes on a deletion request before any resources are deleted from Kubernetes|
|`post-delete`|Executes on a deletion request after all of the release's resources have been deleted|
|`pre-upgrade`|Executes on an upgrade request after templates are rendered, but before any resources are updated|
|`post-upgrade`|Executes on an upgrade request after all resources have been upgraded|
|`pre-rollback`|Executes on a rollback request after templates are rendered, but before any resources are rolled back|
|`post-rollback`|Executes on a rollback request after all resources have been modified|
|`test`|Executes when the Helm test subcommand is invoked ( [view test docs](https://helm.sh/docs/chart_tests/))|

hook 을 정의하는 것은 Annotation 으로 하며, 앞에서 설명한 시나리오의 경우에는 pre-install hook 을 사용하는 것이 적절하기 때문에 "helm.sh/hook": pre-install 로 설정한다. 또한 이런 단발성 호출의 경우에는 Job 으로 설정하는 것이 좋다.

```
apiVersion: batch/v1
kind: Job
metadata:
...
  annotations:
    "helm.sh/hook": pre-install
    "helm.sh/hook-weight": "1"
    "helm.sh/hook-delete-policy": before-hook-creation
```

hook-weight 는 낮을 수록 우선순위가 높으며, hook-delete-policy 의 before-hook-creation 은 hook 으로 실행된 리소스를 다음 hook 이 실행될 때 까지 지우지 않고 남겨둔다는 의미이다. policy 는 아래와 같이 3가지가 있다.

|Annotation Value|Description|
|---|---|
|`before-hook-creation`|Delete the previous resource before a new hook is launched (default)|
|`hook-succeeded`|Delete the resource after the hook is successfully executed|
|`hook-failed`|Delete the resource if the hook failed during execution|

그럼 postgresql 을 설치하고 사용자, 데이터베이스, 테이블을 생성하는 pre-install hook 을 작성해 보자.

## 1. postgresql 설치

bitnami chart repo 에서 postgresql 을 다운받아 설치한다.

```
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm repo update
$ helm upgrade -i postgresql bitnami/postgresql --version 10.8.0 -n decapod-db --create-namespace \
--set postgresqlPassword=password \
--set persistence.enabled=true \
--set persistence.storageClass=rbd \
--set persistence.size=10Gi
```

chart 의 value 값을 필요한 부분만 override 하였다. db user 는 postgres 이며 db password는 password 로 설치했다. 테스트 환경에서는 외부 스토리지를 제공하고 있어 10Gi 로 설정하였다.

```
$ kubectl get pods -n decapod-db
NAME                      READY   STATUS    RESTARTS   AGE
postgresql-postgresql-0   1/1     Running   0          35h


$ kubectl get svc -n decapod-db
NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
postgresql            ClusterIP   10.233.46.14           5432/TCP   35h
postgresql-headless   ClusterIP   None                   5432/TCP   35h
```

## 2. pre-install hook 만들기

chart 가 설치될 때 한 번 실행되면 되므로 Job 타입으로 리소스를 작성한다. psql 명령을 사용할 수 있는 컨테이너 이미지를 활용하고 수행할 명령어들은 쉘 스크립트로 작성하였다.

```
{chart_source_home}/templates/pre-install-job.yaml

apiVersion: batch/v1
kind: Job
metadata:
  name: {{ include "tks-contract.fullname" . }}
  namespace: {{ .Values.namespace }}
  labels:
    {{- include "tks-contract.labels" . | nindent 4 }}
  annotations:
    "helm.sh/hook": pre-install
    "helm.sh/hook-weight": "-5"
    "helm.sh/hook-delete-policy": before-hook-creation
spec:
  template:
    metadata:
      name: {{ include "tks-contract.fullname" . }}
    spec:
      restartPolicy: Never
      containers:
      - name: pre-install-job
        image: "bitnami/postgresql:11.12.0-debian-10-r44"
        env:
        - name: DB_ADMIN_USER
          value: {{ .Values.db.adminUser }}
        - name: PGPASSWORD
          value: {{ .Values.db.adminPassword }}
        - name: DB_NAME
          value: {{ .Values.db.dbName }}
        - name: DB_USER
          value: {{ .Values.args.dbUser }}
        - name: DB_PASSWORD
          value: {{ .Values.args.dbPassword }}
        - name: DB_URL
          value: {{ .Values.args.dbUrl }}
        - name: DB_PORT
          value: "{{ .Values.args.dbPort }}"
        command:
        - /bin/bash
        - -c
        - -x
        - |
          # check if ${DB_NAME} database already exists.
          psql -h ${DB_URL} -p ${DB_PORT} -U ${DB_ADMIN_USER} -lqt | cut -d \| -f 1 | grep -qw ${DB_NAME}
          if [[ $? -ne 0 ]]; then
            psql -h ${DB_URL} -p ${DB_PORT} -U ${DB_ADMIN_USER} -c "CREATE DATABASE ${DB_NAME};"
          fi

          # check if ${DB_USER} user already exists.
          psql -h ${DB_URL} -p ${DB_PORT} -U ${DB_ADMIN_USER} -tc '\du' | cut -d \| -f 1 | grep -qw ${DB_USER}
          if [[ $? -ne 0 ]]; then
            psql -h ${DB_URL} -p ${DB_PORT} -U ${DB_ADMIN_USER} -c "create user ${DB_USER} SUPERUSER password '${DB_PASSWORD}';"
          fi

          # check if contracts table in tks database already exists.
          psql -h ${DB_URL} -p ${DB_PORT} -U ${DB_ADMIN_USER} -d ${DB_NAME} -tc '\dt' | cut -d \| -f 2 | grep -qw contracts
          if [[ $? -ne 0 ]]; then
            echo """
              \c ${DB_NAME};
              CREATE TABLE contracts
              (
                  contractor_name character varying(50) COLLATE pg_catalog."default",
                  id uuid primary key,
                  available_services character varying(50)[] COLLATE pg_catalog."default",
                  updated_at timestamp with time zone,
                  created_at timestamp with time zone
              );
              CREATE UNIQUE INDEX idx_contractor_name ON contracts(contractor_name);
              ALTER TABLE contracts CLUSTER ON idx_contractor_name;
              INSERT INTO contracts(
                contractor_name, id, available_services, updated_at, created_at)
                VALUES ('tester', 'edcaa975-dde4-4c4d-94f7-36bc38fe7064', ARRAY['lma'], '2021-05-01'::timestamp, '2021-05-01'::timestamp);

              CREATE TABLE resource_quota
              (
                  id uuid primary key,
                  cpu bigint,
                  memory bigint,
                  block bigint,
                  block_ssd bigint,
                  fs bigint,
                  fs_ssd bigint,
                  contract_id uuid,
                  updated_at timestamp with time zone,
                  created_at timestamp with time zone
              );
            """ | psql -h ${DB_URL} -p ${DB_PORT} -U ${DB_ADMIN_USER}
          fi
```

```
{chart_source_home}/values.yaml

args:
  port: 9110
  dbUrl: postgresql.decapod-db.svc
  dbPort: 5432
  dbUser: tksuser
  dbPassword: password

db:
  adminUser: postgres
  adminPassword: password
  dbName: tks
```

컨테이너에서 필요한 값들(db superuser admin 과 password, db url, db port 등)은 env 로 전달한다. 참고로 postgresql 은 psql 로 접속할 때 패스워드를 전달하는 아규먼트가 없다. 대신 환경 변수로 다음과 같이 설정하면 패스워드를 사용하여 접속할 수 있다. ($ export PGPASSWORD=password)

1. 먼저 DB 에 해당 database 가 존재하는 조회하고 없으면 database 를 생성한다.
2. DB User 가 존재하는지 조회하고 없으면 새로운 DB User 를 생성한다.
3. Table 이 존재하는지 조회하고 없으면 Table 을 생성한다. Table 을 생성하기 위해 psql 로 접속 시에 해당 database 에 접속할 수 있지만 (-d database명 옵션 사용), "\c database명" 으로 database 를 선택할 수 있다는 것을 보여주기 위해 이 부분을 추가하였다.

chart 를 배포하면 다음과 같이 job 이 실행됨을 알 수 있다.

```
$ kubectl get pods -n tks
NAME                               READY   STATUS      RESTARTS   AGE
tks-contract-h5468                 0/1     Completed   0          31h


$ kubectl logs tks-contract-h5468 -n tks
+ psql -h postgresql.decapod-db.svc -p 5432 -U postgres -lqt
+ cut -d '|' -f 1
+ grep -qw tks
+ [[ 0 -ne 0 ]]
+ psql -h postgresql.decapod-db.svc -p 5432 -U postgres -tc '\du'
+ cut -d '|' -f 1
+ grep -qw tksuser
+ [[ 0 -ne 0 ]]
+ psql -h postgresql.decapod-db.svc -p 5432 -U postgres -d tks -tc '\dt'
+ cut -d '|' -f 2
+ grep -qw contracts
+ [[ 0 -ne 0 ]]
```