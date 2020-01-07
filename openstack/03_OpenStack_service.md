



# 오픈스택 서비스

### 1. 오픈스택 설정 확인

![](./img/3.PNG)

![](https://user-images.githubusercontent.com/42603919/71606702-b6d9f100-2bb6-11ea-8c06-699bf98b9314.PNG)

![](https://user-images.githubusercontent.com/42603919/71606703-b7728780-2bb6-11ea-804d-4347c1b07089.PNG)

![](https://user-images.githubusercontent.com/42603919/71606797-79c22e80-2bb7-11ea-8f62-ff0243033942.PNG)

![](https://user-images.githubusercontent.com/42603919/71606832-a70edc80-2bb7-11ea-920d-7f4fb49459f4.PNG)

### 2. Horizon (사용자 관점에서)

> **private cloud** 구축 완료

#### 2.1 사용자 설정

![](https://user-images.githubusercontent.com/42603919/71606941-a591e400-2bb8-11ea-8a33-cdb153c70698.PNG)

#### 2.2 인증 (keystone)

- **프로젝트** = 리소스 Quotas가 설정된 사용자 그룹이다.

![](https://user-images.githubusercontent.com/42603919/71607062-a414eb80-2bb9-11ea-8947-3e16f8915699.PNG)

![](https://user-images.githubusercontent.com/42603919/71607063-a414eb80-2bb9-11ea-942a-e90b62c4c920.PNG)

![](https://user-images.githubusercontent.com/42603919/71607064-a414eb80-2bb9-11ea-9b7f-75a45b0cdebd.PNG)

- **관리자** 입장에서

![](https://user-images.githubusercontent.com/42603919/71607112-079f1900-2bba-11ea-9f1f-81944797e4ce.PNG)

---

> 사용자 account가 등록되어 있는 곳 : /etc/passwd, /etc/group, /etc/shadow <= **리눅스**

---

#### 2.3 관리(관리자 입장)

![](https://user-images.githubusercontent.com/42603919/71607141-34ebc700-2bba-11ea-96e4-5fbb9f0e0e21.PNG)

> `호스트 집합`은 compute host를 관리하기 위한 목적 -> 사용자x(안보임)
>
> `가용성 존`-> 사용자o(보임)
>
> `인스턴스` : Nova 서비스, 인스턴스를 생성할 수 있는 메뉴x, 관리 목적이다.
>
> `이미지` : Glance 서비스, 설치된 이미지 등록
>
> `Flavor` : 인스턴스 타입(AWS에서)과 비슷, 관리자가 생성할 수 있다.(서비스할 환경에 맞춰서 생성가능)
>
> `볼륨` : CINDER 서비스, 볼륨을 생성한 후 선택해서 사용할 수 있도록 함
>
> `네트워크` : NEUTRON 서비스, 가상 라우트 생성가능

- 시스템 정보(중요)

  ![](https://user-images.githubusercontent.com/42603919/71607361-14bd0780-2bbc-11ea-81ec-f64be3865de4.PNG)     

  

  

### 3. Horizon으로 사용 및 관리하기

![](https://user-images.githubusercontent.com/42603919/71607778-8ba7cf80-2bbf-11ea-886a-a3313d3a26b5.PNG)

1~11 : AWS의 ec2와 동일

12 : AWS의 S3와 동일

4~9 : Self-service

1~3 : IT 관계자

10 : Compute service(IaaS)

11 : AWS의 EBS와 동일

8 : 공인 IP, (Fixed IP : 사설 IP), AWS의 EIP개념과 비슷





#### 3.1 가상 네트워크 인프라 구축

- **관리자 표시만 관리자 부분에서 나머지는 사용자 입장**

- Tenant Private Network 생성 (가상 네트워크 2개)

 두 네트워크를 연결하기 위해서 **`라우터`**를 이용한다.

VPC : 가상 네트워크

>  A 클래스 사설 IP : 10
>
> B 클래스 사설 IP : 172.16 ~ 172.31
>
> C 클래스 사설 IP : 192.168.0 ~ 255



![](./img/asd.jpg)

##### int1 (private) <- subint1

- 192.168.0.0/24, Gateway(Router)

- 192.168.0.254

- DNS : 10.0.0.2

- DHCP

- Fixed IP (사설 ip)

  

##### web

- 192.168.0.1~253



##### ext1 (subext1)

- 외부 통신 (windows와 controller(CentOS))  -> xshell, ssh, web browser(windows)를 통해서 web에 접근이 가능한 상태로 만들어 준다.

- 네트워크 주소 정해짐 : 10.0.0.0/24
- Gateway : 10.0.0.2
- DNS : 10.0.0.2
- Floating IP (공인 ip)

- DHCP(X) -> pool(210~220) : 10.0.0.201,10.0.0.220 (콤마 앞뒤로 스페이스 x)



##### Router

- 각 네트워크를 연결 (ext1 & int1)

  

##### 외부네트워크

- 물리망과 연결이 필요 (br-ex와 ext1(Floating ip) 연결)

  

##### 게이트웨이 설정

- 외부네트워크와 Router를 연결

  

##### 내부 인터페이스

- 내부 인터페이스 추가 (Router와 int1(Fixed ip) 연결)



#### 네트워크,라우터 생성 -> 연결

### 관리자

![network1](https://user-images.githubusercontent.com/42603919/71611133-1d243b00-2bda-11ea-8e84-9f69822bdffd.PNG)

![network2](https://user-images.githubusercontent.com/42603919/71611137-1eedfe80-2bda-11ea-931e-62d03385a158.PNG)

### 사용자

![network3](https://user-images.githubusercontent.com/42603919/71611144-201f2b80-2bda-11ea-9ac3-1099dc0d30e5.PNG)



![network4](https://user-images.githubusercontent.com/42603919/71611148-21e8ef00-2bda-11ea-9b1c-9fa49a3cc295.PNG)

### 관리자

![router1](https://user-images.githubusercontent.com/42603919/71611150-244b4900-2bda-11ea-9a6d-e3246155a3d3.PNG)

![router2](https://user-images.githubusercontent.com/42603919/71611151-257c7600-2bda-11ea-9a34-be128f3f83ab.PNG)

![router3](https://user-images.githubusercontent.com/42603919/71611152-26ada300-2bda-11ea-8718-6b00febe29a8.PNG)



### 최종 네트워크 및 라우터

![](https://user-images.githubusercontent.com/42603919/71611153-27ded000-2bda-11ea-9d05-30e8f6119038.PNG)
![virinfra2](https://user-images.githubusercontent.com/42603919/71611155-28776680-2bda-11ea-8188-7c28beb9eafa.PNG)

---



#### 3.2 보안그룹/Floating IP 생성

> 외부통신이 가능한 보안그룹을 생성

나갈 때는 destinatin ip 들어올 때는 source ip

- CIDR : 허용 IP를 지정해서 들어올 수 있게 하는 방식

![security](https://user-images.githubusercontent.com/42603919/71612170-e2be9c00-2be1-11ea-8e91-9ef0e8befc14.PNG)
![security2](https://user-images.githubusercontent.com/42603919/71612171-e2be9c00-2be1-11ea-857c-eb2de2fcc37d.PNG)





#### DB 생성

![DB1](https://user-images.githubusercontent.com/42603919/71612295-cbcc7980-2be2-11ea-8b8b-fd971801cb64.PNG)
![DB2](https://user-images.githubusercontent.com/42603919/71612296-cbcc7980-2be2-11ea-8aa2-04b67cd0de97.PNG)
![DB3](https://user-images.githubusercontent.com/42603919/71612297-cc651000-2be2-11ea-8571-da69cd7af4ad.PNG)
![DB4](https://user-images.githubusercontent.com/42603919/71612298-cc651000-2be2-11ea-90ae-5cc5bc15670c.PNG)



#### 키 페어 생성

![key](https://user-images.githubusercontent.com/42603919/71612363-49908500-2be3-11ea-8c12-e5114bfc6328.PNG)

![key2](https://user-images.githubusercontent.com/42603919/71612394-7775c980-2be3-11ea-8274-31b321f4cc97.PNG)

이 공개키는 사용자 홈디렉토리/.ssh/authorized_keys에 저장된다. 개인키와 공개키가 맞으면 허용(키 기반 접근)



#### Floating IP 생성

- AWS에서 EIP와 동일

할당만 받음 현재까지는

![FIP](https://user-images.githubusercontent.com/42603919/71612498-2d411800-2be4-11ea-8307-1f49b80de8b4.PNG)





#### 이미지 생성

> root 디스크 설치 이미지

![img](https://user-images.githubusercontent.com/42603919/71612632-ffa89e80-2be4-11ea-8214-ee6fa479f36f.PNG)
![img2](https://user-images.githubusercontent.com/42603919/71612633-ffa89e80-2be4-11ea-8454-77bd935224f6.PNG)



#### 인스턴스 가서 인스턴스 시작 누르기

![i1](https://user-images.githubusercontent.com/42603919/71612795-0a176800-2be6-11ea-8d4b-c3c00085e7bd.PNG)



 **nova쪽 스토리지를 사용하겠다 (local 디스크 스토리지)** <- 새로운 볼륨 아니오

새로운 볼륨 생성 -> ''예'' 로 한다면 Cinder쪽에 저장

![i2](https://user-images.githubusercontent.com/42603919/71612796-0a176800-2be6-11ea-9be5-cb23a7aef4e5.PNG)

![i3](https://user-images.githubusercontent.com/42603919/71612797-0aaffe80-2be6-11ea-80d1-861f4a6fcdc2.PNG)
![i4](https://user-images.githubusercontent.com/42603919/71612798-0aaffe80-2be6-11ea-95bb-f6f369f224df.PNG)
![i5](https://user-images.githubusercontent.com/42603919/71612799-0aaffe80-2be6-11ea-9183-3800ac6fe147.PNG)
![i6](https://user-images.githubusercontent.com/42603919/71612800-0aaffe80-2be6-11ea-8a02-fcaeb3582414.PNG)



#### 인스턴스 생성 완료

![in](https://user-images.githubusercontent.com/42603919/71613101-c32a7200-2be7-11ea-9277-e29b853b6e2a.PNG)



![p](https://user-images.githubusercontent.com/42603919/71613379-82cbf380-2be9-11ea-882e-c45d44314070.PNG)

![p2](https://user-images.githubusercontent.com/42603919/71613398-98d9b400-2be9-11ea-9a02-ad0f950200c3.PNG)

#### 인스턴스 콘솔 실행

![ins1](https://user-images.githubusercontent.com/42603919/71613602-cb37e100-2bea-11ea-8d1b-c92d7739a396.PNG)

![ins](https://user-images.githubusercontent.com/42603919/71613571-a2175080-2bea-11ea-8515-210b1f3686cb.PNG)

![ins2](https://user-images.githubusercontent.com/42603919/71613771-8ceef180-2beb-11ea-8150-740fb2a9b5c1.PNG)





#### 유동ip 연결

![u](https://user-images.githubusercontent.com/42603919/71613837-e1926c80-2beb-11ea-8cfc-29216e43d53e.PNG)
![u1](https://user-images.githubusercontent.com/42603919/71613838-e1926c80-2beb-11ea-8fd4-48d2d4eb9d2c.PNG)





#### 볼륨 생성

![v1](https://user-images.githubusercontent.com/42603919/71613929-4057e600-2bec-11ea-88df-0f7cc5b698c1.PNG)

![v2](https://user-images.githubusercontent.com/42603919/71613952-6d0bfd80-2bec-11ea-9338-9521a7d88c05.PNG)

![v3](https://user-images.githubusercontent.com/42603919/71613983-aa708b00-2bec-11ea-91cf-f6b53d224c58.PNG)

![v4](https://user-images.githubusercontent.com/42603919/71614047-f91e2500-2bec-11ea-8653-88293fbbb1b3.PNG)



콘솔에서 확인

![v5](https://user-images.githubusercontent.com/42603919/71614048-f9b6bb80-2bec-11ea-8cc9-54ab448913e4.PNG)

> vdb가 추가된 것을 알 수 있다. 





**이걸 가지고 라우터로 접근할 수 있다. 라우터에서 해당 가상 서비스에 접근하기 위해서 하는 것**

![xshell](https://user-images.githubusercontent.com/42603919/71614563-c4f83380-2bef-11ea-96f8-3894507c04d3.PNG)

