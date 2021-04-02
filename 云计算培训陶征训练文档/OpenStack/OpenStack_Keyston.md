# OpenStack_Keystone

## Keystone 运维

​		Keystone运维中，主要命令格式是`openstack user/project/role `，分别针对用户，项目，角色三种进行配置。

### 用户操作

#### 用户相关命令

``` shell
[root@controller ~]# openstack user --help
Command "user" matches:
  user create				# 创建用户
  user delete				# 删除用户
  user list					# 用户列表
  user password set			# 设置用户密码
  user set					# 用户设置
  user show					# 查看用户
```

#### 创建用户

​		创建用户需要提供用户名、用户邮箱、用户密码等信息。

``` shell
# 用户名：alice 邮箱：alice@example.com 密码：mypassword123
[root@controller ~]# source /etc/keystone/admin-openrc.sh 
[root@controller ~]# openstack user create --password mypassword123 --email alice@example.com --domain demo alice
+---------------------+----------------------------------+
| Field               | Value                            |
+---------------------+----------------------------------+
| domain_id           | cc777c4a1bfe451e8c62b0ce63e63f33 |
| email               | alice@example.com                |
| enabled             | True                             |
| id                  | f60e71b217d84b5bb3de17720e7de26e |
| name                | alice                            |
| options             | {}                               |
| password_expires_at | None                             |
+---------------------+----------------------------------+
```

#### 查看用户列表

``` shell
[root@controller ~]# openstack user list
+----------------------------------+-----------+
| ID                               | Name      |
+----------------------------------+-----------+
| 04fbde06d03e42ed8a2513eea8ad688b | placement |
| 56ddec3ece674649978cb642257eeae5 | nova      |
| 5776c2e7baa44fdfae66a583aeb54f14 | swift     |
| 685e3bd3514e490da7a60059816fc26f | glance    |
| 7fd343b6c0c94407a8812dfc1185adac | admin     |
| a2e5294126a94468afa63ed6b8617dc5 | neutron   |
| a5836dfc626d4c74884f297be239bd6d | demo      |
| e7876881efe142038946dcfefdafeb7c | cinder    |
| f60e71b217d84b5bb3de17720e7de26e | alice     |	# 👈这是我们的新用户
+----------------------------------+-----------+
```

#### 查看用户信息

``` shell
[root@controller ~]# openstack user show alice
+---------------------+----------------------------------+
| Field               | Value                            |
+---------------------+----------------------------------+
| domain_id           | cc777c4a1bfe451e8c62b0ce63e63f33 |
| email               | alice@example.com                |
| enabled             | True                             |
| id                  | f60e71b217d84b5bb3de17720e7de26e |
| name                | alice                            |
| options             | {}                               |
| password_expires_at | None                             |
+---------------------+----------------------------------+
```

### 项目操作

​		Project 就是一个项目、团队或组织，当请求OpenStack服务时，必须定义一个项目。

#### 项目相关命令

``` shell
[root@controller ~]# openstack project --help
Command "project" matches:
  project create		# 创建项目
  project delete		# 删除项目
  project list			# 项目列表
  project purge	
  project set			# 项目设置
  project show			# 查看项目
```

#### 创建项目

​		创建项目需要提供项目名等信息。

``` shell
# 项目名：acme
[root@controller ~]# openstack project create --domain demo acme
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description |                                  |
| domain_id   | cc777c4a1bfe451e8c62b0ce63e63f33 |
| enabled     | True                             |
| id          | e569c81f834c4485a07d70595d852251 |
| is_domain   | False                            |
| name        | acme                             |
| parent_id   | cc777c4a1bfe451e8c62b0ce63e63f33 |
| tags        | []                               |
+-------------+----------------------------------+
```

#### 查看项目列表

``` shell
[root@controller ~]# openstack project list
+----------------------------------+---------+
| ID                               | Name    |
+----------------------------------+---------+
| 00f8022a46484ea1bdbb65197701b6ec | demo    |
| 3b83e017dd82480688bf1b86a5df9f83 | service |
| 57e7eb094ace4170ab2c4792c23ee6e7 | admin   |
| e569c81f834c4485a07d70595d852251 | acme    |	# 👈这是我们创建的项目
+----------------------------------+---------+
```

#### 查看项目

``` shell
[root@controller ~]# openstack project show acme
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description |                                  |
| domain_id   | cc777c4a1bfe451e8c62b0ce63e63f33 |
| enabled     | True                             |
| id          | e569c81f834c4485a07d70595d852251 |
| is_domain   | False                            |
| name        | acme                             |
| parent_id   | cc777c4a1bfe451e8c62b0ce63e63f33 |
| tags        | []                               |
+-------------+----------------------------------+
```

