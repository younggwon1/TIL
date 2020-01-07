# Swift (Object Storage Service)

### Swift 생성 및 다운로드

``` shell
[root@controller ~(keystone_demo)]# swift post demo-c1

[root@controller ~(keystone_demo)]# swift upload demo-c1 cirros-0.3.5-x86_64-disk.img
cirros-0.3.5-x86_64-disk.img

[root@controller ~(keystone_demo)]# swift list demo-c1 --lh
 12M 2020-01-06 07:32:33 application/octet-stream cirros-0.3.5-x86_64-disk.img
 12M
 
[root@controller ~(keystone_demo)]# cd /var/tmp

[root@controller tmp(keystone_demo)]# swift download demo-c1
cirros-0.3.5-x86_64-disk.img [auth 0.735s, headers 1.420s, total 1.578s, 15.737 MB/s]
```

