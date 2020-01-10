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

---

### Docker Stack

`swarm 상태에서 docker stack이 실행된다. 따라서 manager에 접속하여 stack을 실행한다.`

#### overlay 네트워크 생성

**생성**

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh
/ # docker network create --driver=overlay --attachable ch03
```

**확인**

```
/ # docker network ls

o9svf6si6142        ch03                overlay             swarm
```




### **docker-compose(스택 생성)**

> 볼륨 마운트를 걸어놨기 때문에 window와 linux (powershell)에서 동시에 보인다.

![캡처](https://user-images.githubusercontent.com/42603919/72121309-029e4e80-339e-11ea-868b-060fb1ac271d.PNG)

![캡처](https://user-images.githubusercontent.com/42603919/72121436-590b8d00-339e-11ea-8fd0-fa2204c23d27.PNG)



```yaml
# my-webapi.yml

version: "3"
services: 
    api: 
        image: registry:5000/example/echo:latest
        deploy:
            replicas: 3
            placement:
                constraints: [node.role != manager]
        networks: 
            - ch03
        
    nginx:
        image: gihyodocker/nginx-proxy:latest
        deploy:
            replicas: 3
            placement:
                constraints: [node.role != manager]
        environment: 
            BACKEND_HOST: echo_api:8080
        networks: 
            - ch03
        depends_on:
            - api
        
networks:
    ch03:
        external: true
```



**스택 배포하기**

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh

/ # cd /stack

/stack # docker stack deploy -c /stack/my-webapi.yml echo
Creating service echo_api
Creating service echo_nginx
```



**스택 확인**

```
/stack # docker stack ls

NAME                SERVICES            ORCHESTRATOR
echo                2                   Swarm
```



**서비스 확인**

```
/stack # docker service ls

ID                  NAME                MODE                REPLICAS            IMAGE                               PORTS
r2wr9us8re9q        echo_api            replicated          3/3                 registry:5000/example/echo:latest
rdfrj4b3m741        echo_nginx          replicated          3/3                 gihyodocker/nginx-proxy:latest
```





```
/stack # docker service ps echo_api
```

