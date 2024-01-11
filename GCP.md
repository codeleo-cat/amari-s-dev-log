# 구글에서 개발한 클라우드 컴퓨팅 플랫폼. 
- #kubernetes 를 탄생시킨 곳이기에 쿠버네티스를 운영하기에 적합한 환경이다.


GCP 시작
```shell
$ brew install --cask google-cloud-sdk
```
GCP 로그인
```shell
$ gcloud auth login `ACCOUNT`
```
Project 생성 및 region , zone 생성
```bash

$ gcloud projects create example-foo-bar-1 --name="Happy project" --labels=type=happy

$ export CLOUDSDK_CORE_PROJECT=$PROJECT_ID

$ gcloud config set compute/region $REGION

$ gcloud config set compute/zone $ZONE

$ gcloud config get-value compute/region

$ gcloud config get-value compute/zone

$ gcloud compute project-info add-metadata \
   --metadata google-compute-default-region=$REGION,google-compute-default-zone=$ZONE
   
$ gcloud init

```

- [[GKE]]

### Ref
[GCP 공식문서](https://cloud.google.com/?hl=en)
[project 선취권 문제로 삭제 안될 때](https://support.google.com/a/thread/74530265/can-t-remove-google-cloud-project-due-to-dialogflow?hl=en)