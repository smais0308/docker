


从CentOS7.X 开始使用systemd服务来代替daemon，原来管理系统启动和管理服务系统服务的相关命令全部由systemctl命令来替代。
>Unit  
>表示不同类型的sytemd对象，通过配置文件进行标识和配置，文件中主要包含了系统服务，监听socket、保存的系统快照以及其他与init相关的信息
>配置文件:  
>/usr/lib/systemd/system：每个服务最主要的启动脚本设置，类似于之前的/etc/initd.d  
>/run/system/system：系统执行过程中所产生的服务脚本，比上面的目录优先运行  
>/etc/system/system：管理员建立的执行脚本，类似于/etc/rc.d/rcN.d/Sxx类的功能，比上面目录优先运行，在三者之中，此目录优先级最高
>


### 1. 原来的service命令和systemctl命令对比

| daemon | systemctl | 说明 |
|---|---|---|
|service \[服务] start | systemctl start \[unit type]|启动服务|
|service \[服务] stop | systemctl stop \[unit type]|停止服务|
|service \[服务] restart | systemctl restart \[unit type]|重启服务|
- 还多了**2**个命令
	- status：来查看服务运行情况
	- reload：重新加载配置文件，并不是所有服务都支持，network.service

```shell
#启动网络服务
systemctl start network.service
 
#停止网络服务
systemctl stop network.service
 
#重启网络服务
systemctl restart network.service
 
#查看网络服务状态
systemctl status network.serivce
```

### 2. 原来的chkconfig命令与systemctl命令对比
设置开机启动/不启动

| daemon | systemctl | 说明 |
|---|---|---|
|chkconfig \[服务] on |systemctl enable \[unit type] |设置服务开机启动 |
|chkconfig \[服务] off |systemctl disable \[unit type] |禁止开机自动启动 |

应用举例
```shell
#停止cup电源管理服务
systemctl stop cups.service
 
#禁止cups服务开机启动
systemctl disable cups.service
 
#查看cups服务状态
systemctl status cups.service
 
#重新设置cups服务开机启动
systemctl enable cups.service

# Centos 7.x 中取消了iptables, 用firewall取而代之。要关闭防火墙并禁止开机启动服务使用下面的命令

systemctl stop firewalld.service
systemctl disable firewalld.service
```

### 3. 查看系统上所有服务
> 命令格式：
> systemctl [command] [–type=TYPE] [–all]
> 参数详解：
> command - list-units：依据unit列出所有启动的unit。加上 –all 才会列出没启动的unit; - list-unit-files:依据/usr/lib/systemd/system/ 内的启动文件，列出启动文件列表
> –type=TYPE - 为unit type, 主要有service, socket, target

- 应用举例

| systemctl | 说明 |
|---|---|
| systemctl | 列出所有系统服务 |
| systemctl list-units | 列出所有启动unti |
| systemctl list-unit-files | 列出所有启动文件 |
| systemctl list-unit-type=service-all | 列出所有service类型的unit |
| systemctl list-unit-type=service-all grep cpu | 列出cpu电源管理机制的服务 |
| systemctl list-unit-type=target-all | 列出所有target |


### 4. systemctl 特殊的用法
| systemctl | 说明 |
|---|---|
| systemctl is-active \[unit type ] | 查看服务是否运行 |
| systemctl is-enable \[unit type ] | 查看服务是否自启 |
| systemctl mask \[unit type ] | 注销指定服务 |
| systemctl unmask \[unit type ] | 取消注销指定服务 |

```shell
#查看网络服务是否启动
systemctl is-active network.service
 
#检查网络服务是否设置为开机启动
systemctl is-enabled network.service
 
#停止cups服务
systemctl stop cups.service
 
#注销cups服务
systemctl mask cups.service
 
#查看cups服务状态
systemctl status cups.service
 
#取消注销cups服务
systemctl unmask cups.service
```


