# 一些Linux命令
* `apt install`和`apt-get install`：安装软件
* `sudo`超级用户执行
* `su`改变用户(switch user)，如`su root`改变用户为root
* `ls`列表显示当前文件夹内容
* `cd`改变当前目录
* `mkdir`创建新目录
* `cp`复制
* `xz -d`、`tar -xf`、`tar jxvf`一些解压命令
* `make -j(核心数)`根据Makefile编译项目，可以指定使用的核心数量。运行make时，如果报错形如bison: not found，那就sudo apt install bison，谁not found就装谁；如果是fatal error: libelf.h: No such file or directory这种，装libelf-dev，软件包跟个-dev的是对应的开发包，开发包就是带头文件和静态库的

VMware linux虚拟机挂载共享文件夹:
 `sudo vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other`

也可以用root用户只执行 `vmhgfs-fuse .host:/ /mnt/hgfs` ，没有allow_other的话普通用户就访问不了
