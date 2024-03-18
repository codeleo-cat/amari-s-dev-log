---
created: 2024-02-23T15:29
updated: 2024-02-23T17:41
---
terraform azurerm version 3.93.0 에서는 옵션이 제한적이어서 설정 불가.

마이그레이션 유형
- 온라인 마이그레이션은 소스 데이터베이스가 마이그레이션하는 동안 계속 사용 가능
	- sku = premium 에서만 지원
- 오프라인 마이그레이션은 마이그레이션 동안 데이터베이스 접근이 중단됩니다.

// replica와 원본 값이 다름
call mysql.rds_set_configuration('binlog retention hours', 24);