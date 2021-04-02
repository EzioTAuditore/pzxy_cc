# OpenStack_Nova

## Nova运维

### 安全组运维

#### 常用安全组命令

``` shell
[root@controller ~]# openstack security group --help
Command "security" matches:
  security group create			# 创建安全组
  security group delete			# 删除安全组
  security group list			# 安全组列表
  security group rule create	# 创建规则
  security group rule delete	# 删除规则
  security group rule list		# 安全组规则列表
  security group rule show		# 查看安全组规则
  security group set			# 设置安全组
  security group show			# 查看安全组
```

#### 创建安全组

​		**仓用参数补充：**

| 参数        | 功能     | 用法 |
| ----------- | -------- | ---- |
| description | 描述信息 |      |

​		创建安全组需要提供安全组名，描述信息等。

``` shell
[root@controller ~]# openstack security group create test	# --description 添加描述信息
+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field           | Value                                                                                                                                                 |
+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
| created_at      | 2021-03-19T14:20:14Z                                                                                                                                  |
| description     | test                                                                                                                                                  |
| id              | 61d04167-b5ad-4037-b10d-51d162cab596                                                                                                                  |
| name            | test                                                                                                                                                  |
| project_id      | 57e7eb094ace4170ab2c4792c23ee6e7                                                                                                                      |
| revision_number | 2                                                                                                                                                     |
| rules           | created_at='2021-03-19T14:20:15Z', direction='egress', ethertype='IPv4', id='0fab269e-b013-42b4-b1bb-e337128fdacf', updated_at='2021-03-19T14:20:15Z' |
|                 | created_at='2021-03-19T14:20:15Z', direction='egress', ethertype='IPv6', id='4f0dbc20-a41c-4bc8-a8e8-2e6c9d773bbe', updated_at='2021-03-19T14:20:15Z' |
| updated_at      | 2021-03-19T14:20:15Z                                                                                                                                  |
+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
```

#### 查看安全组列表

``` shell
[root@controller ~]# openstack security group list
+--------------------------------------+---------+------------------------+----------------------------------+
| ID                                   | Name    | Description            | Project                          |
+--------------------------------------+---------+------------------------+----------------------------------+
| 3c016803-bf4c-4783-add6-c0e5027deade | default | Default security group | 57e7eb094ace4170ab2c4792c23ee6e7 |
| 61d04167-b5ad-4037-b10d-51d162cab596 | test    | test                   | 57e7eb094ace4170ab2c4792c23ee6e7 |
+--------------------------------------+---------+------------------------+----------------------------------+
```

#### 查看安全组

``` shell
[root@controller ~]# openstack security group show test
+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field           | Value                                                                                                                                                 |
+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
| created_at      | 2021-03-19T14:20:14Z                                                                                                                                  |
| description     | test                                                                                                                                                  |
| id              | 61d04167-b5ad-4037-b10d-51d162cab596                                                                                                                  |
| name            | test                                                                                                                                                  |
| project_id      | 57e7eb094ace4170ab2c4792c23ee6e7                                                                                                                      |
| revision_number | 2                                                                                                                                                     |
| rules           | created_at='2021-03-19T14:20:15Z', direction='egress', ethertype='IPv4', id='0fab269e-b013-42b4-b1bb-e337128fdacf', updated_at='2021-03-19T14:20:15Z' |
|                 | created_at='2021-03-19T14:20:15Z', direction='egress', ethertype='IPv6', id='4f0dbc20-a41c-4bc8-a8e8-2e6c9d773bbe', updated_at='2021-03-19T14:20:15Z' |
| updated_at      | 2021-03-19T14:20:15Z                                                                                                                                  |
+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
```

#### 创建安全组规则

​			**指令参数补充：**

| 参数         | 功能                                   | 用法                                                   |
| ------------ | -------------------------------------- | ------------------------------------------------------ |
| protocol     | 协议类型                               | --protocol TCP\|UDP\|ICMP                              |
| ingress      | 入口                                   |                                                        |
| egress       | 出口                                   |                                                        |
| dst-port     | 端口                                   | --dst-port 本机端口:远程端口                           |
| remote-group | 远程端口组，允许来自其他安全组的IP访问 | --remote-group 远程安全组名                            |
| romote-ip    | 远程IP地址                             | 0.0.0.0/0 全部通过<br />--romote-ip IP地址（CIDR写法） |

``` shell
# 允许所有ICMP通行
[root@controller ~]# openstack security group rule create --protocol icmp test
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| created_at        | 2021-03-19T14:57:02Z                 |
| description       |                                      |
| direction         | ingress                              |
| ether_type        | IPv4                                 |
| id                | 965b8a19-2ca4-4011-9e4e-93303ddf1a49 |
| name              | None                                 |
| port_range_max    | None                                 |
| port_range_min    | None                                 |
| project_id        | 57e7eb094ace4170ab2c4792c23ee6e7     |
| protocol          | icmp                                 |
| remote_group_id   | None                                 |
| remote_ip_prefix  | 0.0.0.0/0                            |
| revision_number   | 0                                    |
| security_group_id | 61d04167-b5ad-4037-b10d-51d162cab596 |
| updated_at        | 2021-03-19T14:57:02Z                 |
+-------------------+--------------------------------------+
```

