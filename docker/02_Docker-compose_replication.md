# Docker

### `Docker compose`과정을 통해서 `replication` 작업 수행하기



- ip 확인
  - ifconfig
  - ip addr show
  - hostname -i
  - docker inspect 컨테이너 id



### mongodb 서버 3개 띄우기

- 이미지 빌드

```powershell
PS C:\Users\HPE\docker\day02\mongo> docker build -t dlrjsapdlf622/mymongodb:latest .
```



- 서버 3개 띄우기 (컨테이너 3개 만들기)

```powershell
PS C:\Users\HPE\docker\day02\mongo> docker run -p 27017:27017 dlrjsapdlf622/mymongodb:latest
```

```powershell
PS C:\Users\HPE\docker\day02\mongo> docker run -p 27018:27017 dlrjsapdlf622/mymongodb:latest
```

```powershell
PS C:\Users\HPE\docker\day02\mongo> docker run -p 27019:27017 dlrjsapdlf622/mymongodb:latest
```

![캡처](https://user-images.githubusercontent.com/42603919/72028000-66544900-32c4-11ea-90b0-8b7abd72638e.PNG)



- 각각의 서버의 ip 주소 확인

```
27017 -> "172.17.0.2", 27018 -> "172.17.0.3", 27019 -> "172.17.0.4"
```



- 자기네들끼리 접속할 때는 어떻게 하는지 (각각의 컨테이너, mongodb 서버들끼리)

---

**ping 이 안 될 경우 (모든 컨테이너, 모든 서버에 설치)**

```
docker exec -it 컨테이너 id bash <- 접속 후

apt-get update
apt-get install -y iputils-ping
```

---

---

- **replicaSet** (replicaSeting)

```pseudocode
rs.initiate();
rs.add("172.17.0.2:27017"); //27017
rs.add("172.17.0.3:27017"); //27018
rs.add("172.17.0.4:27017"); //27019

//-------------------------------------

config = {
    _id: "replication",
    member: [
        {_id:0, host: "172.17.0.2:27017"},
        {_id:1, host: "172.17.0.3:27017"},
        {_id:2, host: "172.17.0.4:27017"},

    ]
}

rs.initiate(config);
rs.conf();

// 두가지의 방법이 있는데 아래의 방법을 추천
```



> docker run -p 27017:27017 dlrjsapdlf622/mymongodb:latest
> docker run -p 27018:27017 dlrjsapdlf622/mymongodb:latest
> docker run -p 27019:27017 dlrjsapdlf622/mymongodb:latest
> 이 3개의 명령어를 dockerfile을 이용하여 한번에 만들 수 있도록 해본다.

![1](https://user-images.githubusercontent.com/42603919/72030658-16c64b00-32cd-11ea-8f25-ce67c7031247.PNG)

![2](https://user-images.githubusercontent.com/42603919/72031278-52faab00-32cf-11ea-9da4-215c16017223.PNG)

![3](https://user-images.githubusercontent.com/42603919/72031279-52faab00-32cf-11ea-9705-e1f4fd3e18fc.PNG)



**새로운 상태에서 실행(컨테이너, 이미지 새로생성)**

```
PS C:\Users\HPE\docker\day02\mongo> docker build -t dlrjsapdlf622/mymongo:latest .
PS C:\Users\HPE\docker\day02\mongo> docker run -p 27017:27017 dlrjsapdlf622/mymongo:latest
-> 오류발생
```



**접속 문제가 발생(이유)**

1.서버가 기동x, 2.ip를 고정적으로 정할 경우 접속 문제가 발생한다.

 -> 따라서 docker-compose를 이용하여 해결해 본다.



### docker-compose

![1](https://user-images.githubusercontent.com/42603919/72030658-16c64b00-32cd-11ea-8f25-ce67c7031247.PNG)

![1](https://user-images.githubusercontent.com/42603919/72031991-76bef080-32d1-11ea-81b4-a4dc13b1a6e6.PNG)

![2](https://user-images.githubusercontent.com/42603919/72031993-76bef080-32d1-11ea-99c0-3a93e0bf3268.PNG)

![3](https://user-images.githubusercontent.com/42603919/72031994-77578700-32d1-11ea-9e11-6c20791d05c8.PNG)



**새로운 상태에서 실행(컨테이너, 이미지 새로생성)**

```
PS C:\Users\HPE\docker\day02\mongo> docker build -t mongo_repl_setup:latest .
```

```
PS C:\Users\HPE\docker\day02\mongo> docker-compose up
```

![1](https://user-images.githubusercontent.com/42603919/72032107-cf8e8900-32d1-11ea-98ff-73da25f30949.PNG)



위의 방법으로 접속x -> 다음과 같이 수정하여 접속

![1](https://user-images.githubusercontent.com/42603919/72044504-3f633a80-32f7-11ea-808b-c1a5bba3ad14.PNG)

![2](https://user-images.githubusercontent.com/42603919/72044505-3ffbd100-32f7-11ea-994a-116783d85eaa.PNG)

![3](https://user-images.githubusercontent.com/42603919/72044506-3ffbd100-32f7-11ea-9efe-aa76d958cdb0.PNG)

![4](https://user-images.githubusercontent.com/42603919/72044507-3ffbd100-32f7-11ea-8e13-de017aad21fd.PNG)



**새로운 상태에서 실행(컨테이너, 이미지 새로생성)**

```powershell
PS C:\Users\HPE\docker\day02\mongo> docker build -t mongo_repl_setup:latest .
```

```powershell
PS C:\Users\HPE\docker\day02\mongo> docker-compose up
```



다른 powershell에서 mongo1에 접속

```powershell
PS C:\Users\HPE\docker\day02\mongo> docker exec -it mongo1 id mongo
```

![1](https://user-images.githubusercontent.com/42603919/72044654-a97bdf80-32f7-11ea-97b8-fbcedfc51397.PNG)

-