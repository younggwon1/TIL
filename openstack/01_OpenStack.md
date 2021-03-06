# 오픈스택(2019.12.30)

### 1.1 클라우드 컴퓨팅

> **`클라우드 컴퓨팅`** 이란 사용자의 요청에 따라 공유된 컴퓨터의 자원이나 데이터를 사용자가 이용하는 컴퓨터 및 휴대폰과 같은 다른 장치로 제공하는 인터넷 기반의 컴퓨팅 환경
>
> 클라우드(인터넷)을 통해 가상화된 컴퓨터의 시스템리소스(IT리소스)를 제공하는 것이다.

- 소비자 => **IaaS** : IT관계자, **PaaS** : 개발자, **SaaS** : End user(기업, 일반 사용자) 

- **DIY** : IaaS 사용하는 방식, 스스로 직접

- **on-premise** : 소프트웨어 등 솔루션을  클라우드 같이 원격 환경이 아닌 자체적으로 보유한 전산실 서버에 직접 설치해 운영하는 방식

- 산업 표준이 **오픈스택**으로 가고 있다.

- **kernel** : 운영체제에서 핵심 프로그램

- **Scale-up** : 서버의 자체 성능을 증가시키는 것

- **Scale-out** : 기존의 서버와 같은 사양 또는 비슷한 사양의 서버 대수를 증가시키는 방법으로 처리 능력을 향샹시키는 것