![캡처](https://user-images.githubusercontent.com/42603919/72121779-5fe6cf80-339f-11ea-80e4-3cadcf5d614c.PNG)

- manager : d75
- woker01 : 026
- woker02 : e53
- woker03 : f34
- registry : 43e

echo_api는 woker01, woker02, worker03에 배포됨

```
/stack # docker service ps echo_nginx
```

![캡처](https://user-images.githubusercontent.com/42603919/72121863-9fadb700-339f-11ea-954c-5a1e73dcb1aa.PNG)

- manager : d75
- woker01 : 026
- woker02 : e53
- woker03 : f34
- registry : 43e

echo_nginx는 woker01, woker02, worker03에 배포됨

---

### **visualizer를 사용해 시각화하기**

> Swarm 클러스터에 컨테이너 그룹이 어떤 노드에 어떻게 배치됐는지 시각화해주는 애플리케이션
>
> Swarm 클러스터 외부에서 접근할 수 있다.

```yaml
# visualizer.yml

version: "3"
services: 
    app: 
        image: dockersamples/visualizer
        ports: 
            - "9000:8080"
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
        deploy:
            mode: global
            placement:
                constraints: [node.role == manager]
```



```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager docker stack deploy -c /stack/visualizer.yml
 visualizer
```

![캡처](https://user-images.githubusercontent.com/42603919/72125250-3d5ab380-33ab-11ea-84da-387715b9de47.PNG)

---

###  Swarm 클러스터 외부에서 서비스 이용하기

> HAproxy를 사용해 swarm 클러스터 외부에서 echo_nginx 서비스에 접근할 수 있도록 한다.
>
> 외부 호스트에서 요청되는 트래픽을 목적 서비스로 보내주는 프록시 서버 설정

```yaml
# ch03-ingress.yml

version: "3"
services: 
    haproxy: 
        image: dockercloud/haproxy
        networks:
            - ch03
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
        deploy:
            mode: global
            placement:
                constraints: 
                    - node.role == manager
        ports:
            - 80:80
            - 1936:1936
networks:
    ch03:
        external: true
```



```yaml
# ch03-webapi.yml

version: "3"
services: 
    api: 
        image: registry:5000/example/echo:latest
        deploy:
            replicas: 3
            placement:
                constraints: [node.role != manager]
        networks: 
            - ch03
        
    nginx:
        image: gihyodocker/nginx-proxy:latest
        deploy:
            replicas: 3
            placement:
                constraints: [node.role != manager]
        environment: 
            SERVICE_PORTS: 80
            BACKEND_HOST: echo_api:8080
        networks: 
            - ch03
        depends_on:
            - api
        
networks:
    ch03:
        external: true
```



**스택 echo로 배포**

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh
/ # docker stack deploy -c /stack/ch03-webapi.yml echo
```



**스택 ingress로 배포**

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh
/ # docker stack deploy -c /stack/ch03-ingress.yml ingress
```



**관계**

![캡처](https://user-images.githubusercontent.com/42603919/72127961-a0047d00-33b4-11ea-901d-8d6df816e6c3.PNG)



**확인**

```
/ # docker stack ls
```

![캡처](https://user-images.githubusercontent.com/42603919/72128064-f5408e80-33b4-11ea-86f8-eefca728d2c3.PNG)

```
/ # docker service ls
```

![캡처](https://user-images.githubusercontent.com/42603919/72127139-0a67ee00-33b2-11ea-8d9d-bb43456a14b7.PNG)

![캡처](https://user-images.githubusercontent.com/42603919/72127184-2f5c6100-33b2-11ea-98c5-85378f7752b1.PNG)

---

### Swarm을 이용한 실전 애플리케이션 개발

#### MySQL 서비스 구축

```
FROM mysql:5.7

# (1) 패키지 업데이트 및 wget 설치
RUN apt-get update
RUN apt-get install -y wget

# (2) entrykit 설치
RUN wget https://github.com/progrium/entrykit/releases/download/v0.4.0/entrykit_0.4.0_linux_x86_64.tgz
RUN tar -xvzf entrykit_0.4.0_linux_x86_64.tgz
RUN rm entrykit_0.4.0_linux_x86_64.tgz
RUN mv entrykit /usr/local/bin/
RUN entrykit --symlink

# (3) 스크립트 및 각종 설정 파일 복사
COPY add-server-id.sh /usr/local/bin/
COPY etc/mysql/mysql.conf.d/mysqld.cnf /etc/mysql/mysql.conf.d/
COPY etc/mysql/conf.d/mysql.cnf /etc/mysql/conf.d/
COPY prepare.sh /docker-entrypoint-initdb.d
COPY init-data.sh /usr/local/bin/
COPY sql /sql

# (4) 스크립트, mysqld 실행
ENTRYPOINT [ \
  "prehook", \
    "add-server-id.sh", \
    "--", \
  "docker-entrypoint.sh" \
]

CMD ["mysqld"]
```



**이미지 빌드**

```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker build -t localhost:5000/ch04/tododb:latest .
```



**이미지 registry에 push**

```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker push localhost:5000/ch04/tododb:latest
```



**이미지 확인**

![캡처](https://user-images.githubusercontent.com/42603919/72133190-2f655c80-33c4-11ea-9530-e98317c899f6.PNG)

![캡처](https://user-images.githubusercontent.com/42603919/72133068-eca38480-33c3-11ea-9b1b-3f13cb1604b0.PNG)



**네트워크 생성**

```
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker exec -it manager sh

/ # docker network create --driver=overlay --attachable todoapp
```



```
version: "3"

services:
  master:
    image: registry:5000/ch04/tododb:latest
    deploy:
      replicas: 1
      placement:
        constraints: [node.role != manager]
    environment:
      MYSQL_ROOT_PASSWORD: gihyo 
      MYSQL_DATABASE: tododb 
      MYSQL_USER: gihyo 
      MYSQL_PASSWORD: gihyo 
      MYSQL_MASTER: "true"
    networks:
      - todoapp

  slave:
    image: registry:5000/ch04/tododb:latest
    deploy:
      replicas: 2
      placement:
        constraints: [node.role != manager]
    depends_on:
      - master
    environment:
      MYSQL_MASTER_HOST: master
      MYSQL_ROOT_PASSWORD: gihyo 
      MYSQL_DATABASE: tododb 
      MYSQL_USER: gihyo 
      MYSQL_PASSWORD: gihyo 
      MYSQL_ROOT_PASSWORD: gihyo 
      MYSQL_REPL_USER: repl 
      MYSQL_REPL_PASSWORD: gihyo 
    networks:
      - todoapp

networks:
  todoapp:
    external: true

```



**스택 생성(등록)**

```
/ # docker stack deploy -c /stack/todo-mysql.yml todo_mysql
```



**스택 생성 확인**

![캡처](https://user-images.githubusercontent.com/42603919/72134772-2c6c6b00-33c8-11ea-9217-d72deae42660.PNG)



**각각의 worker의 정보 확인하기**

```
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker exec -it worker~ sh
/ # hostname -i -> xxx.xxx.xxx.xxx

/ # docker ps -> 컨테이너 id, master인지 slave인지 확인, port
/ # docker exec -it 컨테이너 id bash

root@컨테이너 id:/# hostname -i -> xxx.xxx.xxx.xxx

-> ex) 192.168.0.5, todo_mysql_slave.2, 3306, 10.0.4.49
```



**mysql master node에 접속**

```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker exec -it worker03 sh

/ # docker exec -it 컨테이너 id bash
root@컨테이너 id:/# -> 접속 완료
```



**데이터베이스 **

```powershell
root@컨테이너 id:/# init-data.sh
root@컨테이너 id:/# mysql -uroot -p tododb
```

![캡처](https://user-images.githubusercontent.com/42603919/72137827-dcdd6d80-33ce-11ea-986b-acdf2ccd4bef.PNG)



**mysql slave node에 접속**

```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker exec -it worker01 또는 worker02 sh

/ # docker exec -it 컨테이너 id bash
root@컨테이너 id:/# -> 접속 완료

root@컨테이너 id:/# mysql -uroot -p tododb
```

![캡처](https://user-images.githubusercontent.com/42603919/72137827-dcdd6d80-33ce-11ea-986b-acdf2ccd4bef.PNG)

#### 잘 복제되어 있다.

