# Docker Swarm

### Docker service

> 애플리케이션을 구성하는 일부 컨테이너(단일 또는 복수)를 제어하기 위한 단위

`swarm 상태에서 docker service가 실행된다. 따라서 manager에 접속하여 service를 실행한다.`



**서비스 생성**

> registry에 이미지가 저장이 되어있어야 다음과정 수행 가능. 만약 registry에 이미지가 없다면 registry에 이미지를 등록하고 다음 과정을 수행.

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh

/ # docker service create --replicas 1 --publish 8000:8080 --name echo registry:5000/example/echo:latest

-> registry에 있는 example/echo:latest를 불러와서 사용함.
```

![캡처](https://user-images.githubusercontent.com/42603919/72118297-06c56e80-3394-11ea-9434-72633a60fd27.PNG)

![캡처](https://user-images.githubusercontent.com/42603919/72117547-7d14a180-3391-11ea-92b4-89bf85e9453e.PNG)

---

**서비스 지우기**

```
docker service rm 서비스 id
```

---

**서비스 확인**

```
/ # docker service ls

ID                  NAME                MODE                REPLICAS            IMAGE                               PORTS
osanqjsq1t98        echo                replicated          1/1                 registry:5000/example/echo:late
```



-> 웹브라우저에 localhost:8000에 접속해보면 접속이 되지 않는다. 그 이유는 윈도우(host)에서 8000을 호출하게 되면 8000과 연결되어있는 컨테이너는 없기 때문에 접속이 되지 않는다.(host는 registry와 연결되어 있음)

```
d75f977384db        docker:19.03.5-dind   "dockerd-entrypoint.…"   51 minutes ago      Up 50 minutes       2375-2376/tcp, 3375/tcp, 0.0.0.0:9000->9000/tcp, 0.0.0.0:8000->80/tcp   manager
```

**windows에서 8000이 요청되면 manager는 80으로 응답 -> manager에서 80이 요청되면 echo는 8080으로 응답**

이를 적용하여 고쳐보면,

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh
/ # docker service create --replicas 1 --publish 80:8080 --name echo registry:5000/example/echo:latest
```

![캡처](https://user-images.githubusercontent.com/42603919/72118248-d978c080-3393-11ea-96a1-b4206c63244b.PNG)

![2](https://user-images.githubusercontent.com/42603919/72118249-d978c080-3393-11ea-9f6d-a8af1c871060.PNG)

 

- 서비스에 프로세스 중에서 echo를 확인

```
/ # docker service ps echo
```

![캡처](https://user-images.githubusercontent.com/42603919/72118733-75ef9280-3395-11ea-8927-d7cae5b92cb2.PNG)

![캡처](https://user-images.githubusercontent.com/42603919/72118959-3aa19380-3396-11ea-951f-4ce2597ca838.PNG)

**manager의 컨테이너 id와  NODE의 ID가 동일하다는 것을 알 수 있다.**



**scale up 해보기**

```
/ # docker service scale echo=3
```

![캡처](https://user-images.githubusercontent.com/42603919/72119421-bb14c400-3397-11ea-9418-9dd0a0792fcc.PNG)



```
/ # docker service ps echo
```

![캡처](https://user-images.githubusercontent.com/42603919/72119462-db448300-3397-11ea-80a6-909f3d3ed509.PNG)

- manager : d75
- woker01 : 026
- woker02 : e53
- woker03 : f34
- registry : 43e

echo는 manager, woker01, worker03에 배포됨