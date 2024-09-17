
1. Rclone 사용하는 방법

설치
```
sudo -v ; curl https://rclone.org/install.sh | sudo bash
(password)

export SYSTEMD_EDITOR=vim

rclone config file
vi "해당 path"

[aws_remote]
type : s3
provider: AWS
env_auth: true
access_key_id: AKIA2THMGYSXGMCMTLOI
secret_access_key: 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
region: us-east-1
endpoint: https://bucket.vpce-00258b6b628ecb9ba-ka9d1lxh.s3.us-east-1.vpce.amazonaws.com

[az_remote]
type: azureblob
account: stsdpdraccount
key: "account's access key"
tenant: a141d6e8-fddb-4309-8b71-44753a78495a
client_id: "role > service pricipal > application id"

# client_id
az ad sp create-for-rbac --name AWS-Rclone-Reader --role "Storage Blob Data Reader" \
--scopes /subscriptions/d7985918-5fbf-4c79-ba0a-6ed55be24163/resourceGroups/rg-sdp-dr/providers/Microsoft.Storage/storageAccounts/stssdpdraccount


## 해당 파일 위치 확인 후 batch script & scheduler 작성
vi rclone_sync.sh

crontab -e

0 */3 * * * rclone_sync.sh

crontab -l 확인

# 실행 권한 부여
chmod +x rclone_sync.sh 

# 실행 테스트
./rclone_sync.sh

# crontab start
sudo service cron start

```


---