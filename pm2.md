# Node.js로 만들어진 프로그램을 관리해주는 매니저
**P**(rocess) **M**(anager) **2**

- [공식문서](https://pm2.keymetrics.io/)

### 기능

- 프로그램이 꺼지면 자동으로 다시 켤 수 있다.
```shell
pm2 start app.js
```

- 코드에 변경상황이 생기면 자동으로 반영된다. 
```shell
pm2 start app.js --watch
```

- 로그 확인
```shell
pm2 log
```