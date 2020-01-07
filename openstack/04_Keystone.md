# 키 기반으로 접속하기

### 1. linux share folder에 키인증 파일 넣기

![key1](https://user-images.githubusercontent.com/42603919/71649048-d3109600-2d4e-11ea-83e3-e6f4d1e7e1a2.PNG)

![s1](https://user-images.githubusercontent.com/42603919/71649095-2387f380-2d4f-11ea-9776-42e58d9765fd.PNG)



### 2. 스냅샷 생성

> 백업 기능

### 볼륨 스냅샷 생성

볼륨 -> 스냅샷 생성 클릭

#### 복구

볼륨생성클릭 -> 생성클릭(원래 만들었던 볼륨이 다시 생성)

![volu](https://user-images.githubusercontent.com/42603919/71649487-8dee6300-2d52-11ea-8a80-9beef13a9f62.PNG)





### 인스턴스 스냅샷 생성

인스턴스 -> 스냅샷 생성 클릭



![a1](https://user-images.githubusercontent.com/42603919/71649585-78c60400-2d53-11ea-9c8b-6383ace55085.PNG)

![a](https://user-images.githubusercontent.com/42603919/71649590-811e3f00-2d53-11ea-8db3-f3ec266f9984.PNG)

![b](https://user-images.githubusercontent.com/42603919/71649592-811e3f00-2d53-11ea-849b-c2bb59f84507.PNG)
![c](https://user-images.githubusercontent.com/42603919/71649593-81b6d580-2d53-11ea-823b-bcc3aeede316.PNG)

![d](https://user-images.githubusercontent.com/42603919/71649587-8085a880-2d53-11ea-98e6-8bf3ed4a9466.PNG)
![e](https://user-images.githubusercontent.com/42603919/71649588-811e3f00-2d53-11ea-8926-bed3920d52f1.PNG)
![f](https://user-images.githubusercontent.com/42603919/71649589-811e3f00-2d53-11ea-9b71-b5fe76e783b9.PNG)

![j](https://user-images.githubusercontent.com/42603919/71649616-b9258200-2d53-11ea-93a7-4bd87c9a2900.PNG)

### 3. Object Storage (swift)

#### 오브젝트 스토리지 -> 컨테이너

![c1](https://user-images.githubusercontent.com/42603919/71649782-c55e0f00-2d54-11ea-9e9c-137c6df9ffbe.PNG)

다른 곳에서 오픈스택을 접근할 경우 다운로드를 하여 설치한다.(서비스를 받을 수 있음)

c1 : private 컨테이너

c2 : 다른 사용자 접근 가능



### 4. OpenStack CLI로 관리하기

### Controller

##### 4.1 Identity 서비스(keystone)

원래 mariadb에 접속하기 위해서

``` shell
[root@controller ~]# mysql -uroot
MariaDB [(none)]>
```



``` 
MariaDB [(none)]> show databases;
```

![1](https://user-images.githubusercontent.com/42603919/71651868-483a9600-2d64-11ea-8215-e8f61091ad85.PNG)

``` 
MariaDB [(none)]> use keystone
MariaDB [keystone]> show tables;
```

![2](https://user-images.githubusercontent.com/42603919/71651869-48d32c80-2d64-11ea-8574-fb83a7f2fde1.PNG)

하지만 **CLI 명령어**로 접근이 가능하다.

- 1.

``` 
[root@controller ~]# cat keystonerc_admin
[root@controller ~]# source keystonerc_admin
[root@controller ~(keystone_admin)]# openstack user list
```

![3](https://user-images.githubusercontent.com/42603919/71651941-f47c7c80-2d64-11ea-9ba3-4e0cfa6c538b.PNG)

- 2.

``` 
[root@controller ~]# . keystonerc_admin
[root@controller ~(keystone_admin)]# openstack user list
```

> 위의 부분은 설치를 하게되면 자동으로

#### 5. 수동설치 (Manual controller)

[환경설정](https://docs.openstack.org/install-guide/environment.html)

[keystone](https://docs.openstack.org/keystone/rocky/install/keystone-install-rdo.html)