# OpenStack_Neutron

## Neutron运维

### 网络运维

#### 网络运维常用命令

``` shell
[root@controller ~]# openstack network --help
Command "network" matches:
  network create				# 创建网络
  network delete				# 删除网络
  network list					# 查看网络列表
  network set					# 设置网络
  network show					# 查看网络
  network unset
```

#### 创建网络

​		**创建网络参数补充**

| 参数                      | 功能         | 用法                                                      |
| ------------------------- | ------------ | --------------------------------------------------------- |
| share                     | 共享         | --share：共享<br />--no-share：不共享                     |
| external                  | 外部网络     |                                                           |
| internal                  | 内部网络     |                                                           |
| provider-network-type     | 网络供应类型 | flat<br />geneve<br />gre<br />local<br />vlan<br />vxlan |
| provider-physical-netowrk | 物理网络名   |                                                           |

``` shell
# name:external	type:external provider network type:flat provider physical network:provider share:yes
[root@controller ~]# openstack network create --external --share --provider-network-type flat --provider-physical-network provider external
+---------------------------+--------------------------------------+
| Field                     | Value                                |
+---------------------------+--------------------------------------+
| admin_state_up            | UP                                   |
| availability_zone_hints   |                                      |
| availability_zones        |                                      |
| created_at                | 2021-03-20T09:08:38Z                 |
| description               |                                      |
| dns_domain                | None                                 |
| id                        | 8c7505aa-6770-436a-a109-9bd145beded1 |
| ipv4_address_scope        | None                                 |
| ipv6_address_scope        | None                                 |
| is_default                | False                                |
| is_vlan_transparent       | None                                 |
| mtu                       | 1500                                 |
| name                      | external                             |
| port_security_enabled     | True                                 |
| project_id                | 57e7eb094ace4170ab2c4792c23ee6e7     |
| provider:network_type     | flat                                 |
| provider:physical_network | provider                             |
| provider:segmentation_id  | None                                 |
| qos_policy_id             | None                                 |
| revision_number           | 5                                    |
| router:external           | External                             |
| segments                  | None                                 |
| shared                    | True                                 |
| status                    | ACTIVE                               |
| subnets                   |                                      |
| tags                      |                                      |
| updated_at                | 2021-03-20T09:08:39Z                 |
+---------------------------+--------------------------------------+
```

#### 查看网络列表

``` shell
[root@controller ~]# openstack network list
+--------------------------------------+----------+---------+
| ID                                   | Name     | Subnets |
+--------------------------------------+----------+---------+
| 8c7505aa-6770-436a-a109-9bd145beded1 | external |         |
+--------------------------------------+----------+---------+
```

#### 查看网络

``` shell
[root@controller ~]# openstack network show external
+---------------------------+--------------------------------------+
| Field                     | Value                                |
+---------------------------+--------------------------------------+
| admin_state_up            | UP                                   |
| availability_zone_hints   |                                      |
| availability_zones        |                                      |
| created_at                | 2021-03-20T09:08:38Z                 |
| description               |                                      |
| dns_domain                | None                                 |
| id                        | 8c7505aa-6770-436a-a109-9bd145beded1 |
| ipv4_address_scope        | None                                 |
| ipv6_address_scope        | None                                 |
| is_default                | False                                |
| is_vlan_transparent       | None                                 |
| mtu                       | 1500                                 |
| name                      | external                             |
| port_security_enabled     | True                                 |
| project_id                | 57e7eb094ace4170ab2c4792c23ee6e7     |
| provider:network_type     | flat                                 |
| provider:physical_network | provider                             |
| provider:segmentation_id  | None                                 |
| qos_policy_id             | None                                 |
| revision_number           | 5                                    |
| router:external           | External                             |
| segments                  | None                                 |
| shared                    | True                                 |
| status                    | ACTIVE                               |
| subnets                   |                                      |
| tags                      |                                      |
| updated_at                | 2021-03-20T09:08:39Z                 |
+---------------------------+--------------------------------------+
```

