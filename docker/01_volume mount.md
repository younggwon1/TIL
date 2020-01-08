# Docker

### volume mount

> Docker 공유 폴더

**-v** 옵션 : 필요한 데이터 폴더를 공유

- -v 호스트 폴더 : 컨테이너 폴더



### mysql 설치

#### mysql 데이터를 볼륨마운트에 저장하기

![캡처](https://user-images.githubusercontent.com/42603919/71956524-c6001500-322e-11ea-93c7-d7c7a880db69.PNG)

**이미지 빌드시키기**

```powershell
PS C:\Users\HPE\docker\day02\vd> docker build -t example/mysql-data:latest .
```



**컨테이너 띄우기(데이터 볼륨 컨테이너)**

```powershell
PS C:\Users\HPE\docker\day02\vd> docker run -d --name mysql-data example/mysql-data:latest
```



**mysql 띄우기1**

```
docker run -d --rm --name mysql `
      -e "MYSQL_ALLOW_EMPTY_PASSWORD=yes" `
      -e "MYSQL_DATABASE=volume_test" `
      -e "MYSQL_USER=example" `
      -e "MYSQL_PASSWORD=example" `
      --volumes-from mysql-data `
      mysql:5.7
```

```powershell
PS C:\Users\HPE\docker\day02\vd> docker exec -it mysql bash
root@a5198651608a:/# mysql -uroot -p volume_test
# mysql 로그인
```



**mysql  root에 접속하여 테이블 만들기 -> 데이터 삽입** 



**첫번째로 띄운 mysql 멈추기(--rm을 하여 삭제됨)**

```powershell
PS C:\Users\HPE\docker\day02\vd> docker stop a5198651608a
```



**첫번째 mysql 삭제 후 mysql 띄우기2**

```
docker run -d --rm --name mysql `
      -e "MYSQL_ALLOW_EMPTY_PASSWORD=yes" `
      -e "MYSQL_DATABASE=volume_test" `
      -e "MYSQL_USER=example" `
      -e "MYSQL_PASSWORD=example" `
      --volumes-from mysql-data `
      mysql:5.7
```

```powershell
PS C:\Users\HPE\docker\day02\vd> docker exec -it mysql mysql -uroot -p volume_test
# mysql 로그인
```



**데이터 확인 (select * from tables)**



**--> 데이터가 그대로 남아있다. : 애플리케이션 컨테이너와 데이터 볼륨 컨테이너를 분리하면 쉽게 테이터와 컨테이너를 교체할 수 있다.**



---

```powershell
PS C:\Users\HPE\docker\day02\vd> docker exec -it mysql bash
root@a5198651608a:/# mysql -uroot -p volume_test
과
PS C:\Users\HPE\docker\day02\vd> docker exec -it mysql mysql -uroot -p volume_test
은 동일
```

---



여러개의 컨테이너를 띄우기에는 매우 불편한 상황이었다.

따라서 **docker compose**를 이용하여 이 불편한 상황을 해결한다.

---



### docker compose

> 도커 커맨드 또는 복잡한 설정을 쉽게 관리하기 위한 도구

```
version:
services:
	echo:
	  image:
	  ports:
	  
풀어 써보면,

services.echo.image
services.echo.ports
```

> services 요소 아래의 echo는 컨테이너 이름으로, 그 아래에 다시 어떤 이미지를 실행할지가 정의된다.
>
> image요소는 도커 이미지, ports는 포트 포워딩 설정을 지정한다.



#### docker compose 파일

**1. mysql**

![캡처](https://user-images.githubusercontent.com/42603919/71956917-b92ff100-322f-11ea-86c7-0e9fae7e0fcd.PNG)

```powershell
docker run -d -p 3306:3306 --name my-mysql mysql:5.7
```

**-> 위의 두개는 동일**







**서버가 2대 띄워짐**

![캡처](https://user-images.githubusercontent.com/42603919/71958297-4d4f8780-3233-11ea-9b1b-c334570b4f7a.PNG)

```
docker run -d -p 13306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name
my-mysql mysql:5.7

docker run -d -p 23306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name
my-mysql mysql:5.7
```

**-> 위의 두개는 동일**



```
PS C:\Users\HPE\docker\day02\vd> docker-compose up
```

![캡처](https://user-images.githubusercontent.com/42603919/71958345-6ce6b000-3233-11ea-9ba3-e4ee78ee08a0.PNG)

**서버 다운시키기**

```
PS C:\Users\HPE\docker\day02\vd> docker-compose down
```

![캡처](https://user-images.githubusercontent.com/42603919/71958374-7ec85300-3233-11ea-8f57-5e4e3f3646d8.PNG)



---

**2. mongoDB**

**mongoDB를 Docker container로 실행**

- Dockerfile 작성 (mongodb 설치를 위한 이미지 생성)

  ![1](https://user-images.githubusercontent.com/42603919/71960560-af5ebb80-3238-11ea-8442-b7cb29756e74.PNG)

- Dockerfile 이미지 빌드

  ```powershell
  PS C:\Users\HPE\docker\day02\mongo> docker build -t dlrjsapdlf622/mymongodb:latest .
  ```

  ![2](https://user-images.githubusercontent.com/42603919/71960561-af5ebb80-3238-11ea-9438-6ab95923214c.PNG)

- MongoDB container 생성 -> 실행(서버)

  ```powershell
  PS C:\Users\HPE\docker\day02\mongo> docker run -p 27017 dlrjsapdlf622/mymongodb:latest
  ```

- Client에서 mongodb 테스트(클라이언트)

```powershell
PS C:\Users\HPE> docker exec -it 6e7c63256fe8 bash
root@6e7c63256fe8:/# mongo

> show dbs;
admin   0.000GB
config  0.000GB
local   0.000GB
> use bookstore;
switched to db bookstore
> db.books.save({'title':"docker"});
WriteResult({ "nInserted" : 1 })
> db.books.find();
{ "_id" : ObjectId("5e158bcda5b65a6f0e46ac38"), "title" : "docker" }
```

---



#### 



























