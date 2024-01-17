#kubernetes 
# [[Kubernetes]]에서 애플리케이션을 쉽게 배포하기 위해 사용되는 대표적인 패키지 매니저


### Chart

- Helm package. 
- K8s 클러스터 내에서 애플리케이션, 도구, 서비스 구동하는데 필요한 모든 리소스 정의가 포함되어 있다.
### Repository 

- 차트를 모아두고 공유하는 장소
### Release

- K8s 클러스터에서 구동되는 차트의 인스턴스.

### Helm CLI

- Helm status `...`
- Helm repo add `repo_name`
- Helm repo update
- Helm repo list




helm install mysql stable/mysql \
  --set mysqlRootPassword=root1234! \
  --set mysqlUser=testuser \
  --set mysqlPassword=pwd1234! \
  --set mysqlDatabase=helmktest



MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)

kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il

apt-get update && apt-get install mysql-client -y

exit

mysql -h mysql -p root1234!


###



### Helm upgrade & Helm rollback

- 새로운 버전의 차트가 release 되었을 때, release의 구성을 변경하고자 할 때


Helm v3 vs v2

helm은 K8s 내부에 charts를 설치하고, 각 설치에 대해 새로운 release를 생성한다. <br/> 새로운 chart를 찾기 위해 Helm Chart Repository를 검색할 수 있다.

### Ref
[Helm 공식 Docs](https://helm.sh/)
