使用MobaxTerm版本22.1原版
远程环境：
```shell
[smais@localhost ~]$ locale
LANG=en_US.UTF-8
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_PAPER="en_US.UTF-8"
LC_NAME="en_US.UTF-8"
LC_ADDRESS="en_US.UTF-8"
LC_TELEPHONE="en_US.UTF-8"
LC_MEASUREMENT="en_US.UTF-8"
LC_IDENTIFICATION="en_US.UTF-8"
LC_ALL=
```

产生问题原因：
远程环境和本地环境编码不一致，导致终端（terminator）上显示中文乱码。
![[Pasted image 20220919142653.png]]

远程环境如上，则也要在客户端使用相应的字符集编码，从FFont charset下拉框中找不到相应的中文字符集，则说明选择的字体不合适，从Font中选择相应的的字体，此处选择黑体，字体字符集中选择 DEFAULT和GB2312中其中一个， 设置成功后如图所示：
![[Pasted image 20220919143324.png]]
![[Pasted image 20220919143434.png]]

中文已经正常显示
![[Pasted image 20220919143556.png]]

--- 
- 参考
[MobaxTerm中文乱码问题解决](https://blog.csdn.net/wswg2009314/article/details/126505920)

