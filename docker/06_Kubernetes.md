# Kubernetes

![캡처](https://user-images.githubusercontent.com/42603919/72319401-6c8e5f00-36e2-11ea-82ac-aa79608f46d5.PNG)

> Docker container 운영을 자동화하기 위한 컨테이너 오케스트레이션 툴. 많은 수의 컨테이너를 협조적으로 연동시키기 위한 통합 시스템이며, 이 컨테이너를 다루기 위한 API 및 명령행 도구 등이 함께 제공된다.

- 컨테이너 배포 및 배치 전략
- Scale in/ Scale out
- Service discovery
- 기타 운용



**시작하기**

![캡처](https://user-images.githubusercontent.com/42603919/72319898-c80d1c80-36e3-11ea-99f8-d471a34f5f5f.PNG)

![캡처](https://user-images.githubusercontent.com/42603919/72320119-5c777f00-36e4-11ea-81ba-d447e272b13c.PNG)



**kubectl 설치**

```
https://storage.googleapis.com/kubernetes-release/release/v1.17.0/bin/windows/amd64/kubectl.exe
```



**버전확인**

```powershell
PS C: ~~~ >kubectl version

Client Version: version.Info{Major:"1", Minor:"14", GitVersion:"v1.14.8", GitCommit:"211047e9a1922595eaa3a1127ed365e9299a6c23", GitTreeState:"clean", BuildDate:"2019-10-15T12:11:03Z", GoVersion:"go1.12.10", Compiler:"gc", Platform:"windows/amd64"}
Server Version: version.Info{Major:"1", Minor:"14", GitVersion:"v1.14.8", GitCommit:"211047e9a1922595eaa3a1127ed365e9299a6c23", GitTreeState:"clean", BuildDate:"2019-10-15T12:02:12Z", GoVersion:"go1.12.10", Compiler:"gc", Platform:"linux/amd64"}
```



```powershell
PS C: ~~~ > kubectl get all
```



**Dashboard 설치**

> Kubernetes에 배포된 컨테이너 등에 대한 정보를 보여주는 관리도구

```powershell
PS C: ~~~ > kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.8.3
/src/deploy/recommended/kubernetes-dashboard.yaml
```

```powershell
PS C: ~~~ > kubectl get pod --namespace=kube-system -l k8s-app=kubernetes-dashboard
```

```powershell
PS C: ~~~ > kubectl proxy
```

**웹 브라우저에서**

```
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login
```

![캡처](https://user-images.githubusercontent.com/42603919/72321772-16241f00-36e8-11ea-9dd9-945671c14f21.PNG)



**ip 주소로 접속하기**

```powershell
PS C: ~~~ > kubectl proxy --port=8001 --address=접속할ip주소 --accept-hosts='^*$'
```

![캡처](https://user-images.githubusercontent.com/42603919/72323759-c85de580-36ec-11ea-8903-de8edbcdb173.PNG)



#### Dockerfile

```dockerfile
FROM node:slim
EXPOSE 8000
COPY hello.js .
CMD node hello.js
```



**이미지 빌드**

```powershell
PS C:\Users\HPE\docker\day04> docker build -t dlrjsapdlf622/hello:latest .
```



**이미지 push**

```
PS C:\Users\HPE\docker\day04> docker push dlrjsapdlf622/hello:latest
```

![캡처](https://user-images.githubusercontent.com/42603919/72326887-681e7200-36f3-11ea-9bf1-5310ad7e268f.PNG)

```
PS C:\Users\HPE\docker\day04> docker run -p 8100:8000 dlrjsapdlf622/hello:latest
```

![캡처](https://user-images.githubusercontent.com/42603919/72325751-24c30400-36f1-11ea-8b31-d16752e5795a.PNG)



**pods 생성**

```yaml
# pod.yml

apiVersion: v1
kind: Pod
metadata:
  name: hello-pod
  labels:
    app: hello
spec:
  containers:
  - name: hello-container
    image: kubetm/hello
    ports:
    - containerPort: 8000
```



**kubernetes에 등록하기**

1. 코드

   ```powershell
   PS C:\Users\HPE\docker\day04> kubectl apply -f *.yml
   ```

2. 대시보드

   > 오른쪽 상단 create 클릭 -> yml파일 입력

![캡처](https://user-images.githubusercontent.com/42603919/72326958-87b59a80-36f3-11ea-9fa4-ae9db573eb32.PNG)

**-> hello-pod 클릭 -> 우측 상단 exec 클릭 -> 파일목록 확인(ls)**

![캡처](https://user-images.githubusercontent.com/42603919/72327185-f2ff6c80-36f3-11ea-9894-bd317434d80a.PNG)





**서비스 등록하기**

```yaml
# service.yml

apiVersion: v1
kind: Service
metadata:
  name: hello-svc
spec:
  selector:
    app: hello
  ports:
    - port: 8200
      targetPort: 8000
  type: LoadBalancer
  externalIPs:
  - xxx.xx.xx.xx #(본인 ip주소)
```

**-> 대시보드(create)에서 저것을 복사하여 실행**



**Service 생성 완료**

![캡처](https://user-images.githubusercontent.com/42603919/72327654-ddd70d80-36f4-11ea-9f07-bffc1a366382.PNG)

**powershell에서도 확인 가능**

![캡처](https://user-images.githubusercontent.com/42603919/72328188-d2d0ad00-36f5-11ea-8fca-fba75a5ae232.PNG)

