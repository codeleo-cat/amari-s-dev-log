# Ingress controller 역할을 수행

- service 보호, 연결, 모니터링
- 통신에 대한 측정
- **traffic에 대한 시각화** 
- kiali

**Service Mesh**
- Application에 추가할 수 있는 전용 인프라 계층

**Traffic Control**
- virtual service : 어느 host 혹은 service로 routing 할지, 트래픽(weight)를 어느 정도로 할지
- gateway : 어느 host의 요청, 포트 등을 처리할 지

### Ref
- [Service Mesh - Istio](https://istio.io/latest/about/service-mesh/)
- [istio는-무엇이고-왜-중요할까](https://kr.linkedin.com/pulse/istio%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%99%9C-%EC%A4%91%EC%9A%94%ED%95%A0%EA%B9%8C-sean-lee)