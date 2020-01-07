# Glance (Image Service)

>오픈스택 이미지 서비스는 사용자가 가상머신 이미지를 발견, 등록 및 검색을 할 수 있도록 해준다.
>
>다양한 하이퍼바이저에서 사용할 수 있는 가상머신 이미지를 관리하고, 가상머신에 설치된 운영체제를 보관 및 관리한다. 



## Controller

``` shell
[root@controller ~]# yum install -y wget

[root@controller ~]# wget https://download.cirros-cloud.net/0.3.5/cirros-0.3.5-x86_64-disk.img
```

[cirros 다운 링크](https://download.cirros-cloud.net/0.3.5/)

![1](https://user-images.githubusercontent.com/42603919/71708364-23155880-2e34-11ea-855b-ee9eb73e21a9.PNG)

- 정보 확인 (정확하게)

  ```shell
  [root@controller ~]# file cirros-0.3.5-x86_64-disk.img
  
  [root@controller ~]# qemu-img info cirros-0.3.5-x86_64-disk.img
  ```

  

- 확장자 변환 (img -> vmdk)

  ```shell
  [root@controller ~]# qemu-img convert -O vmdk cirros-0.3.5-x86_64-disk.img cirros-0.3.5-x86_64-disk.vmdk
  ```

  

- 변환 확인

  ```shell
  [root@controller ~]# file cirros-0.3.5-x86_64-disk.vmdk
  cirros-0.3.5-x86_64-disk.vmdk: VMware4 disk image
  
  [root@controller ~]# qemu-img info cirros-0.3.5-x86_64-disk.vmdk
  image: cirros-0.3.5-x86_64-disk.vmdk
  file format: vmdk
  ```

  

- cirros.vmdk 폴더 카피하고 연결

  >  df -h 한후 vmhgfs-fuse가 연결되었는지 확인 -> 연결이 안되어있으면 파일을 하나 생성하여 한다.

  ``` shell
  [root@controller ~]# mkdir /share
  [root@controller ~]# vmhgfs-fuse /share
  [root@controller ~]# df
  
  vmhgfs-fuse         487822356 96378348 391444008  20% /share
  ```

  

       ``` shell
[root@controller ~]# cp cirros-0.3.5-x86_64-disk.vmdk /share/data_share
       ```

> data_share에 있는 vmdk파일을 windows에 있는 cirros폴더로 옮긴다.

- vmware에 가상 OS(cirros)를 만들어 로그인하면 성공

  ![1](https://user-images.githubusercontent.com/42603919/71703069-200b6f80-2e16-11ea-936f-21da6555ac50.PNG)



``` shell
[root@controller ~]#. keystonerc_stack1
[root@controller ~(keystone_stack1)]# 
```



``` shell
[root@controller ~(keystone_stack1)]# openstack image create "cirros-vmdk" --file /root/cirros-0.3.5-x86_64-disk.vmdk --disk-format vmdk --container-format bare
```



- Glance 이미지 리스트 확인

``` shell
[root@controller ~(keystone_stack1)]# glance image-list
```

![2](https://user-images.githubusercontent.com/42603919/71703356-775e0f80-2e17-11ea-9886-c7ef963ebed4.PNG)



- Glance 이미지 정보 확인하기

  ```shell
  [root@controller ~(keystone_stack1)]# glance image-show ID
  ```

  ![3](https://user-images.githubusercontent.com/42603919/71703497-2ac70400-2e18-11ea-8d1c-facc518da2a6.PNG)



## Manual-controller



[Glance install](https://docs.openstack.org/glance/rocky/install/)

-> Red Hat 클릭 (Install and configure (Red Hat))



### Glance 데이터베이스 설정

[설치과정](https://docs.openstack.org/glance/rocky/install/install-rdo.html)

---



