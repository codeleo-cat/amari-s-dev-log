1. 서버 2대 필요 (서로 다른 Ip)
2. mysql version 이 같아야 함.
SHOW VARIABLES LIKE '%VERSION%';

- 기본 mysql 설치 후 설정 확인 (ubuntu)
sudo apt update
sudo apt install mysql-server
mysql --version

sudo systemctl start mysql
sudo systemctl status mysql > active 상태 확인

sudo  vi /etc/my.cnf
azure > server-id =1 
aws > server-id =2
sudo systemctl restart mysql

---
root 권한으로 바꿔서 진행

sudo passwd root
(password)
su

---
- Azure (172.16.30.4)
mysql -h mysql-partner-db.mysql.database.azure.com -u sqlAdmin -p 
@Partnerpwd!

---
ec2
ssh -i "key/dmskey.pem" ec2-user@ec2-3-223-129-214.compute-1.amazonaws.com

---
- AWS (10.0.4.71)
mysql -h rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com -u sqlAdmin -p
kdjPIOIDJvjeifj09892jfjKjdjE30

vpce-0ca94de5e9883d9c9-5su0iib6.dms.us-east-1.vpce.amazonaws.com

---
동일한 user를 두 database server에 각각 생성한 다음

// user 생성
CREATE USER 'replicationuser'@'%' IDENTIFIED BY 'replication';

// mysqldump 를 사용하기 위한 권한
GRANT REPLICATION SLAVE, PROCESS, SELECT, SHOW VIEW, TRIGGER ON *.* TO 'replicationuser'@'%';

// master status 확인을 위한 권한 - 필수 아님.
GRANT REPLICATION CLIENT ON *.* TO 'replicationuser'@'%';


// 부여된 권한 확인
SHOW GRANTS FOR sqlAdmin@'%';

SHOW GRANTS FOR sqlAdmin@'%';

FLUSH PRIVILEGES; - grant 에서는 쓸 필요가 없다.

---
아래 command를 통해 접근이 가능한지 확인.

az : mysql -u replicationuser -p -h 172.16.30.4 -P 3306
(replication)
aws: mysql -u replicationuser -p -h 10.0.4.143 -P 3306 
(replication)

---

SHOW MASTER STATUS\G;
SHOW VARIABLES LIKE '%VERSION%';

az (A)
~~~
 File: mysql-bin.000001
         Position: 1400535
     Binlog_Do_DB: 
~~~

aws (B)
CALL mysql.rds_stop_replication;
CALL mysql.rds_set_external_master( '172.16.30.4', 3306, 'replicationuser', 'replication', 'mysql-bin.000001', 1403849, 0);
CALL mysql.rds_start_replication;
SHOW slave status\G;

// aws dump
mysqldump --single-transaction --set-gtid-purged=OFF --no-tablespaces -h rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com -u replicationuser -p api > api-$(date +%Y%m%d).sql
replication

// az 도 마찬가지로 dump를 뜸.

// mysqldump 를 사용하기 위한 권한
- 덤프된 테이블에 대한 SELECT 권한
- 덤프된 뷰에 대한 SHOW VIEW
- 덤프된 트리거를 위한 TRIGGER
- `--single-transaction`옵션을 사용하지 않는 경우 LOCK TABLES
- `--no-tablespaces`옵션을 사용하지 않는 경우 PROCESS
https://anothercoffee.net/how-to-fix-the-mysqldump-access-denied-process-privilege-error/
+
// master status 확인을 위한 권한 
GRANT REPLICATION CLIENT ON *.* TO 'replicationuser'@'%';
SHOW GRANTS FOR replicationuser@'%';
FLUSH PRIVILEGES;

----
### AZ!!
mysql -h mysql-partner-db.mysql.database.azure.com -u sqlAdmin -p 
@Partnerpwd!

CALL mysql.az_replication_change_master('10.0.4.143', 'replicationuser', 'replication', 3306, 'mysql-bin-changelog.062342', 321, '');

