# OpenStack_综合练习

## 创建实例

### 创建cirros实例

​		本次练习主要对创建网络、创建镜像、创建实例类型、创建浮动IP等内容的综合使用，来创建一个实例。

#### 实例介绍

实例名：cirros

数量：1

镜像：cirros

实例类型：2C2G20GB

网络：internal

浮动IP：192.168.200.69

#### 开始创建

``` shell
# 检查镜像
[root@controller ~]# openstack image list
+--------------------------------------+--------+--------+
| ID                                   | Name   | Status |
+--------------------------------------+--------+--------+
| 9fdae471-668e-4341-b85c-5aa3a8e2db0d | cirros | active |
+--------------------------------------+--------+--------+
# 检查实例类型
[root@controller ~]# openstack flavor list
+----+----------+------+------+-----------+-------+-----------+
| ID | Name     |  RAM | Disk | Ephemeral | VCPUs | Is Public |
+----+----------+------+------+-----------+-------+-----------+
| 6  | 2C2G20GB | 2048 |   20 |         0 |     1 | True      |
+----+----------+------+------+-----------+-------+-----------+
# 检查网络
[root@controller ~]# openstack network list
+--------------------------------------+----------+--------------------------------------+
| ID                                   | Name     | Subnets                              |
+--------------------------------------+----------+--------------------------------------+
| 5e10f730-dd0f-453b-b4c5-31be43c664a4 | internal | 54c58387-2878-4b06-8820-0c3335f8094b |
| 8c7505aa-6770-436a-a109-9bd145beded1 | external | 87081291-9dcd-4be4-9ba0-88b820ef19a5 |
+--------------------------------------+----------+--------------------------------------+
# 查看浮动IP
[root@controller ~]# openstack floating ip list
+--------------------------------------+---------------------+------------------+------+--------------------------------------+----------------------------------+
| ID                                   | Floating IP Address | Fixed IP Address | Port | Floating Network                     | Project                          |
+--------------------------------------+---------------------+------------------+------+--------------------------------------+----------------------------------+
| a4d42a58-a1f3-4669-9d1b-a4aebd4f9096 | 192.168.200.69      | None             | None | 8c7505aa-6770-436a-a109-9bd145beded1 | 57e7eb094ace4170ab2c4792c23ee6e7 |
+--------------------------------------+---------------------+------------------+------+--------------------------------------+----------------------------------+
# 创建云主机
[root@controller ~]# openstack server create --image cirros --flavor 2C2G20GB --network internal cirros
+-------------------------------------+-----------------------------------------------+
| Field                               | Value                                         |
+-------------------------------------+-----------------------------------------------+
| OS-DCF:diskConfig                   | MANUAL                                        |
| OS-EXT-AZ:availability_zone         |                                               |
| OS-EXT-SRV-ATTR:host                | None                                          |
| OS-EXT-SRV-ATTR:hypervisor_hostname | None                                          |
| OS-EXT-SRV-ATTR:instance_name       |                                               |
| OS-EXT-STS:power_state              | NOSTATE                                       |
| OS-EXT-STS:task_state               | scheduling                                    |
| OS-EXT-STS:vm_state                 | building                                      |
| OS-SRV-USG:launched_at              | None                                          |
| OS-SRV-USG:terminated_at            | None                                          |
| accessIPv4                          |                                               |
| accessIPv6                          |                                               |
| addresses                           |                                               |
| adminPass                           | oe95PwLv29YX                                  |
| config_drive                        |                                               |
| created                             | 2021-03-20T11:25:22Z                          |
| flavor                              | 2C2G20GB (6)                                  |
| hostId                              |                                               |
| id                                  | 0b8bacf7-53d9-4a8a-8387-bf3d95ed69a1          |
| image                               | cirros (9fdae471-668e-4341-b85c-5aa3a8e2db0d) |
| key_name                            | None                                          |
| name                                | cirros                                        |
| progress                            | 0                                             |
| project_id                          | 57e7eb094ace4170ab2c4792c23ee6e7              |
| properties                          |                                               |
| security_groups                     | name='default'                                |
| status                              | BUILD                                         |
| updated                             | 2021-03-20T11:25:22Z                          |
| user_id                             | 7fd343b6c0c94407a8812dfc1185adac              |
| volumes_attached                    |                                               |
+-------------------------------------+-----------------------------------------------+
# 绑定浮动IP
[root@controller ~]# openstack server add floating ip cirros 192.168.200.69
[root@controller ~]# openstack server show cirros
+-------------------------------------+----------------------------------------------------------+
| Field                               | Value                                                    |
+-------------------------------------+----------------------------------------------------------+
| OS-DCF:diskConfig                   | MANUAL                                                   |
| OS-EXT-AZ:availability_zone         | nova                                                     |
| OS-EXT-SRV-ATTR:host                | compute                                                  |
| OS-EXT-SRV-ATTR:hypervisor_hostname | compute                                                  |
| OS-EXT-SRV-ATTR:instance_name       | instance-00000002                                        |
| OS-EXT-STS:power_state              | Running                                                  |
| OS-EXT-STS:task_state               | None                                                     |
| OS-EXT-STS:vm_state                 | active                                                   |
| OS-SRV-USG:launched_at              | 2021-03-20T11:25:33.000000                               |
| OS-SRV-USG:terminated_at            | None                                                     |
| accessIPv4                          |                                                          |
| accessIPv6                          |                                                          |
| addresses                           | internal=10.0.0.56, 192.168.200.69                       |
| config_drive                        |                                                          |
| created                             | 2021-03-20T11:25:22Z                                     |
| flavor                              | 2C2G20GB (6)                                             |
| hostId                              | ed7a5fa7c2b4136afa2ee109e92fd8704fecf39df16f2b6c4d46f11e |
| id                                  | 0b8bacf7-53d9-4a8a-8387-bf3d95ed69a1                     |
| image                               | cirros (9fdae471-668e-4341-b85c-5aa3a8e2db0d)            |
| key_name                            | None                                                     |
| name                                | cirros                                                   |
| progress                            | 0                                                        |
| project_id                          | 57e7eb094ace4170ab2c4792c23ee6e7                         |
| properties                          |                                                          |
| security_groups                     | name='default'                                           |
| status                              | ACTIVE                                                   |
| updated                             | 2021-03-20T11:25:33Z                                     |
| user_id                             | 7fd343b6c0c94407a8812dfc1185adac                         |
| volumes_attached                    |                                                          |
+-------------------------------------+----------------------------------------------------------+
```

#### 测试实例

![能够成功访问](http://pic.acdts.top/img/image-20210320114229619.png)

