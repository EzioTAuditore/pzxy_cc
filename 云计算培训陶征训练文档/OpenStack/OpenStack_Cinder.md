# OpenStack_Cinder

## Cinder运维

### Volume运维

#### 常用Volume命令

``` shell
[root@controller ~]# openstack volume --help
Command "volume" matches:
  volume create				# Volume创建
  volume delete				# Volume删除
  volume list				#查看Volume列表
  volume set				# 设置Volume
  volume show				# 查看Volume
```

#### 创建Volume

​		**创建Volume参数补充**

| 参数  | 功能           | 用法           |
| ----- | -------------- | -------------- |
| size  | 大小（单位GB） |                |
| image | 镜像           | --image 镜像名 |

``` shell
# 大小：10GB 名字：10GB
[root@controller ~]# openstack volume create --size 10 10GB
+---------------------+--------------------------------------+
| Field               | Value                                |
+---------------------+--------------------------------------+
| attachments         | []                                   |
| availability_zone   | nova                                 |
| bootable            | false                                |
| consistencygroup_id | None                                 |
| created_at          | 2021-03-20T12:46:24.000000           |
| description         | None                                 |
| encrypted           | False                                |
| id                  | 86dd4fed-e1fa-4d27-bdca-ed6ec75a109f |
| migration_status    | None                                 |
| multiattach         | False                                |
| name                | 10GB                                 |
| properties          |                                      |
| replication_status  | None                                 |
| size                | 10                                   |
| snapshot_id         | None                                 |
| source_volid        | None                                 |
| status              | creating                             |
| type                | None                                 |
| updated_at          | None                                 |
| user_id             | 7fd343b6c0c94407a8812dfc1185adac     |
+---------------------+--------------------------------------+
```

#### 查看Volume列表

``` shell
[root@controller ~]# openstack volume list
+--------------------------------------+------+-----------+------+-------------+
| ID                                   | Name | Status    | Size | Attached to |
+--------------------------------------+------+-----------+------+-------------+
| 86dd4fed-e1fa-4d27-bdca-ed6ec75a109f | 10GB | available |   10 |             |
+--------------------------------------+------+-----------+------+-------------+
```

#### 查看Volume

``` shell
[root@controller ~]# openstack volume show 10GB
+--------------------------------+--------------------------------------+
| Field                          | Value                                |
+--------------------------------+--------------------------------------+
| attachments                    | []                                   |
| availability_zone              | nova                                 |
| bootable                       | false                                |
| consistencygroup_id            | None                                 |
| created_at                     | 2021-03-20T12:46:24.000000           |
| description                    | None                                 |
| encrypted                      | False                                |
| id                             | 86dd4fed-e1fa-4d27-bdca-ed6ec75a109f |
| migration_status               | None                                 |
| multiattach                    | False                                |
| name                           | 10GB                                 |
| os-vol-host-attr:host          | compute@lvm#LVM                      |
| os-vol-mig-status-attr:migstat | None                                 |
| os-vol-mig-status-attr:name_id | None                                 |
| os-vol-tenant-attr:tenant_id   | 57e7eb094ace4170ab2c4792c23ee6e7     |
| properties                     |                                      |
| replication_status             | None                                 |
| size                           | 10                                   |
| snapshot_id                    | None                                 |
| source_volid                   | None                                 |
| status                         | available                            |
| type                           | None                                 |
| updated_at                     | 2021-03-20T12:46:25.000000           |
| user_id                        | 7fd343b6c0c94407a8812dfc1185adac     |
+--------------------------------+--------------------------------------+
```

#### 将Volume添加到实例上

``` shell
# 将10GB的Volume添加到cirros实例的/10GB/路径
[root@controller ~]# openstack server add volume --device /dev/vdb cirros 86dd4fed-e1fa-4d27-bdca-ed6ec75a109f
```

![挂在成功](http://pic.acdts.top/img/image-20210320132323984.png)