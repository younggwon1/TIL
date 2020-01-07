# NOVA (Compute Service)

> compute 서비스는 IaaS 시스템의 주요 부분인 클라우드 컴퓨팅의 FC이다.
>
> 클라우드 컴퓨팅 시스템을 관리하는데 사용한다.
>
> Compute는 인증을 위한 Identity 서비스와 이미지를 위한 Image 서비스, 그리고 사용자와 관리자의 인터페이스를 위한 Dashboard와 상호작용한다.

1. NOVA는Dashboard나 command 라인 명령어가 호출하는 nova-api로부터 시작
2. Nova-api는Queue를 통해 nova compute에 인스턴스를생성하라는명령어를전달
3. 인스턴스 생성 명령어를 전달받은 nova compute는 하이퍼바이저(KVM, QEMU) 라이브러리를 통해 하이퍼바이저에게 인스턴스를 생성하라는 명령어를 다시 전달
4. 하이퍼바이저(KVM, QEMU)는 인스턴스를 생성
5. 생성된 인스턴스는 nova-console을 통해 사용자가 접근할 수 있도록 구성됨

---

- 상세 부분

  - nova-api : Nova와 연동을 위한 **[RESTful API](http://sarc.io/index.php/miscellaneous/749)** 이다.

  - nova-scheduler : Queue에 있는 요청들을 수행할 Compute host를 선정하는 역할을 한다. 어느 host에 생성할 것인가 판단을 위해서는 filer를 등록하게 된다. 그 filer를 거치면서 최적의 host를 선택하게 된다.

  - nova-compute : Hypervisor API를 통해 인스턴스를 생성하고 삭제하는 역할을 한다.

  - nova-conductor : nova-compute와 DB 사이에 위치하고 Nova Database에 접근하기 위한 역할을 한다.

  -  nova-novncproxy : 인스턴스의 콘솔 접속을 위한 Proxy 데몬이다.

  - nova-consoleauth : 인스턴스의 콘솔 접속을 위한 인증 데몬이다.

    

### NOVA가지원하는가상화유형

A군 : KVM, QEMU

B군 : 유료 하이퍼바이저 ...

C군 : docker, baremetal ...

#### `Container` 중요



---

### 수동설치(Manual `controller`)

[NOVA 설치 ](https://docs.openstack.org/nova/rocky/install/controller-install-rdo.html)

성공

![2](https://user-images.githubusercontent.com/42603919/71711256-7a6ef500-2e43-11ea-8e59-666ac60d359e.PNG)



### 수동설치 (`Compute1`)

[Compute 설치](https://docs.openstack.org/nova/rocky/install/compute-install-rdo.html)



**compute1에서 실행**

``` shell
# systemctl enable libvirtd.service openstack-nova-compute.service
# systemctl start libvirtd.service openstack-nova-compute.service
```



> **`이때 compute1에서 실행되지 않는다면 controller에서 다음과 같은 과정을 한다.`**



**방화벽 설정(해제, controller에서)**

```shell
vi /etc/sysconfig/iptables
13번 아래에 추가
-A INPUT -s 10.0.0.101/32 -p tcp -m multiport --dports 5671,5672 -m comment --comment "001 amqp incoming amqp_10.0.0.101" -j ACCEPT
-A INPUT -s 10.0.0.101/32 -p tcp -m multiport --dports 5671,5672 -j ACCEPT
-A INPUT -s 10.0.0.100/32 -p tcp -m multiport --dports 5671,5672 -j ACCEPT

systemctl reload iptables
```

**`설정을 한 후 compute1에서 자동으로 다음으로 넘어가면 성공, 하지만 넘어가지 않는다면 다음 과정을 수행`**

**연결 시작하기(compute에서)**

```shell
#systemctl stop openstack-nova-compute
#systemctl start openstack-nova-compute
```



성공(all in one에서 compute 노드를 붙임, controller&compute1)

##### 연결되었는지 확인하기

`controller`에서 확인

![3](https://user-images.githubusercontent.com/42603919/71713432-0f2a2080-2e4d-11ea-8862-18d8f0cef0fc.PNG)

`compute1`에서 확인

``` shell
[root@compute1 ~]# yum install -y openstack-utils
```

``` shell
[root@compute1 ~]# openstack-status
```

![1](https://user-images.githubusercontent.com/42603919/71788878-e763d400-3069-11ea-8b38-c3b8ab5be423.PNG)

``` shell
[root@compute1 ~]# systemctl status libvirtd
```



![2](https://user-images.githubusercontent.com/42603919/71788880-e763d400-3069-11ea-882b-fe96b096d69d.PNG)



##### openstack 홈페이지에 접속(admin 계정(관리자))

**`관리 -> compute -> 하이퍼바이저`**에 들어가서 compute1과 controller가 있는지 확인

![hypervisor](https://user-images.githubusercontent.com/42603919/71788766-bcc54b80-3068-11ea-8ab8-a0aadcf66691.PNG)

![hypervisor1](https://user-images.githubusercontent.com/42603919/71788779-d8c8ed00-3068-11ea-8f7e-e25238697881.PNG)