### 子网运维

#### 子网运维常用命令

``` shell
[root@controller ~]# openstack subnet --help
Command "subnet" matches:
  subnet create				# 创建子网
  subnet delete				# 删除子网
  subnet list				# 子网列表
  subnet set				# 设置子网
  subnet show				# 查看子网
```

####  创建子网

​		**创建子网参数补充：**

| 参数            | 功能         | 用法                                                   |
| --------------- | ------------ | ------------------------------------------------------ |
| network         | 指定一个网络 |                                                        |
| subnet-range    | 网段         |                                                        |
| gateway         | 网关         |                                                        |
| ip-version      | IP版本       | IPV4：4<br />IPV6：6                                   |
| dhcp            | DHCP服务     |                                                        |
| allocation-pool | DHCP池       | --allocation-pool start=开始IP,end=结束IP              |
| dns-nameserver  | DNS服务器    | --dns-nameserver <dns-nameserver>                      |
| host-route      | 主机路由     | --host-route destination=<subnet>,gateway=<ip-address> |

``` shell
# name:ext-subnet network:external subnet-range:192.168.200.0/24 gateway:192.168.200.1 DHCP:yes DHCP池：192.168.200.50-192.168.200.200 DNS:114.114.114.114
[root@controller ~]# openstack subnet create \
> --network external \
> --subnet-range 192.168.200.0/24 \
> --gateway 192.168.200.1 \
> --dhcp --allocation-pool start=192.168.200.50,end=192.168.200.200 \
> --dns-nameserver 114.114.114.114 \
> ext-subnet
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| allocation_pools  | 192.168.200.50-192.168.200.200       |
| cidr              | 192.168.200.0/24                     |
| created_at        | 2021-03-20T10:35:51Z                 |
| description       |                                      |
| dns_nameservers   | 114.114.114.114                      |
| enable_dhcp       | True                                 |
| gateway_ip        | 192.168.200.1                        |
| host_routes       |                                      |
| id                | 87081291-9dcd-4be4-9ba0-88b820ef19a5 |
| ip_version        | 4                                    |
| ipv6_address_mode | None                                 |
| ipv6_ra_mode      | None                                 |
| name              | ext-subnet                           |
| network_id        | 8c7505aa-6770-436a-a109-9bd145beded1 |
| project_id        | 57e7eb094ace4170ab2c4792c23ee6e7     |
| revision_number   | 0                                    |
| segment_id        | None                                 |
| service_types     |                                      |
| subnetpool_id     | None                                 |
| tags              |                                      |
| updated_at        | 2021-03-20T10:35:51Z                 |
+-------------------+--------------------------------------+
```

#### 查看子网列表

``` shell
[root@controller ~]# openstack subnet list
+--------------------------------------+------------+--------------------------------------+------------------+
| ID                                   | Name       | Network                              | Subnet           |
+--------------------------------------+------------+--------------------------------------+------------------+
| 87081291-9dcd-4be4-9ba0-88b820ef19a5 | ext-subnet | 8c7505aa-6770-436a-a109-9bd145beded1 | 192.168.200.0/24 |
+--------------------------------------+------------+--------------------------------------+------------------+
```

#### 查看子网

``` shell
[root@controller ~]# openstack subnet show ext-subnet
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| allocation_pools  | 192.168.200.50-192.168.200.200       |
| cidr              | 192.168.200.0/24                     |
| created_at        | 2021-03-20T10:35:51Z                 |
| description       |                                      |
| dns_nameservers   | 114.114.114.114                      |
| enable_dhcp       | True                                 |
| gateway_ip        | 192.168.200.1                        |
| host_routes       |                                      |
| id                | 87081291-9dcd-4be4-9ba0-88b820ef19a5 |
| ip_version        | 4                                    |
| ipv6_address_mode | None                                 |
| ipv6_ra_mode      | None                                 |
| name              | ext-subnet                           |
| network_id        | 8c7505aa-6770-436a-a109-9bd145beded1 |
| project_id        | 57e7eb094ace4170ab2c4792c23ee6e7     |
| revision_number   | 0                                    |
| segment_id        | None                                 |
| service_types     |                                      |
| subnetpool_id     | None                                 |
| tags              |                                      |
| updated_at        | 2021-03-20T10:35:51Z                 |
+-------------------+--------------------------------------+
```

