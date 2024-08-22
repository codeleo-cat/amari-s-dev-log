export do="--dry-run=client -o yaml"

echo 'source <(kubectl completion zsh)' >> ~/.zshrc
source ~/.zshrc

9)
잠시 kube-scheduler를 멈춰라.
k -n **kube-system** get po | grep schedule
-> 해당 file을 mv

pod를 만드는데 어떤 node에도 scheduled 되어서는 안된다.
이제 pod를 `A` node에 스케줄링하라.
-> 위의 file을 다시 원 자리로 mv

10)
RBAC ServiceAccount Role RoleBinding
**sa** = service account
k auth can-i create secret 등으로 항상 확인할 것.
role 생성 시 --verb=단수, 단수 --resource=단수
전부 단수다.
rolebinding할 때 sa 같은 경우 {namespace}:{sa} 로 해야 한다.

11)
DaemonSet은 명령형으로 선언 불가능하다.
그러니 deploy 형식으로 먼저 만들고, replicas, strategy 등의 부분을 지우자.

12)
node selector와 비슷하지만, 좀 더 flexible한 nodeAffinity
- required
- preferred
podAntiAffinity 
topologyKey


reserving a persistentvolume
- 1번과 2번 example을 적절히 섞자. ns 설정해야 한다는 것. 
persistent volumes

pod에서 volumeMounts 하는 부분과 똑같다.


