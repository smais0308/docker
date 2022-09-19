tree -l//以树状图列出目录内容
tree -a //所有
tree -i //不以阶梯状
tree -s  //列出文件或目录大小
tree -t  //按更改时间
tree -N //使能够正常显示中文文件 
stat myfile  //显示目录或文件的详细信息

![[Pasted image 20220919144432.png]]

last  //显示最近登录系统的用户信息-6列
history （10） //显示历史指令-默认1000行
sudo adduser lilei sudo  //给普通用户赋予root权限
sudo usermod -G sudo lilei  //同上
alias l=’ls’  //定义命令别名
unalias l  //删除别名
alias  //列出别名
