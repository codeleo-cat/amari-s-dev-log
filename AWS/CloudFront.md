#AWS #CDN 
## Content Delivery Network
- web page, image, video 등의 컨텐츠를 캐싱하여 유저에게 제공함.
- global service (region은 us-east-1)

**Edge location**에서 데이터를 캐싱.
- 원본의 copy를 가지고 있다는 의미. 그래서 원본이 만료되어도 캐싱된 데이터는 남아있다.

Static Contents - html/js (CloudFront에서 처리). 안 변하는 것들
Dynamic Contents - login이 필요한 내용, 게시판, 댓글 등

### Terms
- Origin (원본) : 실제 컨텐츠가 존재하는 서버 (S3, EC2)
	- S3 Origin
	- Custom Origin - S3를 제외한 다른 모든 Origin. 
		- failover 대비하여 복수 개의 Origin 접근 가능.
		- IP 주소는 사용 불가. **Domain만 사용 가능**
		- Primary 에서 500 response가 떨어진다 > Secondary로 자동 요청.

- Distribution (배포) : CDN 구분 단위
- Behavior : protocol, cashing 정책 등 어떻게 컨텐츠를 전달할지 결정

Caching - 2 tier (2 계층 구조)
요청 순서 : viewer -> Edge location -> Regional edge cache -> Origin

**Cache Hit** : Edge location에 해당 cache object를 가지고 있어, Origin까지 요청하지 않아도 컨텐츠 제공 가능. 이 cache hit이 **높을수록** 좋다. 캐시히트와 반대되는 것이 cache miss

**Cache Key** : http://example.com/image.png?color=blue
color=blue에 해당. (query string) 잘 구성해야 됨.

Cache TTL : Cache object를 얼마나 오래 보관할지에 대한 설정.


Cloud Front에는 파일 압축 기능이 있다.
- gzip
- brotli

---
[(LV.300)Amazon CloudFront 완벽 정리 정리 (2) : CloudFront Origin(원본))](https://www.youtube.com/watch?v=oa-6f8QyFyo)
[(LV.300)Amazon CloudFront 완벽 정리 정리 (3) : CloudFront Caching(캐싱)](https://www.youtube.com/watch?v=oa-6f8QyFyo)

