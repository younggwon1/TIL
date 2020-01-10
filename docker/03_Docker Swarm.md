# Docker Swarm

> 여러 docker host를 클러스터로 묶어주는 컨테이너 오케스트레이션 (클러스터 구축 및 관리 (주로 멀티 호스트))



**멀티호스트를 하기 위해서 도커 컨테이너 안에서 도커 호스트를 실행한다.(docker in docker => `dind`)**



**`dind` 설치**

```powershell
docker pull docker:19.03.5-dind
```



**registry**(x1)

> local에다가 원하는 이미지를 관리하고 다운받아 저장함



**manager**(x1)

>swarm 클러스터 전체를 제어하는 역할을 한다. 여러 대 실행되는 도커 호스트에 서비스가 담긴 컨테이너를 적절히 배치한다.



**worker**(x3)

>도커 호스트, 여기서는 dind 컨테이너.

**-> 모든 manager, worker 컨테이너는 registry 컨테이너에 의존한다.**

### 이 5개를 설치해 본다.



-  docker-compose를 실행하면 manager, worker, registry 컨테이너가 실행 상태가 된다.

```yaml
version: "3"
services: 
  registry:
    container_name: registry
    image: registry:latest
    ports: 
      - 5000:5000
    volumes: 
      - "./registry-data:/var/lib/registry"

  manager:
    container_name: manager
    image: docker:19.03.5-dind
    privileged: true
    tty: true
    ports:
      - 8000:80
      - 9000:9000
    depends_on: 
      - registry
    expose: 
      - 3375
    command: "--insecure-registry registry:5000"
    volumes: 
      - "./stack:/stack"

  worker01:
    container_name: work01
    image: docker:19.03.5-dind
    privileged: true
    tty: true
    depends_on: 
      - manager
      - registry
    expose: 
      - 7946
      - 7946/udp
      - 4789/udp
    command: "--insecure-registry registry:5000"

  worker02:
    container_name: work02
    image: docker:19.03.5-dind
    privileged: true
    tty: true
    depends_on: 
      - manager
      - registry
    expose: 
      - 7946
      - 7946/udp
      - 4789/udp
    command: "--insecure-registry registry:5000"

  worker03:
    container_name: work03
    image: docker:19.03.5-dind
    privileged: true
    tty: true
    depends_on: 
      - manager
      - registry
    expose: 
      - 7946
      - 7946/udp
      - 4789/udp
    command: "--insecure-registry registry:5000"
```



- 모든 컨테이너가 서로 협조해 클러스터로 동작

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker-compose up
```



- 각각의 컨테이너에 접속하여 ip확인 (다른 powershell)

  ```powershell
  PS C:\Users\HPE\docker\day03\swarm> docker exec -it registry sh
  / # hostname -i -> 172.26.0.2
  PS C:\Users\HPE\docker\day03\swarm> docker exec -it worker01 sh
  / # hostname -i -> 172.26.0.5
  PS C:\Users\HPE\docker\day03\swarm> docker exec -it worker02 sh
  / # hostname -i -> 172.26.0.4
  PS C:\Users\HPE\docker\day03\swarm> docker exec -it worker03 sh
  / # hostname -i -> 172.26.0.6
  PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh
  / # hostname -i -> 172.26.0.3
  ```



**swarm 설정**

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker swarm init
```

**swarm 확인**

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker node ls

ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
id *   docker-desktop      Ready               Active              Leader              19.03.5
```

**swarm 삭제(탈출)**

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker swarm leave --force
```

---

---

#### 하지만 클러스터 관리는 manager가 해야한다. 따라서 다음과 같이 설정을 해야한다.

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker-compose up
```



**manager에서 swarm 설정**

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh
/ # docker swarm init
docker swarm join --token ~~~~
```



**각각의 worker에 연결**(참여)

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker exec -it work01 sh
/ # docker swarm join --token ~~~
PS C:\Users\HPE\docker\day03\swarm> docker exec -it work02 sh
/ # docker swarm join --token ~~~
PS C:\Users\HPE\docker\day03\swarm> docker exec -it work03 sh
/ # docker swarm join --token ~~~
```



**manager에서 확인**

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager docker node ls
```

![1](https://user-images.githubusercontent.com/42603919/72051162-08485580-3306-11ea-88c6-1be976f3f01d.PNG)

---



#### 이미지 등록(로컬에 등록되어 있는 레지스토리에)

![캡처](https://user-images.githubusercontent.com/42603919/72051515-cbc92980-3306-11ea-9de8-03a1b3628ff3.PNG)

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker tag busybox:latest localhost:5000/busybox:latest
```

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker push localhost:5000/busybox:latest
```

![캡처](https://user-images.githubusercontent.com/42603919/72051643-0632c680-3307-11ea-9784-a80049e068e1.PNG)

#### manager에 접속하여 이미지 pull하기

```powershell
PS C:\Users\HPE\docker\day03\swarm> docker exec -it manager sh
```

```
/ # docker pull registry:5000/busybox:latest
```

![1](https://user-images.githubusercontent.com/42603919/72052076-c9b39a80-3307-11ea-9b86-8ca00639286f.PNG)