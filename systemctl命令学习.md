


从CentOS7.X 开始使用systemd服务来代替daemon，原来管理系统启动和管理服务系统服务的相关命令全部由systemctl命令来替代。

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













---
[CentOS7中systemctl的使用](https://blog.csdn.net/u012486840/article/details/53161574)