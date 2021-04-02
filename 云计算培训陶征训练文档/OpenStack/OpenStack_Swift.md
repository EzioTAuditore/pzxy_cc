# OpenStack_Swift

## Swift运维

​		在Swift中,使用OpenStack命令来进行管理的话,对象存储需要先创建容器(container).

​		object命令是针对文件进行操作的

### 容器运维

#### 容器运维常用命令

``` shell
[root@controller ~]# openstack container --help
Command "container" matches:
  container create			# 创建容器
  container delete			#　删除容器
  container list			# 容器列表
  container show			#　查看容器
```

#### 创建容器

``` shell
[root@controller ~]# openstack container create test
+---------------------------------------+-----------+------------------------------------+
| account                               | container | x-trans-id                         |
+---------------------------------------+-----------+------------------------------------+
| AUTH_57e7eb094ace4170ab2c4792c23ee6e7 | test      | txf2205dcbfd86495e8bb2e-006055f5fe |
+---------------------------------------+-----------+------------------------------------+
```

#### 查看容器列表

```shell
[root@controller ~]# openstack container list
+------+
| Name |
+------+
| test |
+------+
```

#### 查看容器

```shell
[root@controller ~]# openstack container show test
+--------------+---------------------------------------+
| Field        | Value                                 |
+--------------+---------------------------------------+
| account      | AUTH_57e7eb094ace4170ab2c4792c23ee6e7 |
| bytes_used   | 0                                     |
| container    | test                                  |
| object_count | 0                                     |
+--------------+---------------------------------------+
```

### 对象存储运维

#### 对象存储运维常用命令

```shell
[root@controller ~]# openstack object --help
Command "object" matches:
  object create					# 上传文件
  object delete					# 删除文件
  object list					# 文件列表
  object show					# 查看文件
  object store account set		# 权限设置
  object store account show		# 查看权限
```

#### 上传文件

```shell
# 创建文件
[root@controller ~]# touch whatisthis
[root@controller ~]# openstack object create test whatisthis
+------------+-----------+----------------------------------+
| object     | container | etag                             |
+------------+-----------+----------------------------------+
| whatisthis | test      | d41d8cd98f00b204e9800998ecf8427e |
+------------+-----------+----------------------------------+
```

#### 查看文件列表

```shell
[root@controller ~]# openstack object list test
+------------+
| Name       |
+------------+
| whatisthis |
+------------+
```

