# Docker 

## Swarm을 이용한 실전 어플리케이션 개발 - API 서비스 구축

>TODO 앱의 도메인을 담당할 API를 만들어 본다.

```dockerfile
# todoapi

FROM golang:1.10

WORKDIR /
ENV GOPATH /go

COPY . /go/src/github.com/gihyodocker/todoapi
RUN go get github.com/go-sql-driver/mysql
RUN go get gopkg.in/gorp.v1
RUN cd /go/src/github.com/gihyodocker/todoapi && go build -o bin/todoapi cmd/main.go
RUN cd /go/src/github.com/gihyodocker/todoapi && cp bin/todoapi /usr/local/bin/

CMD ["todoapi"]
```



**이미지 빌드**

```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\todoapi> docker build -t localhost:5000/ch04/todoapi:latest .
```



**이미지 push**

```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\todoapi> docker push localhost:5000/ch04/todoapi:latest
```

![캡처](https://user-images.githubusercontent.com/42603919/72304932-7e0d4200-36b5-11ea-92b1-5bbf5d9efdad.PNG)

#### -> todoapi의 도커 이미지 준비 완료



**Swarm에서 todoapi 서비스 실행**

```yaml
# todo-app.yml

version: "3"
services:
  nginx:
    image: registry:5000/ch04/nginx:latest
    deploy:
      replicas: 2
      placement:
        constraints: [node.role != manager]
    depends_on:
      - api
    environment:
      WORKER_PROCESSES: 2
      WORKER_CONNECTIONS: 1024
      KEEPALIVE_TIMEOUT: 65
      GZIP: "on"
      BACKEND_HOST: todo_app_api:8080
      BACKEND_MAX_FAILES: 3
      BACKEND_FAIL_TIMEOUT: 10s
      SERVER_PORT: 8000
      SERVER_NAME: todo_app_nginx
      LOG_STDOUT: "true"
    networks:
      - todoapp

  api:
    image: registry:5000/ch04/todoapi:latest 
    deploy:
      replicas: 2
      placement:
        constraints: [node.role != manager]
    environment:
      TODO_BIND: ":8080"
      TODO_MASTER_URL: "gihyo:gihyo@tcp(todo_mysql_master:3306)/tododb?parseTime=true"
      TODO_SLAVE_URL: "gihyo:gihyo@tcp(todo_mysql_slave:3306)/tododb?parseTime=true"
    networks:
      - todoapp

networks:
  todoapp:
    external: true
```



**deploy 명령으로 todo_app 스택 배포**

>그 결과로 'Listen HTTP Server'라는 내용이 출력되면 API 서버가 HTTP요청을 받을 수 있는 상태가 된다.

```
PS C:\Users\HPE\docker\day03\swarm\todo\todoapi> docker exec -it manager sh

/ # docker stack deploy -c /stack/todo-app.yml todo_app
```



#### 확인하기

````
PS C:\Users\HPE\docker\day03\swarm\todo\todoapi> docker exec -it manager sh

/ # docker service ls
````

![캡처](https://user-images.githubusercontent.com/42603919/72309079-47d5bf80-36c1-11ea-98e6-34df67ab84b2.PNG)

```
/ # docker service ps todo_app_api
```

![1](https://user-images.githubusercontent.com/42603919/72309080-47d5bf80-36c1-11ea-9c03-cce08defb4ff.PNG)

![캡처](https://user-images.githubusercontent.com/42603919/72313611-398ea000-36cf-11ea-994f-685183d473c9.PNG)

**각각의 api ip주소 확인**

```
PS C:\Users\HPE\docker\day03\swarm\todo\todoapi> docker exec -it worker01 sh
/ # docker ps -> 컨테이너 id확인

/ # docker exec -it 컨테이너 id bash
root@컨테이너 id:/# hostname -i
-> 10.0.x.xxx

PS C:\Users\HPE\docker\day03\swarm\todo\todoapi> docker exec -it worker02 sh
/ # docker ps -> 컨테이너 id확인

/ # docker exec -it 컨테이너 id bash
root@컨테이너 id:/# hostname -i
-> 10.0.x.xxx
```



**각각의 포트번호 확인**

```
root@컨테이너 id:/# apt-get update
root@컨테이너 id:/# apt-get install -y net-tools
```



```
PS C:\Users\HPE\docker\day03\swarm\todo\todoapi> docker exec -it worker01 sh
/ # docker ps -> 컨테이너 id확인

/ # docker exec -it 컨테이너 id bash
root@컨테이너 id:/# netstat -ntpl
-> 8080

PS C:\Users\HPE\docker\day03\swarm\todo\todoapi> docker exec -it worker02 sh
/ # docker ps -> 컨테이너 id확인

/ # docker exec -it 컨테이너 id bash
root@컨테이너 id:/# netstat -ntpl
-> 8080
```



**serveGET(api1또는 api2에 접속해서)**

> GET 메서드는 요청 파라미터 status를 받아 todo 테이블에서 SELECT 쿼리를 실행하고 조건이 일치하는 JSON 포맷으로 된 레코드의 배열을 응답으로 반환한다.

```
root@컨테이너 id:/# curl -XGET http://localhost:8080/todo?status=TODO
root@컨테이너 id:/# curl -XGET http://localhost:8080/todo?status=PROGRESS
root@컨테이너 id:/# curl -XGET http://localhost:8080/todo?status=DONE
```

![캡처](https://user-images.githubusercontent.com/42603919/72310090-2a562500-36c4-11ea-834c-208b33fc82e3.PNG)



**servePOST(api1또는 api2에 접속해서)**

> POST 메서드는 새로운 TODO를 추가한다. 추가할 TODO의 내용을 다음과 같이 JSON으로 전달하면 그 내용을 새로운 TODO로 추가한다.

```
root@컨테이너 id:/# curl -XPOST -d '{"title":"test modified" ,"content":"test text"}' http://localhost:8080/todo
```

![캡처](https://user-images.githubusercontent.com/42603919/72313298-48c11e00-36ce-11ea-88e6-f52a15d246b9.PNG)



**servePUT(api1또는 api2에 접속해서)**

> PUT메서드는 이미 추가된 TODO를 수정한다. id로 지정된 레코드가 이미 존재하는 경우 해당 레코드의 내용을 수정하며 존재하지 않으면 404를 반환한다.

```
root@컨테이너 id:/# curl -XPUT -d '{"id":9, "title":"test modified" ,"content":"test text", "status": "PROGRESS"}' http://localhost:8080/todo
```

![캡처](https://user-images.githubusercontent.com/42603919/72313532-fd5b3f80-36ce-11ea-975b-9e3156eedbf7.PNG)

---



### Ngnix 구축하기

```dockerfile
# todonginx

FROM nginx:1.13

RUN apt-get update
RUN apt-get install -y wget
RUN wget https://github.com/progrium/entrykit/releases/download/v0.4.0/entrykit_0.4.0_linux_x86_64.tgz
RUN tar -xvzf entrykit_0.4.0_linux_x86_64.tgz
RUN rm entrykit_0.4.0_linux_x86_64.tgz
RUN mv entrykit /usr/local/bin/
RUN entrykit --symlink

RUN rm /etc/nginx/conf.d/*
COPY etc/nginx/nginx.conf.tmpl /etc/nginx/
COPY etc/nginx/conf.d/ /etc/nginx/conf.d/

ENTRYPOINT [ \
  "render", \
      "/etc/nginx/nginx.conf", \
      "--", \
  "render", \
      "/etc/nginx/conf.d/upstream.conf", \
      "--", \
  "render", \
      "/etc/nginx/conf.d/public.conf", \
      "--" \
]

CMD ["nginx", "-g", "daemon off;"]
```



**이미지 빌드**

```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\todonginx> docker build -t localhost:5000/ch04/nginx:latest .
```

![캡처](https://user-images.githubusercontent.com/42603919/72315919-cc333d00-36d7-11ea-99cd-79cb68a461ac.PNG)

![캡처](https://user-images.githubusercontent.com/42603919/72315942-dead7680-36d7-11ea-9531-fb0f23e84148.PNG)

**이미지 push**

```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\todonginx> docker push localhost:5000/ch04/nginx:latest
```

-> 위의 부분을 실행할 때 todo-app.yml의 nginx 부분을 주석처리하고 진행했기 때문에 주석을 풀고 다시 stack deploy를 진행한다.



**deploy 명령으로 todo_app 스택 배포**

```
PS C:\Users\HPE\docker\day03\swarm\todo\todonginx> docker exec -it manager sh

/ # docker stack deploy -c /stack/todo-app.yml todo_app
```



**확인하기**

```
PS C:\Users\HPE\docker\day03\swarm\todo\todonginx> docker exec -it manager sh

/ # docker stack services todo_app
```

![캡처](https://user-images.githubusercontent.com/42603919/72316096-55e30a80-36d8-11ea-8ae9-39d284d03823.PNG)

```
/ # docker service ps todo_app_nginx
```

![캡처](https://user-images.githubusercontent.com/42603919/72316125-698e7100-36d8-11ea-8c48-908871521dca.PNG)

![캡처](https://user-images.githubusercontent.com/42603919/72316644-3a78ff00-36da-11ea-8cf8-e7ca73b798d1.PNG)



**Nginx.1에 접속**

```
PS C:\Users\HPE\docker\day03\swarm\todo\todonginx> docker exec -it worker02 sh

root@컨테이너 id:/# curl http://localhost:8000
```



```
root@d98bf197fa4d:/# curl -XGET http://localhost:8000/todo?status=TODO

[{"id":8,"title":"인그레스 구축하기","content":"외부에서 스웜 클러스터에 접근하게 해주는 인그레스 구축","status":"TODO","created":"2020-01-14T02:47:40Z","updated":"2020-01-14T02:47:40Z"}]
```

---



### 웹 서비스 구축

```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\todoweb> docker build -t localhost:5000/ch04/todoweb:latest .
```



```powershell
PS C:\Users\HPE\docker\day03\swarm\todo\todoweb> docker push localhost:5000/ch04/todoweb:latest
```



다음시간에 하기













