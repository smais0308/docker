1. 下载软件附加执行权限
报错信息
```
[4995:0915/015407.167005:FATAL:setuid_sandbox_host.cc(158)] The SUID sandbox helper binary was found, but is not configured correctly. Rather than run without sandboxing I'm aborting now. You need to make sure that /home/smais/Downloads/squashfs-root/chrome-sandbox is owned by root and has mode 4755.

```
解决方法：
将文件解压缩
```
Obsidian-0.15.9.AppImage --appimage-extract
将chrome-sandbox文件权限修改成4755，用户及用户族root
sudo chown root:root chrome-sandbox
sudo chmod 4755 chrome-sandbox

然后运行 ./AppRun
```