**说明:systemctl mask和systemctl enable/disable的区别**
- systemctl enable会在/etc/systemed/system/目录下创建需要的符号连接，软连接实际指向的文件为/usr/lib/systemd/system/目录中的文件，实际启作用的也是这个目录中的文件。
- systemctl disable xxx 是将/etc/systemd/system中的软连接删除
- systemctl mask xxx 是创建一个指向/dev/null的符号链接，例如：
```shell
[root@NameNode01 system]# systemctl mask NetworkManager 
Created symlink from /etc/systemd/system/NetworkManager.service to /dev/null.
```

使用mask后无法使用start 启动服务，因为mask是创建了一个软连接，链接到/dev/null中
举例：
```shell
[root@NameNode01 system]# systemctl start NetworkManager
Failed to start NetworkManager.service: Unit is masked.
```

- 与开关机相关的其他命令：
```shell
#进入睡眠模式
systemctl suspend
 
#进入休眠模式
systemctl hibernate
 
#强制进入救援模式
systemctl rescue
 
#强制进入紧急救援模式
systemctl emergency
```

### 5. 设置系统运行级别
5.1 运行级别对应表

| init级别 | systemctl target |
|---|---|
| 0 | shutdown.target|
| 1 | emergency.target |
| 2 | rescure.target |
| 3 | multi-user.target |
| 4 | 无 |
| 5 | graphical.target |
| 6 | 无 |
此外还是一个getty.target用来设置tty的数量。
5.2 设置运行级别
```shell
#命令格式
systemctl [command] [unit.target]
#参数详解：
command:
get-default :取得当前的target
set-default :设置指定的target为默认的运行级别
isolate :切换到指定的运行级别
unit.target :为5.1表中列出的运行级别
```
| systemctl命令	| 说明 |
|---|---|
|systemctl get-default	|获得当前的运行级别|
|systemctl set-default multi-user.target	|设置默认的运行级别为mulit-user|
| systemctl isolate multi-user.target	|在不重启的情况下，切换到运行级别mulit-user下|
|systemctl isolate graphical.target	|在不重启的情况下，切换到图形界面下|
### 6. 使用systemctl分析各个服务之前的依赖关系
```shell
systemctl list-dependencies [unit] [–reverse]
# –reverse`是用来检查寻哪个unit使用了这个unit

#获得当前运行级别的target
[root@www ~]# systemctl get-default
multi-user.target
 
#查看当前运行级别target(mult-user)启动了哪些服务
[root@www ~]# systemctl list-dependencies
default.target
├─abrt-ccpp.service
├─abrt-oops.service
├─vsftpd.service
├─basic.target
│ ├─alsa-restore.service
│ ├─alsa-state.service
.....(中间省略).....
│ ├─sockets.target
│ │ ├─avahi-daemon.socket
│ │ ├─dbus.socket
.....(中间省略).....
│ ├─sysinit.target
│ │ ├─dev-hugepages.mount
│ │ ├─dev-mqueue.mount
.....(中间省略).....
│ └─timers.target
│   └─systemd-tmpfiles-clean.timer
├─getty.target
│ └─getty@tty1.service
└─remote-fs.target
 
#查看哪些target引用了当前运行级别的target
[root@www ~]# systemctl list-dependencies --reverse
default.target
└─graphical.target
```

### 7. 关闭网络服务
在使用systemctl关闭网络服务时有一些特殊 需要同时关闭unit.servce和unit.socket

使用systemctl查看开启的sshd服务
```bash
[root@www system]#  systemctl list-units --all | grep sshd
sshd-keygen.service loaded inactive dead        OpenSSH Server Key Generation
sshd.service        loaded active   running     OpenSSH server daemon
sshd.socket         loaded inactive dead        OpenSSH Server Socket
```

可以看到系统同时开启了 sshd.service 和 sshd.socket , 如果只闭关了 sshd.service 那么 sshd.socket还在监听网络，在网络上有要求连接 sshd 时就会启动 sshd.service 。因此如果想完全关闭sshd服务的话，需要同时停用 sshd.service 和 sshd.socket 。

```bash
systemctl stop sshd.service
systemctl stop sshd.socket
systemctl disable sshd.service sshd.socket
```








---
参考：
[CentOS7中systemctl的使用](https://blog.csdn.net/u012486840/article/details/53161574)