- 需要注意selinux
`setenforce 0 #临时关闭selinux`
`vim /etc/selinux/config #selinux 的配置文件`
>SELINUX=disabled
- 需要注意防火墙
`systemctl stop firewalld #临时关闭防火墙 `
`systemctl disable firewalld #永久关闭防火墙`
`systemctl enable firewalld #开启防火墙`
1. 安装samba服务
yum install samba -y
systemctl start samba 
`vim /etc/samba/smb.conf #samba 的配置文件`
```shell
	comment = gongxiang
	path=/home/john/gongxiang
	public = no
	read only = yes 
	valid users=john root
	wirte list = root
```
2. 添加samba用户
```shell
pdbedit -a test      #创建和系统用户对应的Samba共享用户test和root

testparm    #使用：“testparm” 命令工具对 “smb.coonf ”配置文件的正确性进行检查
```


















配置项解释
```shell
comment：共享目录的描述信息；

path：设置对应共享目录在服务器上的文件夹路径；

public：是否所有人可以访问共享目录；

read only：是否只读，与 “writable” 作用相反；

valid users：共享目录的授权设置，允许哪些用户访问共享目录，这里设置了两个用户 “ test”和
“root”。也可授权一个组，可以使用：“@组名” 的形式，但也需要为组内的每个系统用户创建对应的Samba共享用户。

write list：设置共享目录为 “只读” 后，也可以单独授予某些用户有写入的权限（这里授权“root” 用户可以写入）；

写入上述信息后，保存退出即可，若要共享多个目录，另起一行以同样的格式写入即可；
==================================================================================
在 “smb.conf ” 文件中存在三个特殊的配置段：

[ global ] ：全局设置：这部分配置项的内容对整个Samba服务器都有效。

[ homes ]
：宿主目录的共享设置：设置Linux用户的默认共享，对应用户的宿主目录。当用户访问服务器中与自己用户名同名的共享目录时，通过验证后将自动映射到该用户的宿主文件夹中。

[ printers ]：打印机共享设置：如果需要共享打印机设备，可以在这部分进行配置

==================================================================================
Smb.conf文件中常见的配置项及含义说明：

常见全局配置项的含义 workgroup：所在工作组名称；

netbios name ：设置NetBIOS名称。如果不填，则默认会使用该服务器的DNS名称的第一部分。netbios
name和workgroup名字不要设置成一样了。

server string：服务器描述信息；

security：安全级别，可用值如下：User（本服务器验证连接）、server（指定另一台服务器验证）、ads（由Windows域控制器验证）；

log file：日志文件位置，“%m” 变量表示客户机地址；

passwd backend：设置共享账户文件的类型；

comment：对共享目录的注释、说明信息；

path：共享目录在服务器中对应的实际路径；

browseable：该共享目录在“网上邻居”中是否可见；

guest ok：是否允许所有人访问，等效于“public” ；

writable：是否可写，与 read only 的作用相反；

我们想要共享某个目录时，在配置文件的最后另起一行，按照上面的格式输入相应的信息，就可以了


