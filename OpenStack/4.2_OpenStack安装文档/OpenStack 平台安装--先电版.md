# OpenStack 平台安装--先电版

## 声明

​		**本文档是配合2021年4月2日录制视频使用！**

​		视频文件请到QQ群下载观看。

## 检查系统配置

1. 网络配置

   1. 网卡名
   2. IP地址
   3. 能否访问

   * controller节点

     ![image-20210402140223591](http://pic.acdts.top/img/image-20210402140223591.png)

   * compute

     ![image-20210402140328930](http://pic.acdts.top/img/image-20210402140328930.png)

2. 计算机名

   1. controller节点

      ![image-20210402140004889](http://pic.acdts.top/img/image-20210402140004889.png)

   2. compute节点

   ![image-20210402140053218](http://pic.acdts.top/img/image-20210402140053218.png)

3. hosts文件

   * 未配置

4. 防火墙开启状态

   ![image-20210402140455796](http://pic.acdts.top/img/image-20210402140455796.png)

5. 仓库状态（yum）

   * 未配置

## 配置系统基础内容

1. 远程连接服务器
2. 配置`hosts`文件
3. 关闭防火墙
4. 配置yum仓库
   1. controller节点本地仓库
   2. compute节点远程仓库
5. 硬盘分区

### 配置hosts文件

1. 远程连接

   ![image-20210402140921257](http://pic.acdts.top/img/image-20210402140921257.png)

2. 添加个主机IP信息到`hosts`文件

   ![image-20210402141104579](http://pic.acdts.top/img/image-20210402141104579.png)

   ​	

   ```shell
   [root@controller ~]# cat << EOF >> /etc/hosts
   > 192.168.100.10 controller
   > 192.168.100.20 compute
   > EOF
   [root@controller ~]# cat /etc/hosts
   127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
   ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
   192.168.100.10 controller		# 添加成功
   192.168.100.20 compute			# 添加成功
   ```

### 关闭防火墙

1. 关闭`firewalld`防火墙服务

   ![image-20210402141454480](http://pic.acdts.top/img/image-20210402141454480.png)

   ```shell
   [root@controller ~]# systemctl status firewalld
   ● firewalld.service - firewalld - dynamic firewall daemon
      Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
      Active: active (running) since Fri 2021-04-02 21:59:01 CST; 14min ago	# 👈 防火墙处于开启状态
        Docs: man:firewalld(1)
    Main PID: 720 (firewalld)
      CGroup: /system.slice/firewalld.service
              └─720 /usr/bin/python -Es /usr/sbin/firewalld --nofork --no...
   
   Apr 02 21:59:00 controller systemd[1]: Starting firewalld - dynamic f....
   Apr 02 21:59:01 controller systemd[1]: Started firewalld - dynamic fi....
   Hint: Some lines were ellipsized, use -l to show in full.
   [root@controller ~]# systemctl stop firewalld && systemctl disable firewalld	# 先关闭防火墙，然后关闭开机自启
   Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
   Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
   [root@controller ~]# systemctl status firewalld
   ● firewalld.service - firewalld - dynamic firewall daemon
      Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
      Active: inactive (dead)	# 👈防火墙关闭
        Docs: man:firewalld(1)
   
   Apr 02 21:59:00 controller systemd[1]: Starting firewalld - dynamic f....
   Apr 02 21:59:01 controller systemd[1]: Started firewalld - dynamic fi....
   Apr 02 22:13:54 controller systemd[1]: Stopping firewalld - dynamic f....
   Apr 02 22:13:55 controller systemd[1]: Stopped firewalld - dynamic fi....
   Hint: Some lines were ellipsized, use -l to show in full.
   [root@controller ~]# 
   
   ```

2. 关闭`selinux`安全服务

   ![image-20210402141815328](http://pic.acdts.top/img/image-20210402141815328.png)

   ```shell
   [root@controller ~]# setenforce 0	# 临时关闭
   [root@controller ~]# getenforce 
   Permissive
   [root@controller ~]# sed -i -e "s/=enforcing/=permissive/" /etc/selinux/config 		# 永久关闭
   [root@controller ~]# cat /etc/selinux/config 
   
   # This file controls the state of SELinux on the system.
   # SELINUX= can take one of these three values:
   #     enforcing - SELinux security policy is enforced.
   #     permissive - SELinux prints warnings instead of enforcing.
   #     disabled - No SELinux policy is loaded.
   SELINUX=permissive		# 👈 修改这里
   # SELINUXTYPE= can take one of three two values:
   #     targeted - Targeted processes are protected,
   #     minimum - Modification of targeted policy. Only selected processes are protected. 
   #     mls - Multi Level Security protection.
   SELINUXTYPE=targeted 
   ```

### 配置yum仓库

1. 上传镜像文件到服务器

   1. 使用SFTP上传镜像文件到controller节点

      ![image-20210402142135692](http://pic.acdts.top/img/image-20210402142135692.png)

2. 挂在镜像文件，复制到指定地点

   1. 创建文件夹：centos，openstack

      ![image-20210402142305560](http://pic.acdts.top/img/image-20210402142305560.png)

   2. 挂在镜像文件并复制

      1. centos镜像

      ```shell
      mount -o loop ./CentOS-7-x86_64-DVD-1804.iso /mnt/		# 挂在到临时文件夹/mnt
      cp -rvf /mnt/* /opt/centos/								# 复制内容到/opt/centos
      umount /mnt/											# 解除文件挂载
      ```

      ![image-20210402142742093](http://pic.acdts.top/img/image-20210402142742093.png)

      2. iaas镜像文件

      ![image-20210402142941812](http://pic.acdts.top/img/image-20210402142941812.png)

      ![image-20210402143055764](http://pic.acdts.top/img/image-20210402143055764.png)

3. 编写yum文件

   1. controller节点

      ``` shell
      [root@controller ~]# mv /etc/yum.repos.d/* /media/
      [root@controller ~]# cat << EOF >> /etc/yum.repos.d/centos.repo
      > [centos]
      > name=centos
      > baseurl=file:///opt/centos
      > gpgcheck=0
      > enabled=1
      > 
      > [openstack]
      > name=openstack
      > baseurl=file:///opt/openstack/iaas-repo
      > gpgcheck=0
      > enable=1
      > EOF
      [root@controller ~]# cat /etc/yum.repos.d/centos.repo 
      [centos]
      name=centos
      baseurl=file:///opt/centos
      gpgcheck=0
      enabled=1
      
      [openstack]
      name=openstack
      baseurl=file:///opt/openstack/iaas-repo
      gpgcheck=0
      enable=1
      ```

   2. compute节点

      ``` shell
      [root@controller ~]# cp /etc/yum.repos.d/centos.repo /etc/yum.repos.d/ftp.repo	# 复制模板，改名字
      [root@controller ~]# vi /etc/yum.repos.d/ftp.repo 								# 编辑repo文件
      [root@controller ~]# cat /etc/yum.repos.d/ftp.repo 
      [centos]
      name=centos
      baseurl=ftp://192.168.100.10/centos		# 修改为ftp地址
      gpgcheck=0
      enabled=1
      
      [openstack]
      name=openstack
      baseurl=ftp://192.168.100.10/openstack/iaas-repo		# 修改为ftp地址
      gpgcheck=0
      enable=1
      [root@controller ~]# scp /etc/yum.repos.d/ftp.repo root@compute:/etc/yum.repos.d/	# 发送文件到远程服务器
      The authenticity of host 'compute (192.168.100.20)' can't be established.
      ECDSA key fingerprint is SHA256:zFoxX7kv4WYOnM0JLoIYETEtrg5CInDEQk88uHV1Y3M.
      ECDSA key fingerprint is MD5:4d:68:9f:37:25:c9:a5:73:aa:d9:fc:8f:8b:50:12:d0.
      Are you sure you want to continue connecting (yes/no)? yes
      Warning: Permanently added 'compute,192.168.100.20' (ECDSA) to the list of known hosts.
      root@compute's password: 
      ftp.repo  
      ```

4. 测试yum仓库

   1. 清空yum缓存

      ![image-20210402143913311](C:%5CUsers%5CEzioTA%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20210402143913311.png)

   2. 查看软件列表（可选）

   3. 配置远程仓库

      1. 安装`vsftpd`服务

         ![image-20210402144128691](http://pic.acdts.top/img/image-20210402144128691.png)

      2. 配置vsftpd，添加匿名用户访问地址到/opt目录

         ![image-20210402144304994](http://pic.acdts.top/img/image-20210402144304994.png)

         ![image-20210402144322037](http://pic.acdts.top/img/image-20210402144322037.png)

      3. 开启vsftpd服务，配置开机启动

         ![image-20210402144409426](http://pic.acdts.top/img/image-20210402144409426.png)

      4. 测试远程仓库

         ![image-20210402144449777](http://pic.acdts.top/img/image-20210402144449777.png)

### 硬盘分区

1. 检查分区表

   ![image-20210402144813754](http://pic.acdts.top/img/image-20210402144813754.png)

2. 进入分区工具

   ``` shell
   [root@compute yum.repos.d]# lsblk					# 查看硬盘分区
   NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda               8:0    0  100G  0 disk 
   ├─sda1            8:1    0    1G  0 part /boot
   └─sda2            8:2    0   50G  0 part 
     └─centos-root 253:0    0   50G  0 lvm  /
   sr0              11:0    1 1024M  0 rom  
   [root@compute yum.repos.d]# parted /dev/sda		# 使用分区工具进入硬盘
   GNU Parted 3.1
   Using /dev/sda		# 👈 当前使用硬盘
   Welcome to GNU Parted! Type 'help' to view a list of commands.
   (parted) p                                                                
   Model: VMware, VMware Virtual S (scsi)
   Disk /dev/sda: 107GB	# 👈 当前应硬盘总大小
   Sector size (logical/physical): 512B/512B
   Partition Table: msdos		# 👈 当前硬盘分区表类型
   Disk Flags: 
   #　👇　当前硬盘分区信息
   Number  Start   End     Size    Type     File system  Flags
    1      1049kB  1075MB  1074MB  primary  xfs          boot
    2      1075MB  54.8GB  53.7GB  primary               lvm
   
   (parted) mkpart　# 创建分区
   Partition type?  primary/extended? p		#　主分区                                      
   File system type?  [ext2]? xfs              # 分区格式                      
   Start? 55G                                   # 起始位置                     
   End? 80G                                     # 结束位置                    
   (parted) mkpart                                                           
   Partition type?  primary/extended? p                                      
   File system type?  [ext2]? xfs                                            
   Start? 80G
   End? 100G                                                                 
   (parted) p                                                                
   Model: VMware, VMware Virtual S (scsi)
   Disk /dev/sda: 107GB
   Sector size (logical/physical): 512B/512B
   Partition Table: msdos
   Disk Flags: 
   # 👇 分区后的分区表
   Number  Start   End     Size    Type     File system  Flags
    1      1049kB  1075MB  1074MB  primary  xfs          boot
    2      1075MB  54.8GB  53.7GB  primary               lvm
    3      54.8GB  80.0GB  25.2GB  primary
    4      80.0GB  100GB   20.0GB  primary
   
   (parted) q                                                                
   Information: You may need to update /etc/fstab.
   
   [root@compute yum.repos.d]# lsblk			# 查看分区状态
   NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda               8:0    0  100G  0 disk 
   ├─sda1            8:1    0    1G  0 part /boot
   ├─sda2            8:2    0   50G  0 part 
   │ └─centos-root 253:0    0   50G  0 lvm  /
   ├─sda3            8:3    0 23.5G  0 part 
   └─sda4            8:4    0 18.6G  0 part 
   sr0              11:0    1 1024M  0 rom 
   ```

3. 格式化分区

   ![image-20210402145431395](http://pic.acdts.top/img/image-20210402145431395.png)

**系统基础配置完成！可以进行iaas安装**

## 安装OpenStack

1. 安装前置环境
2. 配置系统参数
3. 使用脚本安装平台

### 安装前置环境

1. 安装iaas-xiandian软件包

   ![image-20210402145833486](http://pic.acdts.top/img/image-20210402145833486.png)

### 配置系统参数（修改openrc.sh文件）

``` shell
[root@controller ~]# sed -i -e "s/^#//" -e "s/PASS=/PASS=000000/" /etc/xiandian/openrc.sh 	# 删除所有的#号，填写默认密码为000000
[root@controller ~]# vi /etc/xiandian/openrc.sh 
[root@controller ~]# cat /etc/xiandian/openrc.sh 
#--------------------system Config--------------------##	
#Controller Server Manager IP. example:x.x.x.x
HOST_IP=192.168.100.10

#Controller HOST Password. example:000000 
HOST_PASS=000000

#Controller Server hostname. example:controller
HOST_NAME=controller

#Compute Node Manager IP. example:x.x.x.x
HOST_IP_NODE=192.168.100.20

#Compute HOST Password. example:000000 
HOST_PASS_NODE=000000

#Compute Node hostname. example:compute
HOST_NAME_NODE=compute

#--------------------Chrony Config-------------------##
#Controller network segment IP.  example:x.x.0.0/16(x.x.x.0/24)
network_segment_IP=192.168.100.0/24

#--------------------Rabbit Config ------------------##
#user for rabbit. example:openstack
RABBIT_USER=openstack

#Password for rabbit user .example:000000
RABBIT_PASS=000000

#--------------------MySQL Config---------------------##
#Password for MySQL root user . exmaple:000000
DB_PASS=000000

#--------------------Keystone Config------------------##
#Password for Keystore admin user. exmaple:000000
DOMAIN_NAME=demo
ADMIN_PASS=000000
DEMO_PASS=000000

#Password for Mysql keystore user. exmaple:000000
KEYSTONE_DBPASS=000000

#--------------------Glance Config--------------------##
#Password for Mysql glance user. exmaple:000000
GLANCE_DBPASS=000000

#Password for Keystore glance user. exmaple:000000
GLANCE_PASS=000000

#--------------------Nova Config----------------------##
#Password for Mysql nova user. exmaple:000000
NOVA_DBPASS=000000

#Password for Keystore nova user. exmaple:000000
NOVA_PASS=000000

#--------------------Neturon Config-------------------##
#Password for Mysql neutron user. exmaple:000000
NEUTRON_DBPASS=000000

#Password for Keystore neutron user. exmaple:000000
NEUTRON_PASS=000000

#metadata secret for neutron. exmaple:000000
METADATA_SECRET=000000

#Tunnel Network Interface. example:x.x.x.x
INTERFACE_IP=192.168.100.10				# 每个节点填写自己的IP地址

#External Network Interface. example:eth1
INTERFACE_NAME=eth1

#External Network The Physical Adapter. example:provider
Physical_NAME=provider

#First Vlan ID in VLAN RANGE for VLAN Network. exmaple:101
minvlan=101

#Last Vlan ID in VLAN RANGE for VLAN Network. example:200
maxvlan=200

#--------------------Cinder Config--------------------##
#Password for Mysql cinder user. exmaple:000000
CINDER_DBPASS=000000

#Password for Keystore cinder user. exmaple:000000
CINDER_PASS=000000

#Cinder Block Disk. example:md126p3
BLOCK_DISK=sda3

#--------------------Swift Config---------------------##
#Password for Keystore swift user. exmaple:000000
SWIFT_PASS=000000

#The NODE Object Disk for Swift. example:md126p4.
OBJECT_DISK=sda4

#The NODE IP for Swift Storage Network. example:x.x.x.x.
STORAGE_LOCAL_NET_IP=192.168.100.20

#--------------------Heat Config----------------------##
#Password for Mysql heat user. exmaple:000000
HEAT_DBPASS=000000

#Password for Keystore heat user. exmaple:000000
HEAT_PASS=000000

#--------------------Zun Config-----------------------##
#Password for Mysql Zun user. exmaple:000000
ZUN_DBPASS=000000

#Password for Keystore Zun user. exmaple:000000
ZUN_PASS=000000

#Password for Mysql Kuryr user. exmaple:000000
KURYR_DBPASS=000000

#Password for Keystore Kuryr user. exmaple:000000
KURYR_PASS=000000

#--------------------Ceilometer Config----------------##
#Password for Gnocchi ceilometer user. exmaple:000000
CEILOMETER_DBPASS=000000

#Password for Keystore ceilometer user. exmaple:000000
CEILOMETER_PASS=000000

#--------------------AODH Config----------------##
#Password for Mysql AODH user. exmaple:000000
AODH_DBPASS=000000

#Password for Keystore AODH user. exmaple:000000
AODH_PASS=000000

#--------------------Barbican Config----------------##
#Password for Mysql Barbican user. exmaple:000000
BARBICAN_DBPASS=000000

#Password for Keystore Barbican user. exmaple:000000
BARBICAN_PASS=000000
```

### 脚本安装OpenStack

1. 双节点安装pre服务并重启终端

   ![image-20210402150527121](http://pic.acdts.top/img/image-20210402150527121.png)

2. controller节点安装

   1. MySQL
   2. Keystone
   3. Glance
   4. Nova-controller
   5. Neutron-controller
   6. Dashboard
   7. Cinder-controller
   8. Swift-controller

   ![image-20210402150911309](http://pic.acdts.top/img/image-20210402150911309.png)

3. compute节点安装

   1. Nova-compute
   2. Neutron-compute
   3. Cinder-compute
   4. Swift-compute

   ![image-20210402151613184](http://pic.acdts.top/img/image-20210402151613184.png)

4. 添加controller节点用于计算

   1. 修改openrc文件

      ![image-20210402152152591](http://pic.acdts.top/img/image-20210402152152591.png)

   2. 运行安装脚本👆

      这里要输入controller节点密码：

      ![image-20210402152351318](http://pic.acdts.top/img/image-20210402152351318.png)

      ![image-20210402152424729](http://pic.acdts.top/img/image-20210402152424729.png)

      ![image-20210402152515693](http://pic.acdts.top/img/image-20210402152515693.png)

**安装完毕！**

