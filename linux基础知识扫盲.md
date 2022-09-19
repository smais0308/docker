### 用户与用户组管理
/etc/passwd	用户
/etc/group 用户组信息
/etc/shadow 用户的密码信息

1. 用户
	1. 创建用户
		useradd username -m  //创建用户，不能被立即使用，需要设置密码
		passwd test     //给test这个用户设置密码



userdel -r test    //删除用户 -r表示删除家目录
注意要在后面追加-m，否则不会在home路径下创建该用户的文件夹。



### 系统相关
init 6 //重启 （0-停机，1-单用户，2-多用户，3-完全多用户，4-图形化，5-安全模式，6-重启）

