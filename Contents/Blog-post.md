---
created: 2024-01-05T10:30
updated: 2024-03-18T11:36
---
- terraform 개선 (시리즈)
- why IaC
- VPN setting
- Azure container app 배포
- Mysql replication - m/m 
- Mysql replication - m/s
- Github Actions 
- CI/CD Tool 선택 기준
- Azure DMS + AWS DMS
- AWS DataSync
- Azure DataFactory
- Tool - obsidian & Icloud & git [참고](https://velog.io/@qkqkwl147/Obsidian-git-sync-%EB%8F%99%EA%B8%B0%ED%99%94)
- 네트워크 관리사 실기 합격 수기 [[실기-router 설정 및 확인]]
---
- Pulumi
- GCP / GKE
- helm
- K8s
- Datadog
- ArgoCD
- Docker
- Istio
---
#블로그 에 대한 생각
많은 Tool이 있다.
velog / github.io / tistory / obsidian (publish)
**medium** 을 선택했다. 
단순한 UI. 내용이라는 본질에 집중하고 싶었다.
TTL (Total Time Reading)

- 블로그에 대한 조언
> 자주 글을 써야 한다는 강박보다는 **메시지가 있는 글**을 써야 한다.
주제보다 중요한 것은 어떠한 정보를 얼마나 **퀄리티있게 잘 전달**하느냐이다.
기술적 어려움에 대한 고찰이 좋고 단순한 TIL이나 남의 글을 복붙하지 않은 글. 기술적 연관이 없는 개인사 등은 지양.

- 알고리즘에 대한 조언
>실무에선 레벨 2 이하의 문제들을 ‘반복’해야 한다.



- [ ] blog 📅 2023-11-30 🔼 
	1. velog에 올린 내용들 > tistory migration
	2. [[Azure로 어떻게 배포를 하는거야?]]
	3. terraform을 학습하면서 공부한 내용들
		1. Why I adapted with terraform logo
		2. 무수한 고민들과 삽질
		3. [[Terraform 개선]]
		4. 서로 다른 네트워크 간 데이터 전송하기
		5. [[terraform-with-az]]


마이그레이션 유형
- 온라인 마이그레이션은 소스 데이터베이스가 마이그레이션하는 동안 계속 사용 가능
	- sku = premium 에서만 지원
- 오프라인 마이그레이션은 마이그레이션 동안 데이터베이스 접근이 중단됩니다.