FLUSH PRIVILEGES 명령어
새로운 사용자 계정을 생성하거나 기존 사용자의 권한을 변경한 후, FLUSH PRIVILEGES를 실행하면 MySQL 서버가 메모리에 캐시된 권한 정보를 업데이트하여 변경 사항이 즉시 반영됩니다.

Binlog
MySQL server에서 DB에 변경사항이 생기면 (정확히는 트랜잭션이 commit되는 순간)그 변화된 event를 기록하는 이진 파일.

Replication
Replica DB가 Source의 Binlog를 읽어가서 relay log로 저장한다.

docker-compose.yml 
파일명을 맞춰주세요.


SELECT user FROM mysql.user;

CHANGE MASTER TO MASTER_HOST='mysql_master', MASTER_USER='replica_user', MASTER_PASSWORD='replica_password', MASTER_PORT=3306, MASTER_LOG_FILE='mysql-bin.000003', MASTER_LOG_POS=843;

START SLAVE;

1.test - rollback (azure db write 전으로 돌아가자.)
2.dr - write 일어난 azure db 그대로 rds에도 적용. 그리고 다시 replica set up (M : RDS)

네, Slave DB를 Master DB의 특정 시점으로 돌리면서 동시에 데이터베이스의 테이블에도 해당 시점의 상태로 자동으로 반영되게 할 수 있습니다. 이를 위해 MySQL에서는 **Point-in-Time Recovery (PITR)** 기능을 사용할 수 있습니다. 이 기능을 사용하면 특정 시점까지의 변경 사항만을 적용하여 데이터베이스를 복구할 수 있습니다

mysqlbinlog --stop-datetime="2024-08-09 17:20:00" --host=master_host --user=replica_user --password=replica_password mysql-bin.000003 | mysql -u root -p
(rootpassword)

docker exec -it mysql_slave bash