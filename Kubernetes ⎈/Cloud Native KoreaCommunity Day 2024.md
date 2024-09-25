---
tags:
  - k8s
created: 2024-09-24T13:03:00
tag:
---
https://kcd-korea.net

| Time          | Loc     | Speaker | Topic                                                       |
| ------------- | ------- | ------- | ----------------------------------------------------------- |
| 13:00-13:35   | Track 1 | 유저스틴    | 클라우드 네이티브 오케스트레이터만 있으면 고통 끝, 행복 시작!                         |
| 13:35-14:10   | Track 1 | 황인환     | kubernetes 에서 kafka 활용하기                                    |
| 14:10 - 15:15 | Track 1 | 안종석,정백균 | Cloud Native 서비스의 Multi-AZ/Multi-Region 환경을 위한 HA           |
| 15:15-15:35   | Track 1 | 김범승     | 엣지 컴퓨팅을 위한 2노드 HA 쿠버네티스                                     |
| 15:35 - 16:10 | Track 1 | 유지연     | OpenTelemetry 기반 하이브리드 클라우드 Kubernetes 환경의 Observability 구축 |
| 16:30-17:05   | Track 2 | 강상진     | 안전한 쿠버네티스 운영을 위한 보안 Best Practice 소개                        |
| 17:05 - 17:40 | Track 1 | 문지현     | 선언형으로 만드는 멀티 클러스터 - Cluster API 와 App of Apps 패턴            |

Cloud Native란?
사용자가 설정해야 하는 모든 것을 자동화 하는 것. `또한 desired state와 current state를 트래킹하며 맞추는 것.`

**.NET Aspire** - 클라우드 네이티브 애플리케이션 (컨테이너만 있다면 어떤 앱이라도 오케스트레이션 가능하다.)
- Service discovery - WithReference 활용
- Integration - redis, rabbitMQ, AWS, Azure 
	ex) AddPostgres // 자동으로 db connection pull을 만들어준다.
- Flexible Integrations & Deployment
	ex) AddContainer("react-app")
---
kubernetes에서 kafka 활용하기
신한카드 내부에서 spring 기반 사용.
kafka (onpremiss에 설치하기도 어렵고, scale up 시 issue가 있음)
**strimzi** - kafka를 k8s cluster 에서 실행하도록 설계된 프로젝트
kafka를 CDC로 많이 사용 (Apicurio Registry를 통해서 *직렬화* (데이터 구조나 객체를 저장 혹은 전달 가능한 형태로 변환하는 것)/역직렬화 등을 할 수 있음.)

---
Cloud Native 서비스의 Multi-AZ/Multi-Region 환경을 위한 HA

eBPF 기반의 loxiLB - Kubernetes Master CNI에 직접 접속.
loxilb는 eBPF 기반 클라우드 네이티브를 위한 오픈소스 로드밸런서. (Golang으로 개발됨.)
기존 하드웨어 형태의 로드밸런서와 동일한 동작을 지원하며,  
주로 온프레미스, 엣지 및 퍼블릭 클라우드 Kubernetes 클러스터 배포를 지원하도록 설계되었습니다.
L7 LB 기능 + ingress + Security + Proxy 
[LoxiLB](https://www.loxilb.io/)

Warm standby 로 구성됨.

---
엣지 컴퓨팅을 위한 2노드 HA 쿠버네티스

엣지 컴퓨팅이란?
엣지 컴퓨팅(Edge Computing)은 클라우드 컴퓨팅과 반대되는 개념으로, 인터넷이 아닌 로컬 장치(예: 스마트폰, 태블릿, IoT 장치 등)에서 데이터를 처리하는 기술

kubernetes만으로 2 Node HA는 불가능하고 여러 오픈 소스 프로젝트가 필요했음.

---
Opentelemetry (OTel) : Observability framework
- 표준화 : 일관된 방식으로 observability 데이터 수집
- 확장성 : 분산 아키텍처에서 확장 가능한 방식
- 다양한 언어 지원
	- 구성
		- Otel Specification (명세)
		- Otel Collector (수집기)
		- Otel Instrumentation (로그 수집)
- 전체 구조 : Alloy -> Otel Collector -> Loki, Mimir, Tempo -> Grafana

---
안전한 쿠버네티스 운영을 위한 보안 Best Practice 소개
보안 3요소 (CIA) 
- C - 비밀 데이터 보호, RBAC, network policy (pod 간, service 간 통신 제어)
- I - 이미지 무결성 검증, security context, application integrity 모니터링
- A - 리소스 제한과 QoS(Quality of Service, 서비스 품질) 관리, 자동 복구, 중앙 집중식 로깅 및 모니터링
- 컨테이너 보안
	런타임 보안 : Docker, Containerd
	이미지 보안 : 이미지 스캔, 서명 및 취약점 관리
	Cgroups, Namespace 격리
	Sandbox에서의 container 격리 실행 (미리 실행)
	권한 제한 : root 사용 제한, 권한 최소화
- 네트워크 및 서비스 보안
	네트워크 트래픽 제어 : CNI 플러그인, Calico, Weave
	네트워크 정책 (Ingress, Egress 제어)
	CoreDNS 보안, DNS 암호화
	Service Mesh 보안 : Istio, Linkerd
- 로그 및 모니터링
	모니터링 시스템 구성 : Prometheus, Grafana
	컨테이너 로그 수집 : Fluentd, ELK
	애플리케이션 성능 모니터링 : APM
	
단순히 **로컬** 파일 또는 디렉토리를 Docker 이미지로 **복사**하려는 경우에는 `COPY`
`ADD` can extract compressed files (ex. XXX.tar) and copy files from a remote location via a URL.