## Docker 최적화 방법

- 

## Docker run VS Docker exec

- debug

## Docker 사용 시 장점

- **이식성
	- 어떤 OS 환경에서든 동일하게 동작한다. (environment disparity 해결)
- **자원 효율성
	- 모든 dependency, code를 #container 에 패키징한다. VM보다 훨씬 적은 자원을 사용하고 가볍다. (VM 대비해서 Guest OS layer가 없기 때문에 적은 overhead, 작은 이미지 사이즈 > 부팅이 빠르다. 하나의 Host OS를 공유하고, 이 커널 위에 개별 프로세스와 환경을 격리화 시키는 방식이다.)
- **배포 관리 용이성
	- 여러 컨테이너로 구성된 애플리케이션을 쉽게 관리 가능하다. ex) docker compose

