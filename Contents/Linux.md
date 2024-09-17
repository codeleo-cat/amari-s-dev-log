---
created: 2024-01-17T12:23
updated: 2024-03-07T09:59
---
#Linux

### Rsyslog (Rocket Fast System for Log processing)
server 내 생성되는 로그를 로그파일이나 콘솔, 외부 서버로 저장할 수 있게 해주는 프로그램


##### 리눅스는 Swap 파티션 설정을 권장한다.

- Swap 파티션이 하는 일? 
	1. ==메모리(RAM)의 범람 시 댐의 역할==을 한다. = 메모리가 완전히 가득 차면 추가적으로 이 스왑 파티션에서 프로그램이 실행된다. = 일종의 예비 공간
	2. 시스템이 최대 절전 모드로 들어갈 때마다 메모리의 내용을 보관하는 장소
	3. RAM에서 더 많은 공간을 사용할 수 있게 '드물게 필요한 항목'을 옮길 수 있도록 함.

#swap

- MMU 기법


- 하나 정도는 꼭 있어야 하나?
	- 위의 장점도 있지만 단점도 있다.
	1. 동적으로 크기를 조정할 수 없어 ==하드디스크의 공간을 차지==
	2. 반드시 성능을 향상시켜주지는 않는다.

- 크기는 어느 정도인 게 좋나?
	- 적어도 설치된 RAM의 크기와 같아야 하고, Swap 파티션에 이미 옮겨져 있는 여러 항목을 위해 추가적으로 10-25%의 공간을 더 줘야 함.
	- 컴퓨터에서 **최대 절전 기능**을 활용하고 싶다면 ..

##### 리눅스는 프로세스들이 요청하는 메모리를 제외한 메모리들 중 거의 대부분을 disk cache로 사용한다.

---
#### 파일 및 디렉토리 관리
- 원본폴더를 통째로(즉 하위폴더 및 파일들을 포함하여) 목적지폴더로 복사
	cp -r `source directory` `destination directory`/
- mkdir `directory`
- mkdir -p `상위directory`/`하위directory`
- mv `file` `new_file`

#### 메모리 확인
- top (메모리 사용량 모니터링. 프로세스 당 메모리 & CPU 사용량)
- df -h (디스크 사용량, 단위 포함해서 쉽게 보여줌)
- free

#### 확인
- ls -a
- cat -ln `file` # row number 확인
```vim
     1 apple
     2 banana
     3 blueberry
     4 kiwi
```
- head -1 `fiile`
위에서부터 한 줄 출력 (head -n `fiile` )
```vim
apple
```

- tail -1 `fiile`
아래에서부터 한 줄 출력 (tail -n `fiile` )
```vim
kiwi
```

---
#### 파일 권한 변경
- chmod 700 `file` 
	: 사용자(본인)에게만 모든 권한을 준다.

- r (읽기, read) : 4
- w (쓰기, write) : 2
- e (실행, execute) : 1

---
- 파일 제거
	- rm
- 이동 
	- mv
- 파일 종류(type) 확인
	- file
- 복사
	- cp
- 네트워크 연결 상태, *라우팅 테이블*, 인터페이스 상태 확인
	- netstat
- 자주 쓰이는 명령어를 쓰기 편하게 바꾸기 
	- alias
- 현재 프로세스들의 상태 표시
	- ps
- ps -aux (모든 user의 process 상태를 볼 때.)
	- a : 터미널에 연관된 프로세스 출력
	- u : 프로세스 사용자/소유자 (user)
	- x : 터미널 세션이 끊겨도 구동되는 프로세스 출력
- ps -ef
	- Process Status -e (모든 사용자들이 구동시킨 process) -f (full format; 보다 상세하게)
- 계정 생성 시 패스워드 생성
	- passwd
- (전체) 파일 시스템의 구조와 용량을 보여주기 (현재 *설치*된 하드디스크 용량 확인)
	- df (disk free)
- 파일 시스템의 하드 사용량을 체크
	- du
- CPU 메모리 사용 정보 확인
	- top
- 현재 시스템에 login 중인 사용자의 list를 보여줌
	- who
- 파일과 디렉토리의 사용 권한을 부여
	- chmod
- 명령어 x에 대한 *매뉴얼* 페이지 얻기 (특정 명령어에 대한 설명)
	- man
- 프로세스를 *중지*
	- kill
- 강제 종료
	- reboot, shutdown -r now, init 0
- 현재 위치 확인 
	- pwd
- windows의 ipconfig 명령어와 같이 ip address 정보를 확인
	- ifconfig
- (사용자에 대한) 파일이나 폴더를 찾을 때 사용
	- find
- 포트/프로토콜 정보 확인
	- etc/ services
- 부팅 메뉴를 선택, 선택된 커널을 고정 
	- grup
- /etc/init 파일의 내용을 순차적으로 실행
	- init
- 부팅 시 필요한 마운트 정보를 가지고 있는 것
	- etc/fstab



cd /opt/homebrew/etc/nginx 

^ / $ 각각 행 맨 앞/ 맨 뒤
dd / 해당 행 삭제
yy / p 해당 행 복사 / 붙여넣기

찾기
ps -af | grep 찾을꺼

포트 찾기
sudo lsof -i : 포트번호
sudo kill -9 포트번호

cat acr.tf | pbcopy : 복사 붙여넣기

---
# 데비안 (Debian GNU/Linux)


- 데비안은 전 세계에서 가장 많이 서버로 이용되는 OS였으나 2016년 이후로 [우분투](https://namu.wiki/w/%EC%9A%B0%EB%B6%84%ED%88%AC "우분투")서버에게 그 자리를 내주었다.
- 데비안은 프로그램들을 deb란 패키지로 묶어서 관리

---
프로세스 (process)
- 실행 중인 프로그램. 각각 독립된 메모리 공간을 가진다.

스레드 (thread)
- 프로세스 내에서 실행되는 작은 실행 단위. 프로세스의 자원을 공유한다.

가상 주소 공간
- 각 프로세스가 실제 메모리 주소와 분리된 가상 주소를 사용하는 메모리 기술
---
컨테이너
- 독립된(=격리된) 프로세스 실행 환경을 제공하는 기술.
리눅스 name space와 cgroup을 사용하여 구현된다.

name space

cgroup
- 리소스 제어 그룹을 의미한다. 프로세스 그룹에 대한 자원 제한 및 모니터링 역할.

---
가상 머신

---
사용자 공간 (user mode)
- 일반 프로세스 코드가 실행되는 모드

커널 공간 (kernel mode)
- 시스템 리소스에 접근할 수 있는 모드

