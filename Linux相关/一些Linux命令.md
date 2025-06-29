# 一些Linux命令

首先介绍两个实用的CLI工具：
* `tldr`一个比`man`更简单易懂的命令文档查询工具：https://github.com/tldr-pages/tldr
* `fuck`自动纠正命令错误：https://github.com/nvbn/thefuck

## Just For Fun

* `neofetch`/`fastfetch`打印系统信息
* `fortune`随机输出名言
* `cowsay [message]`输出各种动物（默认是牛）说话的ascii表情
* `fortune | cowsay`输出牛说fortune名言的ascii表情

## 最基础

* `sudo`超级用户执行
* `su`改变用户(switch user)，如`su root`改变用户为root
* `ls`列表显示当前文件夹内容
* `cd`改变当前目录
* `mkdir`创建新目录
* `cp`复制
* `mv`移动或重命名（源路径和目标路径相同，仅文件名不同，即为重命名）
* `rm`删除

## 日常使用

* `passwd`修改密码
* `top`/`ps -aux`查看当前系统进程
* `htop`查看当前系统进程（更高级）
* `netstat -a`显示所有网络连接
* `lsof -i:80`查看正在使用端口的进程
* `whoami`显示当前用户
* `who`显示当前登录到该机器的所有用户
* `id`显示当前用户id、用户组id等信息
* `cwd`显示当前工作目录的绝对路径
* `xz -d`、`tar -xf`、`tar jxvf`一些解压命令
* `date`显示当前时间
* `cal`显示当月日历`cal [month] [year]`显示指定年月日历`cal [year]`显示指定年全年日历

## 文件管理

* `mount /dev/sda1 /mnt/udisk0`临时挂载设备（如U盘），关机后失效。修改`/etc/fstab`文件以实现开机自动挂载
* `udisksctl unmount -b /dev/sda1`卸载文件系统
* `udisksctl power-off -b /dev/sda`关闭设备电源（弹出U盘）
* `lsof /mnt/udisk0`查看文件被哪个进程占用
* `fdisk -l`查看磁盘分区信息
* `fdisk`修改磁盘分区表
* `lsblk`列出块设备信息
* `lsblk -f`列出块设备信息，带磁盘uuid（用于在`/etc/fstab`中配置磁盘挂载）
* `df -h`查看磁盘使用情况
* `chmod`修改文件权限，查阅：https://www.runoob.com/linux/linux-comm-chmod.html
* `chown user[:group] /path/to/file`修改文件所有者
* `chgrp group /path/to/file`修改文件所属用户组

### /etc/fstab文件

`/etc/fstab` 文件用于配置开机自动挂载的设备，格式如下：

```
<file system> <mount point> <type> <options> <dump> <pass>
UUID=xxx-xx... /mnt/pool0 ext4 defaults 0 2
```

磁盘设备UUID建议通过 `lsblk -f` 命令查看，也可以通过 `blkid` 命令查看

其中：
* `options` 参数可以设置多个，用逗号分隔，`defaults`表示以默认参数`rw, suid, dev, exec, auto, nouser, async`挂载。后面的参数会覆盖前面的defaults参数，例如，`defaults,ro`表示挂载的该磁盘只读
* `dump` 字段表示是否需要被dump命令备份， `0` 表示不需要， `1` 表示需要，通常根文件系统 `/` 设为1，其余为0
* `pass` 字段表示是否需要在系统启动时被fsck命令检查，`0`表示不检查，`1`表示最先检查，`2`表示在设为1的磁盘之后检查，通常根文件系统 `/` 设为1，其余为2或0

一般磁盘设为 `defaults 0 2` 即可

## systemctl

* `status`查看状态
* `list-units`查看所有服务 | 条件查询示例：`systemctl list-units --type=target --all | grep suspend`
* `enable`开机自启
* `disable`禁止开机自启
* `start`启动
* `stop`停止
* `restart`重启
* `reload`重新加载
* `mask`屏蔽服务，屏蔽后无法启动
* `unmask`取消屏蔽

### 启动级别

| Runlevel | systemd Target         | 功能说明                                               |
|----------|------------------------|-------------------------------------------------------|
| 0        | poweroff.target        | 🔴 关机（系统 halt）                                   |
| 1        | rescue.target          | 🔧 单用户模式（Single User Mode），仅 root 登录，用于维护|
| 2        | multi-user.target      | 👤 多用户模式，无网络服务，无图形界面（部分系统为默认模式）|
| 3        | multi-user.target      | 🌐 多用户模式，有网络服务，无图形界面，常用于服务器       |
| 4        | multi-user.target      | ⚙️ 用户自定义（大多数系统默认与 runlevel 3 相同）        |
| 5        | graphical.target       | 🖥️ 图形界面模式（含登录管理器，如 GNOME/KDE）            |
| 6        | reboot.target          | 🔁 重启（系统 reboot）                                 |

注意：systemd将runlevel 2、3、4都视为相同的multi-user.target

* `systemctl get-default`查看当前默认启动级别
* `sudo systemctl set-default multi-user.target`设置默认启动级别
* `sudo systemctl isolate multi-user.target`切换到指定启动级别

## 软件包管理相关

（根据不同的包管理器选择）  
搜索软件（根据大概的名字，从软件源中查询某个软件包的确切名字）： `apt search` 、 `yum search` 、 `dpkg -l | grep` 等  
安装软件： `apt install` 、 `yum install` 、 `dpkg -i` 等  
卸载软件： `apt remove` 、 `yum remove` 、 `dpkg -r` 等  
升级软件： `apt upgrade` 、 `yum upgrade` 等  
更新软件源： `apt update` 、 `yum update` 等

### apt源配置

配置文件在 `/etc/apt/sources.list`

（各种发行版都可选）
* 中科大源：https://mirrors.ustc.edu.cn/help/ubuntu.html
* 清华源：https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/
* 阿里源：https://developer.aliyun.com/mirror/
* 网易源：https://mirrors.163.com/.help/ubuntu.html

更新apt源后，运行： `sudo apt update`

## Makefile编译相关

* `make -j(核心数)`根据Makefile编译项目，可以指定使用的核心数量。运行make时，如果报错形如bison: not found，那就sudo apt install bison，谁not found就装谁；如果是fatal error: libelf.h: No such file or directory这种，装libelf-dev，软件包跟个-dev的是对应的开发包，开发包就是带头文件和静态库的

## 虚拟机

VMware linux虚拟机挂载共享文件夹:
 `sudo vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other`

也可以用root用户只执行 `vmhgfs-fuse .host:/ /mnt/hgfs` ，没有allow_other的话普通用户就访问不了