// 복제 시작
CALL mysql.az_replication_start;
// 복제 상태 확인
show slave status\G;
// 확인
Seconds_Behind_Master가 0
Slave_IO_Running: Yes
Slave_SQL_Running: Yes

|mysql> CALL mysql.az_replication_stop;
|mysql> CALL mysql.az_replication_remove_master;

---
### AWS

mysql -h rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com -u sqlAdmin -p -A
kdjPIOIDJvjeifj09892jfjKjdjE30

call mysql.rds_set_external_master( '172.16.30.4', 3306, 'replicationuser', 'replication', 'mysql-bin.000001', 1406159, 0);

mysql> CALL mysql.rds_start_replication; 
mysql> CALL mysql.rds_stop_replication;

**[출처]** [[AWS] RDS 명령어들](https://blog.naver.com/sory1008/220952534094)|**작성자** [asdf](https://blog.naver.com/sory1008)

// aws dump
mysqldump --single-transaction --set-gtid-purged=OFF -h rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com -u sqlAdmin -p --all-databases > aws_$(date +%Y%m%d).sql
kdjPIOIDJvjeifj09892jfjKjdjE30

// rds에 넣기
mysqldump -h rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com -u sqlAdmin -p api < az_my.sql

mysqldump -h rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com -u sqlAdmin -p abc < az_my.sql

mysqldump -h rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com -u sqlAdmin -p test < az_my.sql

mysqldump -h rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com -u sqlAdmin -p sync_test < az_my.sql
kdjPIOIDJvjeifj09892jfjKjdjE30

create database api;
create database abc;
create database test;
create database sync_test;


mysqldump -h rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com -u sqlAdmin -p abc < az_my.sql

// az에 넣기
mysqldump -h mysql-partner-db.mysql.database.azure.com -u sqlAdmin -p api abc sync_test test < aws_20231113.sql
@Partnerpwd!

// az dump
mysqldump -h mysql-partner-db.mysql.database.azure.com -u sqlAdmin -p --databases api abc sync_test test > az_my.sql
@Partnerpwd!

---
start slave;

CALL mysql.az_replication_start;
CALL mysql.az_replication_stop;
CALL mysql.az_replication_remove_master;

---
1. azure flexible db version 지정 불가능함?
	1. aws는 지정 가능함.
https://ballboystudy.tistory.com/29

create table student_2 (
  ID VARCHAR(4),
  NM VARCHAR(30) NOT NULL,
  GRADE INT NOT NULL,
  GENDER ENUM('M','F'),
  BIRTH DATE DEFAULT '19000101',
  ADDR VARCHAR(100),
  PRIMARY KEY (ID)
);

INSERT INTO student_2(ID, NM, GRADE, GENDER)
VALUES
('Ee3', 'Francis', '34', 'M');

('B81', 'Zia', '9', 'F'),
('C36', 'Maggie', '2', 'F'),
('D56', 'Minnie', '4', 'M'),
('E86', 'Francis', '5', 'M')

select * from stud_3;

INSERT INTO stud_3(id)
VALUES
('Ee32222');

create table aws_db (
  ID VARCHAR(4),
  NM VARCHAR(30) NOT NULL,
  GRADE INT NOT NULL,
  GENDER ENUM('M','F'),
  BIRTH DATE DEFAULT '19000101',
  ADDR VARCHAR(100),
  PRIMARY KEY (ID)
);

INSERT INTO aws_db(ID, NM, GRADE, GENDER)
VALUES
('Ee3', 'Francis', '34', 'M');

select * from aws_db;

----
- Azure
mysql-partner-db.mysql.database.azure.com

---
- AWS (10.0.4.143)
rds-partner-db-multi-cloud.ceibyqnkxakt.us-east-1.rds.amazonaws.com 


https://docs.aws.amazon.com/ko_kr/vpn/latest/s2svpn/HowToTestEndToEnd_Linux.html
