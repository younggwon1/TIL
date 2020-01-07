# Neutron (Networking Service)

> VNI(Virtual Networking Infrastructure) 가상 네트워크 환경을 위한 네트워크 측면의 모든 것을 관리하고 오픈스택 환경에서 Physical Networking Infrastructure(PNI) 물리 네트워크 환경의 측면에서 Layer에 접속하는 것을 관리한다. 네트워킹에서는 객체 추상화를 통해 네트워크, 서브넷, 라우터를 제공한다.



- **FW (방화벽), LB (Load Balance), VPN (가상 사설망)**

![1](https://user-images.githubusercontent.com/42603919/71789883-e1bdbc80-3070-11ea-9a12-4f6e46c1a378.PNG)



- Neutron 구조

  - 사용자는 Neutron API를 이용하여 Neutron 서버로 ip를 할당을 요청한다.

  - Neutron 서버는 들어온 요청을 Queue로 다시 요청한다.

  - Queue는 Neutron에 에이전트와 플러그인으로 ip 할당 지시를 내린다.

  - Neutron 에이전트와 플러그인은 지시 받은 작업을 데이터베이스에 저장한다.

  - Neutron 에이전트는 네트워크 프로바이더에게 작업을 지시한다.

  - 그리고, 수시로 작업상태를 데이터베이스에 업데이트한다.

  - 이제 할당된 ip를 인스턴스에서 사용할 수 있다.

    

---

``` 
VMware Integrated OpenStack (VIO)
VMware Integrated OpenStack은 VMware 지원 OpenStack 배포 버전으로, VMware 가상화 기술을 기반으로 하여 엔터프라이즈급의 OpenStack 클라우드를 간편하게 실행하도록 지원한다. VMware Integrated OpenStack은 IaaS 플랫폼 구축, 개발자에게 표준화된 OpenStack API 액세스 제공, 엣지 컴퓨팅 활용, OpenStack에 NFV 서비스 배포와 같은 다양한 사용 사례에 적합하다.
```

---



### ML2 (Modular Layer 2)

> ML2는 Havana 버전을 릴리즈하면서 처음으로 나온 플러그인으로, IceHouse에서 기본 플러그인으로 채택 되었다. ML2는 기존에 가장 많이 사용했던 OpenvSwitch, Linuxbridge 대신 다양한 네트워크 형태와 메커니즘을 사용할 수 있다. ML2를 사용하면 OpenvSwitch, Hyper-V, OpenDaylight와 같은 SDN 스위치와 실제 물리 스위치를 사용할 수 있다.또한 이런 스위치를 이용하여 FLAT, VxLAN/GRE, VLAN 방식의 네트워킹 타입을 지원한다.

- type

  - local : 3가지 스위치 모드 = host only, NAT, Bridge mode, 이때 host only모드로 동작하는 경우,

    ​			동일 host안에 같은 네트워크 통신만 가능, 물리 인터페이스를 연결x, 서로 다른 네트워크나 호스     			트간 통신 지원x 

  - FLAT 

  - VLAN 

  - VxLAN/GRE 



- Mechanism Drivers

  > Mechanism Drivers의 Agent는 기존에 사용하던 OpenvSwitch, Linuxbridge, Hyper-V 등을 그대로 사용
  >
  > Arista Mechanism Driver나 Cisco Nexus Mechanism Driver를 사용할 수 있다.



**all in one (controller에서)**

``` shell
[root@controller ~(keystone_admin)]# neutron agent-list
```

![2](https://user-images.githubusercontent.com/42603919/71792071-9b6d5b00-307a-11ea-9d6d-0c1fb8175968.PNG)



``` shell
[root@controller ~(keystone_admin)]# openstack-status
```

![1](https://user-images.githubusercontent.com/42603919/71792070-9ad4c480-307a-11ea-9114-ab457d72c027.PNG)

---

#### 들어올 때는 DNAT, 나갈 때는 SNAT

`DNAT` : destination ip가 사설ip로 바꿈

`SNAT` : source ip가 사설 ip가 되기때문에 (라우터는 사설ip를 거름) 사설 ip를 공인 ip로 바꿈

![2](https://user-images.githubusercontent.com/42603919/71795542-05d9c780-308a-11ea-950b-7c2658dde2f2.PNG)

![1](https://user-images.githubusercontent.com/42603919/71795431-7d5b2700-3089-11ea-9da8-e81037197dda.PNG)

---

### **compute1에서**

[Install and configure compute node](https://docs.openstack.org/neutron/rocky/install/compute-install-rdo.html)

``` shell
[root@compute1 ~]# yum install openstack-neutron-linuxbridge ebtables ipset -y
[root@compute1 ~]# cp /etc/neutron/neutron.conf /etc/neutron/neutron.conf.old
[root@compute1 ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.0.0.100 controller
10.0.0.101 compute1
```

``` shell
[root@compute1 ~]# scp controller:/etc/neutron/neutron.conf /etc/neutron/neutron.conf
```

``` shell
[root@compute1 ~]# chgrp neutron /etc/neutron/neutron.conf
```



[Networking Option 2: Self-service networks](https://docs.openstack.org/neutron/rocky/install/compute-install-option2-rdo.html)

``` shell
[root@compute1 ~]# lsmod|grep br_netfilter
만약 출력이 없다면, 
[root@compute1 ~]# modprobe br_netfilter 
을 한 후 다시
[root@compute1 ~]# lsmod|grep br_netfilter 
실행
br_netfilter           22256  0 
bridge                151336  1 br_netfilter
```



``` shell
[root@compute1 ~]# sysctl -a|grep bridge-nf-call
net.bridge.bridge-nf-call-arptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
1으로 올라와 있으면 성공
```



```shell
[root@compute1 ~]# systemctl enable neutron-linuxbridge-agent.service
[root@compute1 ~]# systemctl start neutron-linuxbridge-agent.service
```



### **controller에서** 확인

``` 
[root@controller ~(keystone_admin)]# openstack network agent list
```

![5](https://user-images.githubusercontent.com/42603919/71797279-a8954480-3090-11ea-88da-008cf10e6a8c.PNG)

Linux bridge agent가 올라와 있으면 성공