​		**创建子网参数补充**

| 参数 | 功能 | 用法 |
| ---- | ---- | ---- |
|      |      |      |

### 路由运维

#### 路由运维常用命令

``` shell
[root@controller ~]# openstack route --help
Command "route" matches:
  router add port			# 添加端口到路由
  router add subnet			# 添加子网到路由
  router create				# 创建路由
  router delete				# 删除路由
  router list				# 查看路由列表
  router remove port		# 删除路由端口
  router remove subnet		# 删除路由子网
  router set				# 设置路由
  router show				# 查看路由
```

#### 创建路由

``` shell
# name ext-router
[root@controller ~]# openstack router create ext-router
+-------------------------+--------------------------------------+
| Field                   | Value                                |
+-------------------------+--------------------------------------+
| admin_state_up          | UP                                   |
| availability_zone_hints |                                      |
| availability_zones      |                                      |
| created_at              | 2021-03-20T10:49:05Z                 |
| description             |                                      |
| distributed             | False                                |
| external_gateway_info   | None                                 |
| flavor_id               | None                                 |
| ha                      | False                                |
| id                      | 1a863f4f-3c45-44bb-b21d-ad548c620320 |
| name                    | ext-router                           |
| project_id              | 57e7eb094ace4170ab2c4792c23ee6e7     |
| revision_number         | 0                                    |
| routes                  |                                      |
| status                  | ACTIVE                               |
| tags                    |                                      |
| updated_at              | 2021-03-20T10:49:05Z                 |
+-------------------------+--------------------------------------+
```

#### 将路由器连接到外网

``` shell
# 外部网络：external
[root@controller ~]# openstack router set --external-gateway external ext-router
[root@controller ~]# openstack router show ext-router
+-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field                   | Value                                                                                                                                                                                      |
+-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| admin_state_up          | UP                                                                                                                                                                                         |
| availability_zone_hints |                                                                                                                                                                                            |
| availability_zones      | nova                                                                                                                                                                                       |
| created_at              | 2021-03-20T10:49:05Z                                                                                                                                                                       |
| description             |                                                                                                                                                                                            |
| distributed             | False                                                                                                                                                                                      |
| external_gateway_info   | {"network_id": "8c7505aa-6770-436a-a109-9bd145beded1", "enable_snat": true, "external_fixed_ips": [{"subnet_id": "87081291-9dcd-4be4-9ba0-88b820ef19a5", "ip_address": "192.168.200.51"}]} |
| flavor_id               | None                                                                                                                                                                                       |
| ha                      | False                                                                                                                                                                                      |
| id                      | 1a863f4f-3c45-44bb-b21d-ad548c620320                                                                                                                                                       |
| interfaces_info         | []                                                                                                                                                                                         |
| name                    | ext-router                                                                                                                                                                                 |
| project_id              | 57e7eb094ace4170ab2c4792c23ee6e7                                                                                                                                                           |
| revision_number         | 2                                                                                                                                                                                          |
| routes                  |                                                                                                                                                                                            |
| status                  | ACTIVE                                                                                                                                                                                     |
| tags                    |                                                                                                                                                                                            |
| updated_at              | 2021-03-20T10:51:58Z                                                                                                                                                                       |
+-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

#### 将路由器连接到子网

``` shell
# 子网：int-subnet
[root@controller ~]# openstack router add subnet ext-router int-subnet
[root@controller ~]# openstack router show ext-router
+-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field                   | Value                                                                                                                                                                                      |
+-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| admin_state_up          | UP                                                                                                                                                                                         |
| availability_zone_hints |                                                                                                                                                                                            |
| availability_zones      | nova                                                                                                                                                                                       |
| created_at              | 2021-03-20T10:49:05Z                                                                                                                                                                       |
| description             |                                                                                                                                                                                            |
| distributed             | False                                                                                                                                                                                      |
| external_gateway_info   | {"network_id": "8c7505aa-6770-436a-a109-9bd145beded1", "enable_snat": true, "external_fixed_ips": [{"subnet_id": "87081291-9dcd-4be4-9ba0-88b820ef19a5", "ip_address": "192.168.200.51"}]} |
| flavor_id               | None                                                                                                                                                                                       |
| ha                      | False                                                                                                                                                                                      |
| id                      | 1a863f4f-3c45-44bb-b21d-ad548c620320                                                                                                                                                       |
| interfaces_info         | [{"subnet_id": "54c58387-2878-4b06-8820-0c3335f8094b", "ip_address": "10.0.0.1", "port_id": "bf914d2b-9d4e-473b-b4f1-cb953f0905e9"}]                                                       |
| name                    | ext-router                                                                                                                                                                                 |
| project_id              | 57e7eb094ace4170ab2c4792c23ee6e7                                                                                                                                                           |
| revision_number         | 3                                                                                                                                                                                          |
| routes                  |                                                                                                                                                                                            |
| status                  | ACTIVE                                                                                                                                                                                     |
| tags                    |                                                                                                                                                                                            |
| updated_at              | 2021-03-20T10:54:52Z                                                                                                                                                                       |
+-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