#### 查看安全组规则列表

``` shell
[root@controller ~]# openstack security group rule list test
+--------------------------------------+-------------+-----------+------------+-----------------------+
| ID                                   | IP Protocol | IP Range  | Port Range | Remote Security Group |
+--------------------------------------+-------------+-----------+------------+-----------------------+
| 0fab269e-b013-42b4-b1bb-e337128fdacf | None        | None      |            | None                  |
| 4f0dbc20-a41c-4bc8-a8e8-2e6c9d773bbe | None        | None      |            | None                  |
| 965b8a19-2ca4-4011-9e4e-93303ddf1a49 | icmp        | 0.0.0.0/0 |            | None                  |
+--------------------------------------+-------------+-----------+------------+-----------------------+
```

#### 查看安全组规则

``` shell
[root@controller ~]# openstack security group rule show 965b8a19-2ca4-4011-9e4e-93303ddf1a49
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| created_at        | 2021-03-19T14:57:02Z                 |
| description       |                                      |
| direction         | ingress                              |
| ether_type        | IPv4                                 |
| id                | 965b8a19-2ca4-4011-9e4e-93303ddf1a49 |
| name              | None                                 |
| port_range_max    | None                                 |
| port_range_min    | None                                 |
| project_id        | 57e7eb094ace4170ab2c4792c23ee6e7     |
| protocol          | icmp                                 |
| remote_group_id   | None                                 |
| remote_ip_prefix  | 0.0.0.0/0                            |
| revision_number   | 0                                    |
| security_group_id | 61d04167-b5ad-4037-b10d-51d162cab596 |
| updated_at        | 2021-03-19T14:57:02Z                 |
+-------------------+--------------------------------------+
```

### 实例类型运维

#### 实例类型常用命令

```shell
[root@controller ~]# openstack flavor --help
Command "flavor" matches:
  flavor create			# 实例类型创建
  flavor delete			# 实例类型删除
  flavor list			# 实例类型列表
  flavor set			# 实例类型设置
  flavor show			# 查看实例类型
  flavor unset
```

#### 创建实例类型

​		常用参数补充：

| 参数      | 功能                   | 用法 |
| --------- | ---------------------- | ---- |
| id        | 实例类型ID             |      |
| ram       | 内存大小（单位mb）     |      |
| vcpus     | 虚拟CPU数量            |      |
| disk      | 硬盘大小               |      |
| swap      | 交换空间大小（单位mb） |      |
| ephemeral | 临时空间               |      |

``` shell
# VCPU：2 RAM：2GB 硬盘大小：20GB ID：6 
[root@controller ~]# openstack flavor create --vcpu 1 --ram 2048 --disk 20 --id 6 2C2G20GB
+----------------------------+----------+
| Field                      | Value    |
+----------------------------+----------+
| OS-FLV-DISABLED:disabled   | False    |
| OS-FLV-EXT-DATA:ephemeral  | 0        |
| disk                       | 20       |
| id                         | 6        |
| name                       | 2C2G20GB |
| os-flavor-access:is_public | True     |
| properties                 |          |
| ram                        | 2048     |
| rxtx_factor                | 1.0      |
| swap                       |          |
| vcpus                      | 1        |
+----------------------------+----------+
```

#### 查看实例类型列表

``` shell
[root@controller ~]# openstack flavor list
+----+----------+------+------+-----------+-------+-----------+
| ID | Name     |  RAM | Disk | Ephemeral | VCPUs | Is Public |
+----+----------+------+------+-----------+-------+-----------+
| 6  | 2C2G20GB | 2048 |   20 |         0 |     1 | True      |
+----+----------+------+------+-----------+-------+-----------+
```

#### 查看实例类型

``` shell
[root@controller ~]# openstack flavor show 6
+----------------------------+----------+
| Field                      | Value    |
+----------------------------+----------+
| OS-FLV-DISABLED:disabled   | False    |
| OS-FLV-EXT-DATA:ephemeral  | 0        |
| access_project_ids         | None     |
| disk                       | 20       |
| id                         | 6        |
| name                       | 2C2G20GB |
| os-flavor-access:is_public | True     |
| properties                 |          |
| ram                        | 2048     |
| rxtx_factor                | 1.0      |
| swap                       |          |
| vcpus                      | 1        |
+----------------------------+----------+
```

### 实例运维

#### 实例管理常用命令