### 角色操作

​		角色限定用户的操作权限。

#### 角色相关命令

``` shell
[root@controller ~]# openstack role --help
Command "role" matches:
  role add					# 绑定用户、项目与角色
  role assignment list		
  role create				# 创建角色
  role delete				# 删除角色
  role list					# 角色列表
  role remove				# 删除角色
  role set					# 设置角色
  role show					# 查看角色
```



#### 创建角色

​		创建角色需要提供角色名。创建角色不需要填写域信息。

``` shell
# 角色名：conpute-user
[root@controller ~]# openstack role create compute-user
+-----------+----------------------------------+
| Field     | Value                            |
+-----------+----------------------------------+
| domain_id | None                             |
| id        | d8d7a7ab4d5a430ebe4235e4cfe5018e |
| name      | compute-user                     |
+-----------+----------------------------------+
```

#### 查看角色列表

``` shell
[root@controller ~]# openstack role list
+----------------------------------+--------------+
| ID                               | Name         |
+----------------------------------+--------------+
| 107f1dc08dc443dea52fa18d4392516d | user         |
| b63910c31e7547adb2e3f1cd3620bdd3 | admin        |
| d8d7a7ab4d5a430ebe4235e4cfe5018e | compute-user |	# 👈 在这呢
+----------------------------------+--------------+
```

#### 查看角色

``` shell
[root@controller ~]# openstack role show compute-user
+-----------+----------------------------------+
| Field     | Value                            |
+-----------+----------------------------------+
| domain_id | None                             |
| id        | d8d7a7ab4d5a430ebe4235e4cfe5018e |
| name      | compute-user                     |
+-----------+----------------------------------+
```

#### 绑定用户和项目权限

​		如果要给用户分配一定的权限，就需要把用户关联绑定到对象的项目和角色。

``` shell
# 将alice分配到acme项目下的compute-user角色
[root@controller ~]# openstack role add --user alice --project acme compute-user
```

### 端点地址查询

​		查看OpenStack平台中所有服务端点信息。

``` shell
[root@controller ~]# openstack endpoint list
+----------------------------------+-----------+--------------+--------------+---------+-----------+----------------------------------------------+
| ID                               | Region    | Service Name | Service Type | Enabled | Interface | URL                                          |
+----------------------------------+-----------+--------------+--------------+---------+-----------+----------------------------------------------+
| 8e15c7e334b44de0bfd6d97743ffc8b2 | RegionOne | nova         | compute      | True    | internal  | http://controller:8774/v2.1                  |
| 9244739e7e6145b1ab38d31e7dd708c4 | RegionOne | glance       | image        | True    | admin     | http://controller:9292                       |
http://controller:8776/v3/%(tenant_id)s      |
| fda9420e56734308ace96ad29bf215f6 | RegionOne | nova         | compute      | True    | public    | http://controller:8774/v2.1                  |
+----------------------------------+-----------+--------------+--------------+---------+-----------+----------------------------------------------+

```

### Keystone运维

#### 查看Keystone相关进程

``` shell
[root@controller ~]# ps -ef | grep -i keystone
keystone   2228   2222  0 19:37 ?        00:00:06 (wsgi:keystone- -DFOREGROUND
keystone   2229   2222  0 19:37 ?        00:00:06 (wsgi:keystone- -DFOREGROUND
keystone   2230   2222  0 19:37 ?        00:00:07 (wsgi:keystone- -DFOREGROUND
keystone   2231   2222  0 19:37 ?        00:00:07 (wsgi:keystone- -DFOREGROUND
keystone   2232   2222  0 19:37 ?        00:00:08 (wsgi:keystone- -DFOREGROUND
keystone   2233   2222  0 19:37 ?        00:00:06 (wsgi:keystone- -DFOREGROUND
keystone   2234   2222  0 19:37 ?        00:00:06 (wsgi:keystone- -DFOREGROUND
keystone   2235   2222  0 19:37 ?        00:00:05 (wsgi:keystone- -DFOREGROUND
keystone   2236   2222  0 19:37 ?        00:00:07 (wsgi:keystone- -DFOREGROUND
keystone   2237   2222  0 19:37 ?        00:00:07 (wsgi:keystone- -DFOREGROUND
root      17883   3025  0 20:40 pts/0    00:00:00 grep --color=auto -i keystone
```

#### 查看Keytone错误日志

``` shell
[root@controller ~]# grep ERROR /var/log/keystone/keystone.log
```
