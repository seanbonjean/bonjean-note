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
* `rm`删除

## 日常使用

* `passwd`修改密码
* `top`/`ps -aux`查看当前系统进程
* `htop`查看当前系统进程（更高级）
* `netstat -a`显示所有网络连接
* `whoami`显示当前用户
* `who`显示当前登录到该机器的所有用户
* `xz -d`、`tar -xf`、`tar jxvf`一些解压命令
* `date`显示当前时间
* `cal`显示当月日历`cal [month] [year]`显示指定年月日历`cal [year]`显示指定年全年日历

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

安装软件： `apt install` 、 `yum install` 、 `dpkg -i` 等（根据不同的包管理器选择）
卸载软件： `apt remove` 、 `yum remove` 、 `dpkg -r` 等
升级软件： `apt upgrade` 、 `yum upgrade`

更新软件源： `apt update`

## Makefile编译相关

* `make -j(核心数)`根据Makefile编译项目，可以指定使用的核心数量。运行make时，如果报错形如bison: not found，那就sudo apt install bison，谁not found就装谁；如果是fatal error: libelf.h: No such file or directory这种，装libelf-dev，软件包跟个-dev的是对应的开发包，开发包就是带头文件和静态库的

## 虚拟机

VMware linux虚拟机挂载共享文件夹:
 `sudo vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other`

也可以用root用户只执行 `vmhgfs-fuse .host:/ /mnt/hgfs` ，没有allow_other的话普通用户就访问不了
