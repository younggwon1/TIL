# Cinder (Block Storage)

> Block Storage 서비스인 Cinder는 Nova에서 생성된 인스턴스에서 확장하여 사용할 수 있는 저장 공간을 생성 및 삭제하고 인스턴스에 연결할 수 있는 기능을 제공



- LVM(Logical Volume Manager)

  > LVM은 하드디스크를 파티션 대신 논리 볼륨으로 할당하고, 다시 여러 개의 디스크를 좀 더 효율적이고 유연하게 관리할 수 있는 방식을 뜻한다.

- ISCSI(Internet Small Computer System Interface)

  > 컴퓨팅 환경에서 데이터 스토리지 시설을 이어주는 IP기반의 스토리지 네트워킹 표준이다. ISCSI는 IP망을 통해 SCSI 명령을 전달함으로써 인트라넷을 거쳐 데이터 전송을 쉽게하고 먼 거리에 걸쳐 스토리지를 관리하는데 쓰인다. 



1. block storage : ISCSI, cinder, EBS
2. file storage : NFS, Manila, EFS
3. object 기반 storage : swift, S3
4. Database 기반 storage : mysql, Dynamic db



## Cinder 생성

```shell
[root@controller ~(keystone_demo)]# cinder create --name demo-v1 1
```

![1](https://user-images.githubusercontent.com/42603919/71801766-41cb5780-309f-11ea-9e4c-265836257628.PNG)



``` shell
[root@controller ~(keystone_demo)]# cinder list
```

![2](https://user-images.githubusercontent.com/42603919/71801767-41cb5780-309f-11ea-91d9-d03fa91185f5.PNG)





### 인스턴스에 볼륨 할당

``` shell
[root@controller ~(keystone_demo)]# nova volume-attach selfservice-instance 0eb5ad24-1050-47d7-bc4e-4255b0e7ffda auto
```



![3](https://user-images.githubusercontent.com/42603919/71802508-3711c200-30a1-11ea-91e0-d3e40e297e88.PNG)

