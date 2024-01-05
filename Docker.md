# Docker

- 리눅스의 응용 프로그램들을 프로세스 격리 기술들을 사용해 컨테이너로 실행하고 관리하는 s/w 플랫폼.
&rarr; 앱을 어떠한 환경에서든 신속하게 **배포**, **확장**, **실행**될 수 있도록 하는 서비스를 제공하는 s/w 플랫폼.

## Docker 최적화 방법

- alpine 

## Docker run VS Docker exec

- docker run - 이미지 &rarr; 컨테이너로 변환되어 하나의 인스턴스화


### Docker Compose
- 여러 도커 컨테이너를 조합해서 기동할 수 있도록 만든 개발자용 오케스트레이션 tool (=상호 의존하는 여러 도커 컨테이너를 빌드하여 실행한다.)
### Docker Swarm
- 복수의 노드로 클러스터를 구성하는 tool

## Docker 사용 시 장점

- **이식성**
	- 어떤 OS 환경에서든 동일하게 동작한다. (environment disparity 해결)
- **자원 효율성**
	- 모든 dependency, code를 #container 에 패키징한다. VM보다 훨씬 적은 자원을 사용하고 가볍다. (VM 대비해서 Guest OS layer가 없기 때문에 적은 overhead, 작은 이미지 사이즈 > 부팅이 빠르다. 하나의 Host OS를 공유하고, 이 커널 위에 개별 프로세스와 환경을 격리화 시키는 방식이다.)
- **배포 관리 용이성**
	- 여러 컨테이너로 구성된 애플리케이션을 쉽게 관리 가능하다. ex) docker compose

![docker vs vm](https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_2-VM-vs-Containers.78f841efba175556d82f64d1779eb8b725de398d.png)


##### ECS 컨테이너 구동이 계속 실패한다면 시도할 수 있는 방법 [AWS docs](https://repost.aws/ko/knowledge-center/ecs-task-container-health-check-failures)
1. Local에서 돌려본다.
		Amazon ECS에 프로비저닝하기 전에 컨테이너를 로컬에서 테스트하여 컨테이너 상태 확인을 통과하는지 확인 ex) Dockerfile health check
2. Log를 확인한다.
		Amazon ECS 작업이 오랜 시간 동안 계속 실행되는 경우 애플리케이션 로그와 Amazon CloudWatch Logs를 확인