``` shell
[root@controller ~]# openstack server --help
Command "server" matches:
  server add fixed ip			# 添加固定IP
  server add floating ip		# 添加浮动IP
  server add network			# 添加网络
  server add port				# 添加端口
  server add security group		# 添加安全组
  server add volume				# 添加卷
  server create					# 创建实例
  server delete					# 删除实例
  server image create			# 使用镜像创建实例
  server list					# 查看实例列表
  server lock					# 锁定实例	
  server migrate		
  server pause					# 暂停实例
  server reboot					# 重启实例
  server rebuild				# 重建实例
  server remove fixed ip		# 删除固定IP
  server remove floating ip		# 删除浮动IP
  server remove network			# 删除网络
  server remove port			# 删除端口
  server remove security group	# 删除安全组
  server remove volume			# 删除卷
  server set					# 实例设置
  server show					# 查看实例
  server start					# 开启实例
  server stop					# 停止实例	
  server unlock					# 实例解锁
  server unpause				# 实例继续
```

#### 创建实例

​		创建实例参数补充：

| 参数    | 功能     | 用法 |
| ------- | -------- | ---- |
| flavor  | 实例类型 |      |
| image   | 镜像     |      |
| volume  | 卷       |      |
| network | 网络     |      |

``` shell
[root@controller ~]# openstack server create --flavor 6 --image cirros cirros
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
| adminPass                           | vfYEVAWo7toc                                  |
| config_drive                        |                                               |
| created                             | 2021-03-19T16:55:51Z                          |
| flavor                              | 2C2G20GB (6)                                  |
| hostId                              |                                               |
| id                                  | 5751f3e8-41cc-44dc-9c0f-80a433cf72e1          |
| image                               | cirros (9fdae471-668e-4341-b85c-5aa3a8e2db0d) |
| key_name                            | None                                          |
| name                                | cirros                                        |
| progress                            | 0                                             |
| project_id                          | 57e7eb094ace4170ab2c4792c23ee6e7              |
| properties                          |                                               |
| security_groups                     | name='default'                                |
| status                              | BUILD                                         |
| updated                             | 2021-03-19T16:55:51Z                          |
| user_id                             | 7fd343b6c0c94407a8812dfc1185adac              |
| volumes_attached                    |                                               |
+-------------------------------------+-----------------------------------------------+
```

#### 查看实例列表

``` shell
[root@controller ~]# openstack server list
+--------------------------------------+--------+--------+----------+--------+----------+
| ID                                   | Name   | Status | Networks | Image  | Flavor   |
+--------------------------------------+--------+--------+----------+--------+----------+
| 5751f3e8-41cc-44dc-9c0f-80a433cf72e1 | cirros | ACTIVE |          | cirros | 2C2G20GB |
+--------------------------------------+--------+--------+----------+--------+----------+
```

#### 查看实例

``` shell
[root@controller ~]# openstack server show cirros
+-------------------------------------+----------------------------------------------------------+
| Field                               | Value                                                    |
+-------------------------------------+----------------------------------------------------------+
| OS-DCF:diskConfig                   | MANUAL                                                   |
| OS-EXT-AZ:availability_zone         | nova                                                     |
| OS-EXT-SRV-ATTR:host                | compute                                                  |
| OS-EXT-SRV-ATTR:hypervisor_hostname | compute                                                  |
| OS-EXT-SRV-ATTR:instance_name       | instance-00000001                                        |
| OS-EXT-STS:power_state              | Running                                                  |
| OS-EXT-STS:task_state               | None                                                     |
| OS-EXT-STS:vm_state                 | active                                                   |
| OS-SRV-USG:launched_at              | 2021-03-19T16:55:58.000000                               |
| OS-SRV-USG:terminated_at            | None                                                     |
| accessIPv4                          |                                                          |
| accessIPv6                          |                                                          |
| addresses                           |                                                          |
| config_drive                        |                                                          |
| created                             | 2021-03-19T16:55:51Z                                     |
| flavor                              | 2C2G20GB (6)                                             |
| hostId                              | ed7a5fa7c2b4136afa2ee109e92fd8704fecf39df16f2b6c4d46f11e |
| id                                  | 5751f3e8-41cc-44dc-9c0f-80a433cf72e1                     |
| image                               | cirros (9fdae471-668e-4341-b85c-5aa3a8e2db0d)            |
| key_name                            | None                                                     |
| name                                | cirros                                                   |
| progress                            | 0                                                        |
| project_id                          | 57e7eb094ace4170ab2c4792c23ee6e7                         |
| properties                          |                                                          |
| status                              | ACTIVE                                                   |
| updated                             | 2021-03-19T16:55:58Z                                     |
| user_id                             | 7fd343b6c0c94407a8812dfc1185adac                         |
| volumes_attached                    |                                                          |
+-------------------------------------+----------------------------------------------------------+
```