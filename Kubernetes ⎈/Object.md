---
created: 2024-01-08T17:27
updated: 2024-01-17T17:44
---
#kubernetes 
# Kubernetes 동작 시스템에서 영속성을 가지는 오브젝트

- 오브젝트 spec
- 오브젝트 status

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

- `apiVersion` - 이 오브젝트를 생성하기 위해 사용하고 있는 쿠버네티스 API 버전이 어떤 것인지
- `kind` - 어떤 종류의 오브젝트를 생성하고자 하는지
- `metadata` - `이름` 문자열, `UID`, 그리고 선택적인 #Namespace 를 포함하여 오브젝트를 유일하게 구분지어 줄 데이터
- `spec` - 오브젝트에 대해 어떤 상태를 의도하는지


### Ref
[쿠버네티스 오브젝트 이해하기](https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/kubernetes-objects/)
