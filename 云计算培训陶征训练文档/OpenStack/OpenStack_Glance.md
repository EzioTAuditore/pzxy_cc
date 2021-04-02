# OpenStack_Glance

## Glance 运维

​		Glance是OpenStack的镜像服务。

​		默认Glance镜像上传后在`/var/lib/glance/images`目录

### 镜像相关命令

``` shell
[root@controller ~]# openstack image --help
Command "image" matches:		
  image create				# 创建镜像
  image delete				# 删除镜像
  image list				# 镜像列表	
  image set					# 更新镜像
  image show				# 查看镜像信息
  image unset
```



### 镜像管理

### 创建镜像

​		创建一个镜像，需要提供镜像名、源文件、镜像格式等信息。

``` shell
[root@controller ~]# openstack image create --disk-format qcow2 --container-format bare --file /opt/openstack/images/cirros-0.3.4-x86_64-disk.img --public cirros
+------------------+------------------------------------------------------+
| Field            | Value                                                |
+------------------+------------------------------------------------------+
| checksum         | ee1eca47dc88f4879d8a229cc70a07c6                     |
| container_format | bare                                                 |
| created_at       | 2021-03-19T13:01:20Z                                 |
| disk_format      | qcow2                                                |
| file             | /v2/images/9fdae471-668e-4341-b85c-5aa3a8e2db0d/file |
| id               | 9fdae471-668e-4341-b85c-5aa3a8e2db0d                 |
| min_disk         | 0                                                    |
| min_ram          | 0                                                    |
| name             | cirros                                               |
| owner            | 57e7eb094ace4170ab2c4792c23ee6e7                     |
| protected        | False                                                |
| schema           | /v2/schemas/image                                    |
| size             | 13287936                                             |
| status           | active                                               |
| tags             |                                                      |
| updated_at       | 2021-03-19T13:01:20Z                                 |
| virtual_size     | None                                                 |
| visibility       | public                                               |
+------------------+------------------------------------------------------+
```

### 查看镜像列表

``` shell
[root@controller ~]# openstack image list
+--------------------------------------+--------+--------+
| ID                                   | Name   | Status |
+--------------------------------------+--------+--------+
| 9fdae471-668e-4341-b85c-5aa3a8e2db0d | cirros | active |
+--------------------------------------+--------+--------+
```

### 查看镜像

```shell
[root@controller ~]# openstack image show cirros
+------------------+------------------------------------------------------+
| Field            | Value                                                |
+------------------+------------------------------------------------------+
| checksum         | ee1eca47dc88f4879d8a229cc70a07c6                     |
| container_format | bare                                                 |
| created_at       | 2021-03-19T13:01:20Z                                 |
| disk_format      | qcow2                                                |
| file             | /v2/images/9fdae471-668e-4341-b85c-5aa3a8e2db0d/file |
| id               | 9fdae471-668e-4341-b85c-5aa3a8e2db0d                 |
| min_disk         | 0                                                    |
| min_ram          | 0                                                    |
| name             | cirros                                               |
| owner            | 57e7eb094ace4170ab2c4792c23ee6e7                     |
| protected        | False                                                |
| schema           | /v2/schemas/image                                    |
| size             | 13287936                                             |
| status           | active                                               |
| tags             |                                                      |
| updated_at       | 2021-03-19T13:01:20Z                                 |
| virtual_size     | None                                                 |
| visibility       | public                                               |
+------------------+------------------------------------------------------+
```

### 删除镜像

```shell
# 上传镜像
[root@controller ~]# openstack image create --disk-format qcow2 --container-format bare --file /opt/openstack/images/cirros-0.3.4-x86_64-disk.img --public cirros_2
+------------------+------------------------------------------------------+
| Field            | Value                                                |
+------------------+------------------------------------------------------+
| checksum         | ee1eca47dc88f4879d8a229cc70a07c6                     |
| container_format | bare                                                 |
| created_at       | 2021-03-19T13:05:11Z                                 |
| disk_format      | qcow2                                                |
| file             | /v2/images/522d0fd7-5fc8-402f-91f5-46daf94e65cd/file |
| id               | 522d0fd7-5fc8-402f-91f5-46daf94e65cd                 |
| min_disk         | 0                                                    |
| min_ram          | 0                                                    |
| name             | cirros_2                                             |
| owner            | 57e7eb094ace4170ab2c4792c23ee6e7                     |
| protected        | False                                                |
| schema           | /v2/schemas/image                                    |
| size             | 13287936                                             |
| status           | active                                               |
| tags             |                                                      |
| updated_at       | 2021-03-19T13:05:11Z                                 |
| virtual_size     | None                                                 |
| visibility       | public                                               |
+------------------+------------------------------------------------------+
# 删除镜像
[root@controller ~]# openstack image delete cirros_2
# 查看列表，查不到cirros_2
[root@controller ~]# openstack image list
+--------------------------------------+--------+--------+
| ID                                   | Name   | Status |
+--------------------------------------+--------+--------+
| 9fdae471-668e-4341-b85c-5aa3a8e2db0d | cirros | active |
+--------------------------------------+--------+--------+
```

### 镜像运维

### 修改镜像元数据

​		使用`openstack image set `命令来修改镜像的元数据

``` shell
[root@controller ~]# openstack image set --min-disk 1 cirros	# 修改最小启动硬盘1GB
[root@controller ~]# openstack image show cirros
+------------------+------------------------------------------------------+
| Field            | Value                                                |
+------------------+------------------------------------------------------+
| checksum         | ee1eca47dc88f4879d8a229cc70a07c6                     |
| container_format | bare                                                 |
| created_at       | 2021-03-19T13:01:20Z                                 |
| disk_format      | qcow2                                                |
| file             | /v2/images/9fdae471-668e-4341-b85c-5aa3a8e2db0d/file |
| id               | 9fdae471-668e-4341-b85c-5aa3a8e2db0d                 |
| min_disk         | 1     # 👈 修改成功了                                  |
| min_ram          | 0                                                    |
| name             | cirros                                               |
| owner            | 57e7eb094ace4170ab2c4792c23ee6e7                     |
| protected        | False                                                |
| schema           | /v2/schemas/image                                    |
| size             | 13287936                                             |
| status           | active                                               |
| tags             |                                                      |
| updated_at       | 2021-03-19T13:09:35Z                                 |
| virtual_size     | None                                                 |
| visibility       | public                                               |
+------------------+------------------------------------------------------+
```

## 