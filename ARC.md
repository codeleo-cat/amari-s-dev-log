#Github_Actions #kubernetes
# CI using Github Actions & ARC
### ARC (**A**ctions **R**unner **C**ontroller)
- vm처럼 단일 instance에 Github Actions runner를 설치할 수도 있지만, #ARC 를 사용해 K8s 위에 설치할 수 있다. (= 쿠버네티스 파드로 러너 실행 가능.)
	- 장점 : 비용 효율성 & 확장성 & 속도

1. App Repository > workflow 생성 
	```
	git clone `repository url`
	cd 해당 repository 경로
	mkdir -p .github/workflows
    cd .github/workflows
	vi github-actions-demo.yml
	```
- github-actions-demo.yml &darr;
```yaml
name: GitHub Actions Demo
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v4
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
```
- https://docs.github.com/en/actions/quickstart#creating-your-first-workflow

2. [personal access token 발급](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic)
	- Repository access 는 특정 repo만 설정
	- Permissions는 Actions, Administration, Contents, Secrets, Webhooks, Pull requests 설정
# K8s cluster
```
NAMESPACE="arc-systems"
echo $NAMESPACE

helm install arc \
    --namespace "${NAMESPACE}" \
    --create-namespace \
    oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set-controller
    
INSTALLATION_NAME="arc-runner-set"
NAMESPACE="arc-runners"
GITHUB_CONFIG_URL="https://github.com/<your_enterprise/org/repo>"
GITHUB_PAT="<PAT>"
helm install "${INSTALLATION_NAME}" \
    --namespace "${NAMESPACE}" \
    --create-namespace \
    --set githubConfigUrl="${GITHUB_CONFIG_URL}" \
    --set githubConfigSecret.github_token="${GITHUB_PAT}" \
    oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set

```
```yaml
kubectl patch service kubernetes -n default  -p "{\"metadata\":{\"finalizers\":null}}"
```
### Ref
---
- [Github Actions by ARC](https://tech.buzzvil.com/blog/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4%EC%97%90%EA%B2%8C-github-actions-%EC%84%A4%EC%B9%98%EC%97%90-%EB%8C%80%ED%95%B4-%EB%AC%BB%EB%8B%A4/)
- https://velog.io/@alli-eunbi/Git-EKS-ARC-세팅-방법
- https://ykarma1996.tistory.com/195
- https://trunk.io/blog/how-to-self-host-github-actions-runners