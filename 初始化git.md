1. 生成密钥
ssh-keygen -t rsa -C "邮箱地址"
-t 指定密钥类型 默认rsa
-C 注释文字
2. 测试是否成功免密
ssh -T git@github.com
显示如下
```
Hi smais0308! You've successfully authenticated, but GitHub does not provide shell access.
```
**最好使用ssh方式clone，https经常出错**

3. 查看远程项目地址
git remote -v
```
origin	git@github.com:smais0308/docker.git (fetch)
origin	git@github.com:smais0308/docker.git (push)
```
4. 修改远程提交地址
git remote rm origint
5. git remote add origin git@github.com:smais0308/docker.git
6. 提交
git push -u origin main
git commit -a -m "brief"


**会出现的问题**
```
sign_and_send_pubkey: signing failed: agent refused operation问题
错误解释：表示ssh-agent已经在运行，但是找不到附加的任何keys。
```
解决办法：
```
eval "$(ssh-agent -s)"
sshd-add
但是之在当前terminal中生效。
```
