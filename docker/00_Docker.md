# Docker

> **도커**(Docker)는 리눅스의 응용 프로그램들을 소프트웨어 컨테이너 안에 배치시키는 일을 자동화하는 오픈소스 프로젝트이다. 컨테이너형 가상화 기술 



![1](https://user-images.githubusercontent.com/42603919/71858537-f6608a00-312e-11ea-9067-6e22bfe4635f.PNG)

- **`이미지`** : 단일 파일로 존재, 부팅 가능한 OS 운영체제를 설치할 때 필요한 파일.
- **`오케스트레이션 도구`** : 여러가지 서비스를 `자동화`하기 위해서 사용 (많은 수의 서비스)
  - 도커 스웜, 쿠버네티스



### Docker file

```powershell
FROM ubuntu:16.01 # FROM base image

COPY helloworld /usr/local/bin # COPY 소스 타겟 -> 소스를 어디로 보낼지
RUN chmod +x /usr/local/bin/helloworld # 실행

CMD ["helloworld"] # Command (제일 마지막에 와야한다.)
```



### Docker image build

```powershell
$ docker image build -t helloworld:latest . # 이미지 빌드(빈 껍데기)
# . 은 현재 디렉토리에 있는 Docker file을 의미한다.
# : 다음에는 버전이 나온다.
sending build context to Docker daemon 97.5MB
```



```powershell
$ docker container run helloworld:latest # 실체화 시킴
# run = create + start
# docker stop 을 한 후 docker start를 해야한다. (docker run을 하게되면 create + start가 된다.)
Hello, World~
```



### 로컬 도커 환경 구축하기

[Docker 사이트](https://hub.docker.com/)

General (위에 3곳 표시) -> Shared Drives (C 선택, 적용,  비밀번호 입력) -> 나머지 부분은 나중에



- docker 사용할 때는 powershell 사용 (권장)



#### docker login

``` powershell
PS C:\Users\HPE> docker login
```



#### docker version

```powershell
PS C:\Users\HPE> docker version
```



- 도커 이미지 : 도커 컨테이너를 구성하는 파일 시스템과 실행할 애플리케이션 설정을 하나로 합친 것으로, 컨테이너를 생성하는 템플릿 역할을 한다.
- 토커 컨테이너 : 도커 이미지를 기반으로 생성되며, 파일 시스템과 애플리케이션이 구체화돼 실행되는 상태.



### 도커 이미지 관련 커맨드

``` powershell
PS C:\Users\HPE> docker image --help 를 입력하여 확인
```

![2](https://user-images.githubusercontent.com/42603919/71865023-ea7fc280-3144-11ea-8806-e2b2fb0bf1a5.PNG)

### 도커 이미지 다운로드

```powershell
PS C:\Users\HPE> docker image pull gihyodocker/echo:latest

Digest: sha256:4520b6a66d2659dea2f8be5245eafd5434c954485c6c1ac882c56927fe4cec84
Status: Downloaded newer image for gihyodocker/echo:latest
docker.io/gihyodocker/echo:latest
```

#### 

### 도커 이미지 확인

```powershell
PS C:\Users\HPE> docker image ls

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
gihyodocker/echo    latest              3dbbae6eb30d        2 years ago         733MB
```



### 도커 이미지 기동

```powershell
PS C:\Users\HPE> docker run gihyodocker/echo:latest
```



### 이미지를 갖고 실체화 시킨 컨테이너 목록 확인

```powershell
PS C:\Users\HPE> docker container ls 또는
PS C:\Users\HPE> docker ps
```

![3](https://user-images.githubusercontent.com/42603919/71865024-ea7fc280-3144-11ea-8d70-ada1adecee8a.PNG)





### 컨테이너 시작(포트번호 부여)

```powershell
PS C:\Users\HPE> docker run -p 8080 gihyodocker/echo:latest

PS C:\Users\HPE> docker run -p 9000:8080 gihyodocker/echo:lates # 포트 9000번으로 접속하면 8080 주소로 연결
```





```powershell
PS C:\Users\HPE> docker run -d -p 8080 gihyodocker/echo:latest # 데몬 (컨테이너 생성)
4dfea8b5f80e04a277e107b87e8d9413ca52ac978320c45804b8628f9373f8bd

PS C:\Users\HPE> docker ps # 실행중인 컨테이너 보여주기

PS C:\Users\HPE> docker ps -a # 종료된 컨테이너까지 보여주는 것

PS C:\Users\HPE\docker\day01\simpleweb> docker container prune # 모든 컨테이너 삭제

PS C:\Users\HPE> docker rm 컨테이너 id # 컨테이너 삭제

PS C:\Users\HPE> docker ps -q # 컨테이너 id를 가지고옴

PS C:\Users\HPE> docker stop [container ID] or [container name] # 컨테이너 종료

PS C:\Users\HPE> docker stop $(docker ps -q) # 가지고 있는 모든 컨테이너 id를 불러와 멈추기

PS C:\Users\HPE> docker rm $(docker ps -qa) # 가지고 있는 모든 컨테이너 id를 불러와 삭제하기
```





###  컨테이너 배포

---

#### 일반적인 절차

```
Users\HPE\docker\day01\simpleweb> npm install
```



![1](https://user-images.githubusercontent.com/42603919/71869717-6fbfa300-3156-11ea-8216-a6b42d6c7ba4.PNG)



![2](https://user-images.githubusercontent.com/42603919/71869718-6fbfa300-3156-11ea-95e6-7b86de25cbed.PNG)



**dependencies에 관련된 파일 다운로드**

```powershell
PS C:\Users\HPE\docker\day01\simpleweb> ls

d-----     2020-01-07   오후 1:59                node_modules -> 얘가 있어야 동작

PS C:\Users\HPE\docker\day01\simpleweb> npm start -> 서버를 기동
> @ start C:\Users\HPE\docker\day01\simpleweb
> node index.js

Listening on port 8080
```



**웹 브라우저에 띄우기**

![3](https://user-images.githubusercontent.com/42603919/71869719-70583980-3156-11ea-9e17-54658abd51a7.PNG)



**이것을 가지고 `docker`를 만들어 본다**

- 절차(일반적인)

  - Node.js 설치

  - 개발 : package.json, index.js
  - 의존성 해결 : npm install
  - 서버 기동 : npm start

- 절차(docker)
  - FROM -> Node를 사용 가능한 이미지
  - RUN -> npm install 실행
  - CMD -> npm start

---

#### Docker 절차

![1](https://user-images.githubusercontent.com/42603919/71870899-c4fdb380-315a-11ea-834e-c9fd8e67cf02.PNG)



![2](https://user-images.githubusercontent.com/42603919/71870929-e65e9f80-315a-11ea-9a71-7736176e479b.PNG)



**해결방법** (Step 2/3 : RUN npm install  ---> Running in b659ae4e79b0 /bin/sh: npm: not found)

1. 노드설치
2. Base Image를 교체

-> **2**을 이용해서 해결

![1](https://user-images.githubusercontent.com/42603919/71871388-5ae60e00-315c-11ea-8201-34abc83e7395.PNG)
![2](https://user-images.githubusercontent.com/42603919/71871389-5ae60e00-315c-11ea-9341-42d43f6b25a8.PNG)



**이미지 생성 완료 -> 이 이미지를 가지고 컨테이너 생성**

```powershell
PS C:\Users\HPE\docker\day01\simpleweb> docker run -d dlrjsapdlf622/simpleweb:latest
9549e06f84979e5d65df99da25b815e2303b82296f839c6191a4ac6e65977f93
```



**컨테이너를 생성 했지만 컨테이너가 죽어있음**(이미지 문제가 생김)

```powershell
PS C:\Users\HPE\docker\day01\simpleweb> docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

PS C:\Users\HPE\docker\day01\simpleweb> docker ps -a
CONTAINER ID        IMAGE                            COMMAND                  CREATED             STATUS                        PORTS               NAMES
9549e06f8497        dlrjsapdlf622/simpleweb:latest   "docker-entrypoint.s…"   28 seconds ago      Exited (254) 25 seconds ago                       loving_lichterman
-> 컨테이너가 죽어있다.
```

****



**그 원인을 찾아보기**

**./package.json**이 없기 때문에 문제가 발생

![1](https://user-images.githubusercontent.com/42603919/71872007-6df9dd80-315e-11ea-9100-df1e74e573cd.PNG)

![1](https://user-images.githubusercontent.com/42603919/71872032-80741700-315e-11ea-8708-53e52446d890.PNG)


![2](https://user-images.githubusercontent.com/42603919/71872008-6e927400-315e-11ea-9d2d-75d6b61f5938.PNG)



**/index.js**가 없기 때문에 문제가 발생

![1](https://user-images.githubusercontent.com/42603919/71872120-cfba4780-315e-11ea-917c-39c743c6f70e.PNG)

![1](https://user-images.githubusercontent.com/42603919/71872103-bc0ee100-315e-11ea-9013-cfb7620a5a5c.PNG)

![1](https://user-images.githubusercontent.com/42603919/71872990-5708ba80-3161-11ea-8021-b43644ad3923.PNG)



##### 웹 브라우저에 띄어지지 않음 -> 포트 연결 시켜야함

```powershell
PS C:\Users\HPE\docker\day01\simpleweb> docker container run -d -p 8080:8080 dlrjsapdlf622/simpleweb:latest

12a4df8e73b475db49fe74f381da110247a03c0fbf13285b9858b460be94cf95
```

![3](https://user-images.githubusercontent.com/42603919/71869719-70583980-3156-11ea-9e17-54658abd51a7.PNG)

![1](https://user-images.githubusercontent.com/42603919/71873716-35a8ce00-3163-11ea-973f-2fbce37b2d48.PNG)



##### 가지고 있는 이미지 파일을 계정에 등록하기

```powershell
PS C:\Users\HPE\docker\day01\simpleweb> docker push dlrjsapdlf622/simpleweb:latest
```

![1](https://user-images.githubusercontent.com/42603919/71873582-e5317080-3162-11ea-9268-2eda1805a54f.PNG)





##### 계정에 등록된 이미지 파일 사용하기

```powershell
PS C:\Users\HPE\docker\day01\simpleweb> docker pull dlrjsapdlf622/simpleweb:latest

PS C:\Users\HPE\docker\day01\simpleweb> docker image ls
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
dlrjsapdlf622/simpleweb   latest              9c1f375f2c68        12 minutes ago      114MB

PS C:\Users\HPE\docker\day01\simpleweb> docker run -d -p 8080:8080 dlrjsapdlf622/simpleweb:latest
```



![3](https://user-images.githubusercontent.com/42603919/71869719-70583980-3156-11ea-9e17-54658abd51a7.PNG)



##### 작동중인 컨테이너에 추가 옵션 사용가능

- i : 입력모드

- t : tty

```powershell
PS C:\Users\HPE\docker\day01\simpleweb> docker exec -it container id or name sh
```



---

---



![1](https://user-images.githubusercontent.com/42603919/71872103-bc0ee100-315e-11ea-9013-cfb7620a5a5c.PNG)

위에 있는 그림에서 package.json 파일과 index.js파일은 root directory에 위치해 있다.

이것을 /home/node directory로 옮겨본다.



RUN npm install에서 오류 발생(root에서 run하기 때문)

디렉토리를 바꿔서 실행하는 것이 좋다.

![1](https://user-images.githubusercontent.com/42603919/71875369-786ca500-3167-11ea-8626-1f0d6d1d061b.PNG)



![1](https://user-images.githubusercontent.com/42603919/71875630-1ceee700-3168-11ea-972d-cd98454f0365.PNG)



```
PS C:\Users\HPE\docker\day01\simpleweb> docker build -t dlrjsapdlf622/simpleweb:latest .
```

![2](https://user-images.githubusercontent.com/42603919/71875631-1d877d80-3168-11ea-935e-12ccaba9b71b.PNG)



![3](https://user-images.githubusercontent.com/42603919/71875633-1d877d80-3168-11ea-9a82-c725fd4be0e2.PNG)



```
PS C:\Users\HPE\docker\day01\simpleweb> docker run -d -p 8080:8080 dlrjsapdlf622/simpleweb:latest
```



```
PS C:\Users\HPE\docker\day01\simpleweb> docker exec -it container id or name sh

/home/node # vi index.js -> vi edit 수정
```

![4](https://user-images.githubusercontent.com/42603919/71875634-1d877d80-3168-11ea-8c22-3c80235ccd80.PNG)

```
PS C:\Users\HPE\docker\day01\simpleweb> docker restart container id or name
```



**하나의 컨테이너에서 수정을 해도 다른 컨테이너에는 적용되지 않는다.**

다른 컨테이너 생성

```
PS C:\Users\HPE\docker\day01\simpleweb> docker run -d -p 9090:8080 dlrjsapdlf622/simpleweb:latest
```



##### 비교

![5](https://user-images.githubusercontent.com/42603919/71875636-1d877d80-3168-11ea-9405-038c90412526.PNG)

![6](https://user-images.githubusercontent.com/42603919/71875637-1e201400-3168-11ea-9c2c-647af15fc981.PNG)



![캡처](https://user-images.githubusercontent.com/42603919/71877229-0054ae00-316c-11ea-8926-473e11d9a80f.PNG)

![12](https://user-images.githubusercontent.com/42603919/71877226-0054ae00-316c-11ea-96c5-b04a6c04a117.PNG)



### mysql 5.7 container 설치

```powershell
PS C:\Users\HPE\docker\day01\simpleweb> docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql
:5.7
```



#### mysql 접속하기

```powershell
PS C:\Users\HPE\docker\day01\simpleweb> docker exec -it mysql bash
root@dedfc35f6313:/# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.28 MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

