---
created: 2024-01-30T17:42
updated: 2024-02-06T09:29
---
### 속도 차이? 🔑 결국 서버의 성능 차이
Github Actions vs Jenkins 실행 속도

이 두 도구의 성능은 서버의 성능과 환경에 따라 달라진다고 이해하는 게 더 맞는 것으로 보입니다.  
Github Actions의 runner가 self-hosted 환경인 경우(사용자가 직접 서버를 설치해서 사용하는 경우)도 그렇고 Jenkins도 서버의 사양, JVM 설정 등에 따라 그 실행 속도에 차이가 있기 때문입니다.  
(Github Actions의 runner가 github에서 제공하는 [github-hosted-runner](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners)인 경우에는 다를 수 있습니다.)

