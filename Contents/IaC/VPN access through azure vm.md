
~~~
# vpn connection 확인
az network vpn-connection show --name vpn-gw-connection --resource-group rg-sdp-dr
~~~

vm-bastion 접속 
username : "azureuser"
password: (key vault secret 참조)

sudo apt-get update
sudo apt-get install mysql-client
sudo apt-get install mysql-server
mysql -V

sudo apt install awscli
aws configure

access_key_id: AKIA2THMGYSXGMCMTLOI
secret_access_key: 

mysql -h rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com -u sqlAdmin -p

kdjPIOIDJvjeifj09892jfjKjdjE30

만약 접속이 오랫동안 안되면 
1. aws / azure 중 한 곳에
subnet > route table > Virtual private gateway 등록되어 있는지
2. vpn 이 잘 연결되어 있는지 확인

show databases;

- s3 정책 편집 - terraform IaC

https://repost.aws/ko/knowledge-center/rds-connectivity-instance-subnet-vpc

https://docs.google.com/document/d/12CbkConvEAikmQO2na2sa_Sj0UYct4Ph7qSp-Zkww38/edit

10/17 vpn 관련.
아 확실히 나의 이해가 부족했음을 느낀다.
제대로 안되는 이유가 있었다. 제대로 몰랐기 때문이다.
이건 정말... 뭐랄까 
다 알고 있는 게 아니었다. 내일은 깔끔하게 정리를 하자.
정리를 한 후 오전에 한번 그려보자. 정말 필요한 절차다.


10/18
1) vpn 리소스 관련 : vpc_id만 data로 가져와서 생성하는 방식으로 변경.
	1) sg, route_table 등 terraform IaC 관리
	2) 추후 vpn connection success 시 slack webhook 등 알림 설정
3) Bastion Subnet + Bastion Host 생성하는 resource 블럭 추가. 해당 Bastion host를 vm - bastion 으로 활용. 
	 vm - Subnet을 분리함. 

10/19
1) 기존 구조 : adf-vm (windows에 shir 설치)& nat gateway 사용함. 
	1) 목적 - nat gateway 통한 outbound ip 고정 (managed 환경은 public ip 대역대가 주기적으로 변경되어 이 대역대를 모두 s3 bucket policy에 등록해주어야 했었음)
	2) AWS OIDC 인증 및 access key 등을 주기적으로 교체하는 목적. 
2) TEST 진행 중 - vpn & interface end point & linux vm에서 oidc script 동작시키는 방향.
3) Azure 권장에 따라, key vault access policy -> RBAC 방식으로 변경 적용 중

Users > sdp-azure-adf

AKIA2THMGYSXGMCMTLOI
jeYBTGQKUeGCEjGy73lG8oqa8db89D8wsaeA0FxC

----

- [x] 테일러
	- [ ] I've delved into my insecurities in this far, in this detail
	- [ ] We all hate about ourselves
	- [ ] don't feel bad for me
	- [ ] It's really honest
- [x] 기생충
	- 도화선 catalyst
	- just shoot right away
	- 골목길 alleyway(s)
- [x] 오늘의 shorts 하나
	- [ ] I'm just out / running some errands, what's up?
	- [ ] I'm on the subway / on my way home (to work)
	- [ ] pursue perfection vs progress
	- [ ] you really shouldn't have 뭘 이런 걸 다 준비했어~😌
	- [ ] I know, but I wanted to get you sth. sorry I didn't have time to wrap it
	- [ ] 이럴 줄 알았어. **I saw this coming**
	- [ ] 왠지 이런 일이 생길 것 같았어. I had a feeling sth like this would happen
	- [ ] He looked so **wacky**🤪 
	- [ ] ~하는 거 오랜만이야. 
		- 1. ~할거야 (I'm gonna meet up with some friends)
		- 2. **We haven't done that in so long.**
	

- [x] 코테 1개 
	- [ ] lambda 식에 대해 연습
- 운동
---

1. 가장 크게 달라진 점
-> ADF를 사용 시 외부 인터넷 통하지 않고 vpce 를 통해 접근한다.

- 추가 보안
	- storage 관련해서 vnet + 특정 ip로만 통신하도록 한다

1. access policy에서 rbac으로 접근한다.
2. module을 사용한다.
