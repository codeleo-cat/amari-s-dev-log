AWS로 배포하는 방법은 아주 많은 글이 올라와 있다. 그런데 Azure는 어떻게 배포를 해야 하나?  # App Service 를 이용하는 배포 방법은 좀 있다. 그런데 좀 더 세부적으로 custom 하게 설정을 하고, 서버리스 형태를 원했기에 방법을 찾아보다가 Azure Container Apps 방식을 택하기로 했다. 
또한 Terraform을 사내에서 도입하여 사용하는 초반이 되었다.

왜 Terraform을 도입하기로 했나? > 링크
	1. 불편해서 도입하기로 했다. console / portal 에서는 resource가 보인다. 그런데 말 그대로 그냥 보이기만 한다. 자원들 사이의 연관관계가 여기 저기 많이 숨어있기도 하다. 그런데 테라폼을 사용하면 이 관계가 보인다. 
	2. 관리하기 쉽다. 팀 전체에 해당 인프라를 설명해야 한다고 가정한다. 
	"container_app.tf 파일 50 line에 ingress = true 값 넣고 terraform apply" 해주세요. 
> "아 콘솔에서 로그인하고, 디렉토리 전환해서, notshydev 리소스 그룹 찾고"
> 3. DR에 대비할 수 있다.


2. 도입하며 느낀 점
	1. 생각보다 느리다... 생각보다 리소스의 생성과 삭제에 시간이 오래 걸린다. 예를 들면 vpn 같은 경우 30분 가량, application gateway의 경우 provisioning까지 25분이 걸린다.


	2. 생각보다 빠르다.
		terraform version
		특히 container app 의 경우 user assign 3.5 까지만 해도 안되었는데 이젠 된다.
		그 주기가 짧게는 며칠에서 평균적으로 체감 1~2주 ?
		그래서 테라폼 버전을 사용할 때는 다른 건 몰라도 이 최신 버전을 사용하는 provider는 꼭 넣기를 권장한다.

	3. 생각보다 똑똑하다
	   cidr 안되는 것들 > 에러를 다 보여준다.
	   뭐는 불가능하다고 보여준다. 
	   naming에서 문제되는 것들 다 집어준다.
	   
	4. 생각보다 안되는 것도 꽤 있다.
		secret 삭제도 안됨.
		하나의 provider로는 안되는 것도 있다. nat gateway 같은 경우 azapi 사용해야 함.

그래도 나는 진짜 추천한다.
만약 terraform 도입기가 궁금하다면 _ 링크

그리고 테라폼 시작 전에 꼭 읽어봤으면 하는 좋은 글
- best practice
- 당근 pay
- terraform 공식 guide
- + 나의 글 ^^


Azure의 아쉬움.
		nat gateway 라고 outbound ip 고정시키기 위한 목적으로 사용했던 것이 있는데, 이게 7월까지만 해도 preview 단계였다. 
		그리고 한국을 지원안했었다.
		지금은 정식 서비스로 오픈된 상태임.
	   
		
결론 Terraform x Azure를 이용하여 infra를 생성하고 관리하고 배포까지!

그런데, 그런데 생각보다 정말 많이 참고할만한 자료가 없었다...
(그나마 공식문서 제외...)
특히 한국어로 글을 정리한 것은 정말 많지 않았고, 오래 전 자료가 대부분이었다. 
원래  처음에는 모든게 어려운 법 아니겠는가? 
이 글은 미래의 나를 위해,
AWS 대신 혹은 AWS와 더불어 DR 등의 목적으로 Azure를 사용하기로 한 이들
Terraform 을 이제 막 도입하기 시작한 Devops 엔지니어를 위한, 
어쩌면 대장정의 첫 시작이 되길.


적절한 서비스 찾기
https://learn.microsoft.com/ko-kr/azure/architecture/guide/technology-choices/compute-decision-tree