- **Hypervisor** : 호스트 컴퓨터에서 다수의 운영체제를 동시에 실행하기 위한 논리적 플랫폼을 말한다.

  ![](https://user-images.githubusercontent.com/42603919/71605937-44661280-2bb0-11ea-863f-ca8997a3981c.PNG)

![](https://user-images.githubusercontent.com/42603919/71605943-53e55b80-2bb0-11ea-816c-2b542c9103fc.PNG)

### 1.2 오픈스택이란?

> **`오픈스택`**은 클라우드 컴퓨팅의 IaaS로서 클라우드 컴퓨팅 환경에서 사용되는 무료 오픈소프 클라우드 소프트웨어이다. 오픈스택은 데이터센터를 통해서 스토리지나 네트워크 자원같은 다양한 종류의 하드웨어를 제어하는 요소로 구성되어 있고, 이 구성 요소들은 밀접한 연관을 가지고 있다. 
>
> IaaS를 제공하기 위해 데이터센터의 인프라를 제어하는 클라우드 오퍼레이팅 시스템(소프트웨어)라고 할 수 있다.

![](https://user-images.githubusercontent.com/42603919/71605950-5fd11d80-2bb0-11ea-9043-94acc372c41b.PNG)

위의 이미지와 같이 common H/W(Server, Network, Storage)와 S/W기반에서 오픈스택이라는 제어도구를 활용하여 가상의 인프라 리소스를 IaaS 형태로 제공하는 역할을 담당한다. 즉, 오픈스택은 Public Cloud와 Private Cloud 환경을 구축할 수 있도록 해주는 오픈소스 소프트웨어로 **`SDDC(software defined data center)`** 의 기반 플랫폼이라고도 정의할 수 있다.

![](https://user-images.githubusercontent.com/42603919/71605952-61024a80-2bb0-11ea-98a6-7cb324f6dd0d.PNG)

가상화 업무는 고객의 서비스 요구에 따라 각 요소별 전문가들이 역할을 수행하여 구성되는 반면, Cloud의 경우  Self-Provisioning 또는 단일화된 창구를 통해 서비스 인프라가 구성된다.

즉, 모두 가상화 기술을 이용하여 H/W 자원의 효율성을 비슷하지만 리소스 제공의 시간이 단축되므로 업무 효율성을 증가시켜 준다. 이처럼 Cloud의 기반이 되는 것이 오픈스택이라 할 수 있다.

- Nova : 컴퓨터
- Swift : 객체 저장소
-  Glance : 이미지 관리
-  Keystone : 인증관리
-  Horizon : 인터페이스
-  Neutron : 네트워킹
-  Cinder : 블록저장소
-  Ceilometer : 모니터링과 계량기
-  Heat : Orchestration

![](https://user-images.githubusercontent.com/42603919/71605958-6cee0c80-2bb0-11ea-9fde-7ed83481f950.PNG)

[오픈스택 서비스 추가 내용](https://arisu1000.tistory.com/27767)

### storage 유형

- **object 기반** : `swift / S3`, RESTful API로 접근하고, HTTP 프로토콜 이용하여 CRUD Operation이 가능하다. "File + Metadata"
- **block 기반** : `cinder`, "server가 EBS, client가 EC2". 장치명 형태로 붙는다.(장치file로 연결. /dev/xada, /dev/xadb, /devxadc...)
- **file 기반** : `Manila`, N/W 기반으로, directory로 mount해서 사용한다.(특정 디렉토리 공유)
- **database 기반**

[오픈스택 참고](https://docs.openstack.org/train/)

[오픈스택 참고1](https://galid1.tistory.com/207?category=764058)

### 가상화 유형

- Hypervisor

  - Full - virtualization (전 가상화)

    > 말그대로 하드웨어를 완전히 가상화하는 방식이다. 하이퍼바이저를 베어메탈에 구동하면, DOM0라고 하는 관리용 가상머신이 구동된다. 전가상화는 나머지 모든 가상머신들의 하드웨어 접근이 이 DOM0를 통해서 이뤄진다. 모든 명령에 대해서 DOM0가 개입을 하게 되는 형태이다.
    >
    > 하드웨어를 완전히 가상화하기 때문에 게스트 운영체제의 별다른 수정없이 사용할 수 있다는 점이 장점이다. 하지만 모든 명령을 중재하게 되기 때문에 성능이 비교적 느리다.

  - Para - virtualization (반 가상화)

    > 하드웨어를 완전히 가상화하지 않는다. 전가상화의 가장 큰 단점인 성능 저하를 해소하고자 모든 명령을 DOM0를 통해 하이퍼바이저에게 요청하는 것이 아니라 하이퍼콜(Hyper Call)이라는 인터페이스로 직접 하이퍼바이저에게 요청을 날릴 수 있게 해주는 것이다. 따라서 성능이 빠르다는 장점이 있다. 하지만 하이퍼콜을 이용하여 하이퍼바이저에게 요청을 보내는 동작을 기존 OS들은 할 수 없다. 즉, 게스트 운영체제가 하드웨어의 가상화 여부를 알고 있어야 하며, 필요한 경우 하이퍼바이저에게 하이퍼콜을 요청할 수 있도록 운영체제의 커널을 수정해야 한다. 따라서 오픈소스 운영체제가 아니라면 반가상화를 이용하기 쉽지가 않다.

- Container



### 오픈스택 Architecture

- **all-in-one**

  ![](https://user-images.githubusercontent.com/42603919/71605945-55168880-2bb0-11ea-88e9-31731cf60451.PNG)

- **two node**

  ![](https://user-images.githubusercontent.com/42603919/71605946-55af1f00-2bb0-11ea-8096-f02b771eee56.PNG)

- **three node**

  ![](https://user-images.githubusercontent.com/42603919/71605947-56e04c00-2bb0-11ea-8424-31b0f48e0a67.PNG)

  

---

### NTP 서버

> NTP서버라 함은 Network Time Protocol 말 그대로 네트워크로 구성된 환경에서 구동되는 시스템들의 시간을 동기화하기 위한 규약( Protocol )이다.
>
> NTP는 계층적인 구조를 가지는데 각각의 계층은 상위 계층으로부터 시간을 동기화한다. NTP Client 세팅으로 상위 계층에서 시간 정보를 받아 동기화할 수 있고 NTP Server 세팅을 통해 다른 장비들에게 정보를 보낼 수 있다.

- 시간 동기화 프로토콜이다.
- 포트번호는 UDP 123번을 사용한다.
- 모든 Server와 Network Device 등의 시스템 시간 정보를 갖고 있다.
- 관리자가 설정을 통해 시간을 변경하는 것이 가능하다.