# 앱을 어떠한 환경에서든 신속하게 **배포**, **확장**, **실행**될 수 있도록 하는 서비스를 제공하는 s/w 플랫폼.

- 리눅스의 응용 프로그램들을 프로세스 격리 기술들을 사용해 컨테이너로 실행하고 관리하는 s/w 플랫폼.
- Docker는 Host OS의 커널 위에 격리를 시켜주는 **반가상화 시스템**
- Docker 컨테이너는 Host 시스템의 리소스를 공유하고 격리된 환경에서 실행된다.

[Docker 공식문서](https://docs.docker.com/)
[Docker CMD & elements](https://ivdevlog.tistory.com/9)

## Docker 최적화 방법

- base image로 alpine 을 사용한다. [alpine](https://namu.wiki/w/Alpine%20Linux)

## Docker run VS Docker exec

- docker run - 이미지 &rarr; 컨테이너로 변환되어 하나의 인스턴스화
- `docker run` 명령어는 **새로운** 컨테이너를 생성하고 실행하는 데 사용된다.
- `docker exec` 명령어는 **이미 실행 중**인 컨테이너에서 추가적인 프로세스나 명령을 실행하는 데 사용된다.

```
docker exec -it <container_id> bash
```
### [[Docker Compose]]

- 여러 도커 컨테이너를 조합해서 기동할 수 있도록 만든 개발자용 오케스트레이션 tool (=상호 의존하는 여러 도커 컨테이너를 빌드하여 실행한다.)
### Docker Swarm

- 복수의 노드로 클러스터를 구성하는 tool

## Docker 사용 시 장점

- **이식성**
	- 어떤 OS 환경에서든 동일하게 동작한다. (environment disparity 해결)
- **자원 효율성** (🔑 더 가볍고 빠르게 실행된다.)
	- 모든 dependency, code를 #container 에 패키징한다. VM보다 훨씬 적은 자원을 사용하고 가볍다. (VM 대비해서 Guest OS layer가 없기 때문에 적은 overhead, 작은 이미지 사이즈 > 부팅이 빠르다. 하나의 Host OS를 공유하고, 이 커널 위에 개별 프로세스와 환경을 격리화 시키는 방식이다.)
- **배포 관리 용이성**
	- 여러 컨테이너로 구성된 애플리케이션을 쉽게 관리 가능하다. ex) docker compose
- VM
![vm|350](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F213CAE3D5265549405)
- **Docker**
![docker|350](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F272F203F5265549F04)
### ECS 컨테이너 구동이 계속 실패한다면 시도할 수 있는 방법 [AWS docs](https://repost.aws/ko/knowledge-center/ecs-task-container-health-check-failures)
1. Local에서 돌려본다.
		Amazon ECS에 프로비저닝하기 전에 컨테이너를 로컬에서 테스트하여 컨테이너 상태 확인을 통과하는지 확인 ex) Dockerfile health check
2. Log를 확인한다.
		Amazon ECS 작업이 오랜 시간 동안 계속 실행되는 경우 애플리케이션 로그와 Amazon CloudWatch Logs를 확인