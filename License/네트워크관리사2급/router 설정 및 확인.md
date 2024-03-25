- IP/subnet mask 설정 **ip add**
```
en
conf t
interface fastethernet 0/0
ip add 192.168.0.0 255.255.255.0
no shutdown
exit
exit
copy r s
```

- 대역폭/클럭 속도 설정 **bandwidth** / **clock rate**
```
# 대역폭
en
conf t
interface serial 2/0
bandwidth 2048
exit
exit
copy r s

# 클럭 속도
en
conf t
interface serial 2/0
clock rate 72000
exit
exit
copy r s
```

- 주석 설정 **description**
```
en
conf t
interface fastethernet 0/0
description ICQA
exit
exit
copy r s
```

- 두번째 IP 주소 설정 ip add + **secondary** / 활성화 **no shutdown**
```
en
conf t
interface serial 2/0
ip add 192.168.0.101 255.255.255.0
ip add 192.168.0.102 255.255.255.0 secondary
no shutdown
exit
exit
copy r s
```

- 기본 gateway 설정 **ip default-gateway** (exit 한 번!)
```
en
conf t
ip default-gateway 192.168.0.10
exit
copy r s
```

- DHCP 설정 **ip dhcp pool** + **network** 
```
en
conf t
ip dhcp pool icqa
network 192.168.100.0 255.255.255.0
exit
exit
copy r s
```

- telnet password 설정 **line vty** + **password**
-> telnet 문제는 *로그인* 설정 유무 확인
```
en
conf t
line vty
password icqa
login
exit
exit
copy r s
```

- telnet 연결 후 3분 50초 동안 입력이 없을 때 세션 자동 종료 **line vty** + **exec-timeout**
```
en
conf t 
line vty
exec-timeout 03 50
exit
exit
copy r s
```

- console 0의 password를 ICQA로 설정하고 로그인 **line console** + **password** + **login**
```
en
conf t
line console 0
password ICQA
login
exit
exit
copy r s
```

- serial 2/0을 활성화 **no shutdown**
```
en
conf t
interface serial 2/0
no shutdown
exit
exit
copy r s
```

- hostname을 network2로 변경, console 0의 password를 route5로 변경한 뒤 로그인
```
en
conf t
hostname network2
line console 0
password route5
login
exit
exit
copy r s
```

---
정보를 확인하고 저장하라 = **show**

- interface 정보를 확인하고 저장 interface
```
en
show interface
copy r s
```

- 접속한 사용자 정보를 확인하고 저장 user
```
en
show user 
copy r s
```

- **라우팅 테이블 정보**를 확인하고 저장 **ip route**
```
en
show ip route
copy r s
```

- 플래쉬 내용을 확인하고 저장 flash
```
en
show flash
copy r s
```

- 프로세스 내용을 확인하고 저장 process
```
en
show process
copy r s
```

---
