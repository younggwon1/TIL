# CLI로 Instance 시작

[Launch an instance](https://docs.openstack.org/install-guide/launch-instance.html) -> [Self-service network](https://docs.openstack.org/install-guide/launch-instance-networks-selfservice.html) -> [Create m1.nano flavor](https://docs.openstack.org/install-guide/launch-instance.html) -> [Launch an instance on the self-service network](https://docs.openstack.org/install-guide/launch-instance-selfservice.html)



### Controller에서 작업

[root@controller ~(keystone_demo)]# 에서 작업을 한 후



``` 
[root@controller ~(keystone_admin)]# openstack port list --router router
```

![1](https://user-images.githubusercontent.com/42603919/71799229-1ee97500-3098-11ea-9add-2653f643582b.PNG)
![2](https://user-images.githubusercontent.com/42603919/71799230-1f820b80-3098-11ea-9817-f0f1d74ff476.PNG)

CLI로 인스턴스를 생성할 수 있다.



``` 
openstack server create --flavor m1.nano --image 자기 이미지 이름 \
  --nic net-id=자기 net id --security-group default \
  --key-name mykey selfservice-instance
```



자기 net id 확인

``` 
[root@controller ~(keystone_demo)]# openstack network list
```



자기 이미지 이름 확인

``` 
[root@controller ~(keystone_demo)]# openstack image list
```



라우터 주소 확인

``` 
[root@controller ~(keystone_demo)]# ip netns
```



유동 ip 설정

``` 
openstack floating ip create ext1
openstack server add floating ip selfservice-instance 설정한 유동 ip
```



접속하기

``` 
ip netns exec 최근 자기 라우터 주소 ssh cirros@설정한 유동 ip
```

![3](https://user-images.githubusercontent.com/42603919/71800880-9e794300-309c-11ea-85fd-b460c0363398.PNG)



