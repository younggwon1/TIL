```
PS C:\Users\HPE\docker\day03\swarm> docker-compose up
```



```
PS C:\Users\HPE\docker\day03\swarm> docker ps
manager, worker01, worker02, worker03, registry 확인
```



manager에서 **swarm** 설정

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh
/ # docker swarm init
docker swarm join --token ~~~~
```



각각의 worker에 연결

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it work01 sh
/ # docker swarm join --token ~~~
PS C:\Users\HPE\docker\day03\swarm> docker exec -it work02 sh
/ # docker swarm join --token ~~~
PS C:\Users\HPE\docker\day03\swarm> docker exec -it work03 sh
/ # docker swarm join --token ~~~
```



manager에서 목록 확인

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager docker node ls
```



---

이미지 등록

```
PS C:\Users\HPE\docker\day03\swarm> docker tag busybox:latest localhost:5000/busybox:latest
```

```
PS C:\Users\HPE\docker\day03\swarm> docker push localhost:5000/busybox:latest
```



manager에 접속하여 이미지 pull하기

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh
/ # docker pull registry:5000/busybox:latest
docker images로 확인
```

---



visualizer를 사용해 시각화하기

```
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager docker stack deploy -c /stack/visualizer.yml visualizer
```



MySQL 서비스 구축

이미지 빌드

```
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker build -t localhost:5000/ch04/tododb:latest .
```



이미지 registry에 push

```
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker push localhost:5000/ch04/tododb:latest
```



이미지 확인

```
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> curl http://localhost:5000/v2/_catalog
```

![캡처](https://user-images.githubusercontent.com/42603919/72133068-eca38480-33c3-11ea-9b1b-3f13cb1604b0.PNG)



네트워크 생성

```
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker exec -it manager sh

/ # docker network create --driver=overlay --attachable todoapp
```



스택 생성(등록)

```
/ # docker stack deploy -c /stack/todo-mysql.yml todo_mysql
```

![캡처](https://user-images.githubusercontent.com/42603919/72134772-2c6c6b00-33c8-11ea-9217-d72deae42660.PNG)



각각의 worker의 정보 확인하기

```
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker exec -it worker~ sh
/ # hostname -i -> xxx.xxx.xxx.xxx

/ # docker ps -> 컨테이너 id, master인지 slave인지 확인, port
/ # docker exec -it 컨테이너 id bash

root@컨테이너 id:/# hostname -i -> xxx.xxx.xxx.xxx

-> ex) 192.168.0.5, todo_mysql_slave.2, 3306, 10.0.4.49
```



mysql master node에 접속

```
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker exec -it worker03 sh

/ # docker exec -it 컨테이너 id bash
root@컨테이너 id:/# -> 접속 완료
```



데이터베이스

```
root@컨테이너 id:/# init-data.sh
root@컨테이너 id:/# mysql -uroot -p tododb
```

![캡처](https://user-images.githubusercontent.com/42603919/72137827-dcdd6d80-33ce-11ea-986b-acdf2ccd4bef.PNG)



mysql slave node에 접속

```
PS C:\Users\HPE\docker\day03\swarm\todo\tododb> docker exec -it worker01 또는 worker02 sh

/ # docker exec -it 컨테이너 id bash
root@컨테이너 id:/# -> 접속 완료

root@컨테이너 id:/# mysql -uroot -p tododb
```

![캡처](https://user-images.githubusercontent.com/42603919/72137827-dcdd6d80-33ce-11ea-986b-acdf2ccd4bef.PNG)

