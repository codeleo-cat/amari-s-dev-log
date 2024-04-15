---
created: 2024-01-09T12:18
updated: 2024-01-11T09:35
---
# [NoSQL](https://namu.wiki/w/NoSQL "NoSQL") [DBMS](https://namu.wiki/w/DBMS "DBMS")의 한 종류.

### 구성

- Document : 정렬된 key, value의 집합. 가장 기본적인 data
- Collection : document의 모음. RDBMS로 치면 Table. 동적 schema를 가진다.
- Database
- _id_ : document의 Primary key

### 특징

- Document Database (JSON처럼 키-밸류(Key-Value) 조합 그대로 저장할 수 있는 **문서** 형식의 **DB**)
- value의 형태는 동적으로 변경 가능하다. *Schemaless*
- NoSQL이면서 Index를 지원한다.
- 특수 쿼리 
- 가용성
- #Scale-out [[Sharding]]
### 문법

- database 생성 or 선택 : use db명
- database 확인 : show dbs
- collection 생성 : db.createCollection('컬렉션명')
- collection 확인 : show collections
- collection 삭제 : db.컬렉션명.drop()
- database 삭제 : db.dropDatabase()
- database 로그인 : db.auth('ID','Password)

#### *Deprecated* 
- ~~db.collection.insert()~~

### MacOS에 MongoDB 설치 (feat.brew)
```bash
$ brew tap mongodb/brew
$ brew install mongodb-community@6.0

$ brew services start mongodb-community@6.0
```
- 접속 확인
http://localhost:27017 으로 접속하면 아래의 문구 출력됨 &rarr; 정상 동작한다는 의미이다.
```
It looks like you are trying to access MongoDB over HTTP on the native driver port.
```
- mongo 6.0 이상 ver &darr;
```bash
$ mongosh
```
- mongo 6.0 이전 ver &darr;
```bash
$ brew install mongodb-community-shell
$ mongo
```

