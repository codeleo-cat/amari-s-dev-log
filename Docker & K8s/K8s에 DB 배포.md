# MySQL
- Helm으로 K8s Cluster에 MySQL 설치

**Initialize a Helm Chart Repository**
```bash
helm version
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo list
```

**Install an Example Chart**

```bash
kubectl create ns database
kubectl get ns

helm install -ns database mysql bitnami/mysql
MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace database mysql -o jsonpath="{.data.mysql-root-password}" | base64 -d)
echo $MYSQL_ROOT_PASSWORD

kubectl run mysql-client --rm --tty -i --restart='Never' --image  docker.io/bitnami/mysql:8.0.36-debian-11-r0 --namespace database --env MYSQL_ROOT_PASSWORD=$MYSQL_ROOT_PASSWORD --command -- bash

echo $MYSQL_ROOT_PASSWORD
pI3O0ZOt4J

mysql -h mysql.database.svc.cluster.local -uroot -p"$MYSQL_ROOT_PASSWORD"
(접속 확인 후)
```

ctrl + D 2번 눌러 빠져 나간다.

**namespace `database`에 배포된 리소스 확인**

```
helm ls --namespace database
kubectl get po -n database
kubectl port-forward mysql-0 3306:3306 -n database
mysql -u root -h 127.0.0.1 -p

```
## Application deploy through Github Actions & Secret 적용

- [Github Actions by ARC](https://tech.buzzvil.com/blog/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4%EC%97%90%EA%B2%8C-github-actions-%EC%84%A4%EC%B9%98%EC%97%90-%EB%8C%80%ED%95%B4-%EB%AC%BB%EB%8B%A4/)
- https://velog.io/@alli-eunbi/Git-EKS-ARC-세팅-방법
- https://ykarma1996.tistory.com/195

- vm처럼 단일 instance에 github action runner를 설치할 수도 있지만, #ARC 를 사용해 K8s 위에 설치할 수 있다. (= 쿠버네티스 파드로 러너 실행 가능.)
	- 장점 : 비용 효율성 & 확장성 & 속도
```
helm install arc --namespace arc --create-namespace \ oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set-controller

```

# Postgres

# Redis

# Qdrant






### Ref
- [[Kubernetes] 쿠버네티스에 데이터베이스 배포하고 애플리케이션 연결하기(Secret 사용)](https://dev-scratch.tistory.com/179)