
Azure 연대기 작성 목적의 repo 정리 Read Me (-10월)

1. Terraform 연대기 최대한 써보자.
	1.오늘 resource group 만들었으니 이걸 써봐도 좋겠다. 
    2. backend 도 같이 설정해서 써보자. 
	    1. 왜 쓰는가 ? 협업을 위한 lock & back up 목적
	    2. azure backend 를 선택한 이유? aws는 s3 뿐만 아니라 DynamoDB도 필수적으로 필요하기 때문.
	    3. versioning { enabled = true # Prevent from deleting --tfstate file }
	    4. var 로 직접 넣으면 관리하기 어렵다. 실제로는 local 변수를 사용하자. tf console > local.config
	3.for Each 를 사용하는 이유
	subnet 생성을 예로 들자.

---