### 浮动IP运维

#### 浮动IP运维常用命令

``` shell
[root@controller ~]# openstack floating ip --help
Command "floating" matches:
  floating ip create			# 创建浮动IP
  floating ip delete			# 删除浮动IP
  floating ip list				# 查看浮动IP列表
  floating ip pool list			# 浮动IP池列表
  floating ip set				# 设置浮动IP
  floating ip show				# 查看浮动IP
```

#### 创建浮动IP

``` shell
# 网络：external
[root@controller ~]# openstack floating ip create external
+---------------------+--------------------------------------+
| Field               | Value                                |
+---------------------+--------------------------------------+
| created_at          | 2021-03-20T11:19:45Z                 |
| description         |                                      |
| fixed_ip_address    | None                                 |
| floating_ip_address | 192.168.200.69                       |
| floating_network_id | 8c7505aa-6770-436a-a109-9bd145beded1 |
| id                  | a4d42a58-a1f3-4669-9d1b-a4aebd4f9096 |
| name                | 192.168.200.69                       |
| port_id             | None                                 |
| project_id          | 57e7eb094ace4170ab2c4792c23ee6e7     |
| qos_policy_id       | None                                 |
| revision_number     | 0                                    |
| router_id           | None                                 |
| status              | DOWN                                 |
| subnet_id           | None                                 |
| updated_at          | 2021-03-20T11:19:45Z                 |
+---------------------+--------------------------------------+
```

#### 查看浮动IP列表

```shell
[root@controller ~]# openstack floating ip list
+--------------------------------------+---------------------+------------------+------+--------------------------------------+----------------------------------+
| ID                                   | Floating IP Address | Fixed IP Address | Port | Floating Network                     | Project                          |
+--------------------------------------+---------------------+------------------+------+--------------------------------------+----------------------------------+
| a4d42a58-a1f3-4669-9d1b-a4aebd4f9096 | 192.168.200.69      | None             | None | 8c7505aa-6770-436a-a109-9bd145beded1 | 57e7eb094ace4170ab2c4792c23ee6e7 |
+--------------------------------------+---------------------+------------------+------+--------------------------------------+----------------------------------+
```

