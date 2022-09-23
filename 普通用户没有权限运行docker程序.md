报错信息如下：
![[Pasted image 20220923151412.png]]
```bash
[smais@localhost docker]$ docker run hello-word
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
```


解决办法：
- 将使用的用户添加到docker组中
- 更新组信息
```shell
sudo gpasswd -a $USER docker #添加用户到docker组中
newgrp docker #刷新组SID
```
