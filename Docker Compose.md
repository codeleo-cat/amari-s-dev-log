https://www.daleseo.com/docker-compose/

`yaml`를 설정 파일로 사용
```bash
 docker-compose -f 
```
모든 서비스 컨테이너를 한 번에 생성하고 실행
`-d` 옵션을 사용하여 백그라운드에서 컨테이너를 띄우는 경우
```bash
$ docker-compose up -d
```
(`-d` 옵션을 사용하지 않으면 현재 터미널에 컨테이너의 로그가 출력되고 
`Ctrl + C`를 눌러서 탈출하는 순간 컨테이너가 모두 정지되기 때문이다.)

모든 서비스 컨테이너를 한 번에 정지시키고 삭제
```bash
$ docker-compose down
```
내려가 있는 있는 특정 서비스 컨테이너를 올리기 위해서 사용한다.
```bash
$ docker-compose start
```
돌아기고 있는 특정 서비스 컨테이너를 정지시키기 위해서 사용
```bash
$ docker-compose stop
```
서비스 컨테이너의 특정 명령어를 일회성으로 실행
```bash
$ docker-compose run
```
