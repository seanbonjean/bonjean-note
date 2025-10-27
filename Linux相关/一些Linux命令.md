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

* `sudo`超级用户执行；`sudo -l`查看当前用户权限
* `su`改变用户(switch user)，如`su root`改变用户为root
* `ls`列表显示当前文件夹内容
* `cd`改变当前目录
* `mkdir`创建新目录
* `cp`复制
* `mv`移动或重命名（源路径和目标路径相同，仅文件名不同，即为重命名）
* `rm`删除

## 日常使用

* `top`/`ps -aux`查看当前系统进程（`top`运行中按`h`查看帮助，`t`和`m`分别切换CPU和内存的条状占用率显示）
* `htop`查看当前系统进程（更高级，带鼠标操作）
* `ip a`（查询IP地址）查看所有网络接口，如eth0、wlan0（连接到局域网的对应接口会列出本机IP地址）
* `netstat -a`显示所有网络连接
* `lsof -i:80`查看正在使用端口的进程
* `date`显示当前时间
* `cal`显示当月日历`cal [month] [year]`显示指定年月日历`cal [year]`显示指定年全年日历

## 终端复用（后台多窗口）

`screen` 命令可以在一个终端连接里开启并同时存在多个虚拟的终端会话，并且在关闭终端后，程序依然在会话中保持运行

* `screen -ls`列出当前所有会话
* `screen -R [session name]`新建并进入一个会话
* `screen -r [session id/name]`回到一个已有的会话（如果只存在一个会话，可以省略session id/name）
* `screen -d -r [session id/name]`回到一个已有但attached的会话（比如当你连接在这个session上时断开了ssh连接，此时session仍然处于attached状态，需要先detach再attach）

在session内时的命令由 `Ctrl + a` 开头：
* `Ctrl + a d`将当前会话detach（后台挂起）
* `Ctrl + a ?`显示帮助

## 监控命令结果

有时候需要监控系统状态，观察某个命令输出的变化，需要借助 `watch` 命令来重复执行某个命令，并实时刷新显示结果

比如：
* `watch ls`实时显示当前目录的文件变化
* `watch free`实时显示内存使用情况
* `watch "ps aux | grep java"`监控某一进程运行情况

尽量给所执行的命令加上引号，防止产生歧义，尤其是带管道的命令

选项：
* `-n`：指定刷新间隔，默认为2秒
* `-d`：高亮显示前后变化(difference)

例如： `watch -n 1 -d "ps aux | grep java"`

## 模糊查找

`fzf` 命令是一个基于 fuzzy matching 的命令行筛选工具，可以快速筛选出符合要求的结果

读取从管道输入的数据源后， `fzf` 提供一个交互界面等待用户输入想要查找的关键字，方向键选择，回车键选中并返回匹配的行

几个使用场景：
* 搜索命令历史：`history | fzf`
* 搜索由`find`过滤后的文件：`find . -type f | fzf`
* 搜索文件并用vim打开：`vim $(ls|fzf)`

## 用户与主机

* `whoami`显示当前用户
* `who`显示当前登录到该机器的所有用户
* `id`显示当前用户id(UID)、用户组id(GID)等信息
* `hostname`显示当前主机名
* `hostnamectl set-hostname xxx`更改主机名，重启后生效（将会更改/etc/hostname文件，并处理与主机名相关的其他系统配置）
* `cwd`显示当前工作目录的绝对路径
* `passwd`修改密码
* `useradd -m username`创建用户
* `userdel -r username`删除用户
* `usermod -l newusername username`更改用户登录名
* `groups username`显示用户所属的所有组（组列表在/etc/group中；组和用户权限强相关，用户加入某个组后就能获取特定的权限，当某个程序报"Permission denied"时通常需要让用户加入某个组）
* `usermod -aG groupname username`添加用户到组，例如：`usermod -aG sudo username`添加到sudo组
* `usermod -u newUID username` `usermod -g newGID username`更改用户UID和GID
* `chsh -s /bin/bash`设置用户默认shell（如果sudo运行，更改的是root的默认shell），通过`cat /etc/shells`查看可用shell列表；`chsh -s /bin/bash username`更改其他用户的默认shell（如果用户没有权限更改默认shell，用sudo运行并使用username指定用户）

## 文件管理

* `diff file1 file2`比较两个文件，分别显示差异（不显示相同处，没有差异时不显示任何信息），加`-u`参数后输出一份完整文件（包括相同内容）并在差异处显示差异
* `diff -r path1 path2`递归比较两个目录，显示里面文件名的差异
* `rsync -avh source/ destination/`增量同步`source`文件夹到`destination`下（形成`destination/source/`），`destination`文件夹中有但`source`中没有的文件不会被删除
* `rsync -avh --delete source/ destination/`同步并删除`destination`中的多余文件（即镜像同步）
* `rsync -avh --dry-run source/ destination/`模拟同步，只显示同步过程中会进行的所有增删等操作，不进行实际同步
* `rsync -avh /path/to/source/ user@remotehost:/path/to/destination/`远程同步，将文件同步到远程主机，支持断点续传，例如：`rsync -avh ~/Documents/ admin@192.168.1.100:/home/admin/backup/`，反过来也可以，还可以以本机为跳板将一个远程同步到另一个远程
* `ln -s /path/to/file /path/to/link`创建软链接（可以是文件或目录）
* `mount /dev/sda1 /mnt/udisk0`临时挂载设备（如U盘），关机后失效。修改`/etc/fstab`文件以实现开机自动挂载
* `udisksctl unmount -b /dev/sda1`卸载文件系统
* `udisksctl power-off -b /dev/sda`关闭设备电源（弹出U盘）
* `lsof /mnt/udisk0`查看文件被哪个进程占用
* `fdisk -l`查看磁盘分区信息
* `fdisk`修改磁盘分区表
* `lsblk`列出块设备信息
* `lsblk -f`列出块设备信息，带磁盘uuid（用于在`/etc/fstab`中配置磁盘挂载）
* `df -h`查看磁盘使用情况
* `du -h`查看当前路径下所有文件夹大小，设置`--max-depth=1`（或简写为`-d1`）将递归限制在当前目录
* `du -ah`查看当前路径下所有文件夹***和文件***大小，递归限制同上，可写为`du -ahd1`
* `du -h filename`查看某个文件大小
* `ncdu filepath`一个交互式的磁盘空间使用情况查看器（`ncdu`默认查看当前目录）
* `chmod`修改文件权限，查阅：https://www.runoob.com/linux/linux-comm-chmod.html
* `chown user[:group] /path/to/file`修改文件所有者
* `chgrp group /path/to/file`修改文件所属用户组

### 打包和压缩

一般单个文件可以直接压缩，例如将文件xxx压缩为xxx.gz；而多个文件则需要先打包为xxx.tar，然后再压缩为xxx.tar.gz

#### 打包/解包

`tar` 命令用于打包和解包文件

* `-c`：打包
* `-x`：解包
* `-v`：显示详细进度
* `-f`：指定打包文件名（必须加`-f`，除非你想用管道）

示例：
* `tar -cvf xxx.tar folder/`打包目录，显示进度
* `tar -xf xxx.tar`解包，不显示进度

#### 压缩/解压

`gzip` 和 `xz` 命令都可以用于压缩和解压文件，区别在于 `xz` 压缩率高，压缩速度慢

无论是压缩还是解压， `gzip` 和 `xz` 都会将源文件删除，只留下压缩后/解压后的文件

示例：
* `gzip xxx`压缩文件
* `gzip -d xxx.gz`解压文件
* `gunzip xxx.gz`也是解压文件，功能与`gzip -d`一样
* `xz xxx`压缩文件
* `xz -d xxx.xz`解压文件
* `unxz xxx.xz`也是解压文件，功能与`xz -d`一样

注意，这里的文件xxx当然可能是.tar文件，压缩后就变成.tar.gz文件或.tar.xz文件

`tar` 命令可以直接添加 `-z` 或 `-J` 参数，指定打包/解包后直接使用gzip或xz进行压缩/解压

例如：
* `tar -czvf xxx.tar.gz folder/`打包目录并压缩，显示进度
* `tar -xJvf xxx.tar.xz`解压文件并解包，显示进度

注意 `-J` 是大写的， `-j` 对应的是另一个压缩算法bzip2(.bz2)

#### 与Windows的兼容

`zip` 和 `unzip` 命令可以兼容Windows下的.zip文件

* `zip xxx.zip file1 file2`Windows下的压缩概念，包含打包和压缩，不删除源文件
* `zip -r xxx.zip folder/`对文件夹压缩，递归包括里面的文件（注意：压缩文件夹时如果不加`-r`参数，则只会压缩一个空壳文件夹，并且***不会报错***！）
* `unzip xxx.zip`Windows下的解压概念，包含解压和解包，不删除源文件

### 查找文件

`find` 命令用于查找文件，格式： `find [搜索路径] [匹配条件] [执行操作]`

例如： `find ./logs -name "*.txt" -delete` 删除所有当前目录下的logs目录中，以.txt结尾的文件

`[搜索路径]` 默认为 `.` ；如果不给定 `[执行操作]` ，则默认输出所有找到的文件

匹配条件有：
* `-type f`：只查找文件/目录（`f`文件，`d`目录）
* `-name "*.txt"`：匹配文件名（区分大小写）
* `-iname "*.txt"`：匹配文件名（不区分大小写）
* `-size +100M`：匹配大于100M的文件（`+`大于，`-`小于）
* `-mtime -7`：匹配最近七天内***修改***的文件
* `-atime +7`：匹配七天之前***访问***的文件
* `-perm 0777`：匹配权限为777的文件

常用的执行操作可以是 `-delete` 和 `-exec`

`-exec` 参数可以执行命令，格式为： `-exec [包含{}的命令] \;`

其中：
* `{}`：占位符，代表find找到的每一个文件
* `\;`：表示-exec命令的结束（注意转义）

例如： `find . -name "*.log" -exec mv {} ./tmp/logs/ \;` 会将所有匹配到的.log文件移动到./tmp/logs目录下

如果用 `-execdir` 代替 `-exec` 后，所执行的命令会在***该文件所在的目录***中执行

例如： `find . -name "*.log" -execdir gzip {} \;` ，这样压缩包会和.log文件放在一起，而不是find命令执行时所在的目录

### /etc/fstab文件

`/etc/fstab` 文件用于配置开机自动挂载的设备，格式如下：

```
<file system> <mount point> <type> <options> <dump> <pass>
UUID=xxx-xx... /mnt/pool0 ext4 defaults 0 2
```

修改fstab后，通过 `sudo mount -a` 挂载fstab中没有被挂载的和挂载情况与fstab中不一致的设备（但是挂载点属于root），或 `reboot` 后自然会按fstab挂载；并注意挂载后使用 `chown` 等命令修改用户权限

其中：
* `options` 参数可以设置多个，用逗号分隔，`defaults`表示以默认参数`rw, suid, dev, exec, auto, nouser, async`挂载。后面的参数会覆盖前面的defaults参数，例如，`defaults,ro`表示挂载的该磁盘只读
* `dump` 字段表示是否需要被dump命令备份， `0` 表示不需要， `1` 表示需要，通常根文件系统 `/` 设为1，其余为0
* `pass` 字段表示是否需要在系统启动时被fsck命令检查，`0`表示不检查，`1`表示最先检查，`2`表示在设为1的磁盘之后检查，通常根文件系统 `/` 设为1，其余为2或0

一般磁盘设为 `defaults 0 2` 即可

磁盘设备UUID建议通过 `lsblk -f` 命令查看，也可以通过 `blkid` 命令查看；如果没有UUID，说明磁盘没有**格式化**，方法见下文

#### 新建分区并格式化

如果没有UUID，说明磁盘没有**格式化**；分区和格式化是不同的概念：先修改分区表有了分区，才能对分区进行格式化，格式化为指定的文件系统

首先使用 `fdisk` 命令新建分区，在设备（如 `sda` ）上创建分区（如 `sda1` ）

```bash
sudo fdisk /dev/sda
p（查看分区表）
n（添加新分区）
输入分区号（sda1就填1，或者直接回车默认按顺序新建，有sda1就会新建sda2）
起始和结束扇区这些直接回车默认
p（查看分区表，当前未实施的改动也会显示）
w（保存分区表并退出fdisk）
或q（不保存直接退出）
```

然后使用 `mkfs` 命令格式化分区：

```bash
sudo mkfs.ext4 /dev/sda1
```

分区的文件系统类型可以修改为想要的类型，除了FAT32使用 `mkfs.vfat` 外，其他类型都是直接使用本名（如 `mkfs.xfs` 、 `mkfs.ntfs` 、 `mkfs.exfat` 等）

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

## wget和curl

wget和curl都可用于下载文件。wget更适合下载文件，支持递归下载、断点续传；而curl更侧重于构建和发送自定义请求数据包，并且支持多种协议

### wget

* `wget http://example.com/file.txt`下载文件
* wget无法上传文件
* `wget -c http://example.com/file.txt`断点续传

### curl

* `curl -O http://example.com/file.txt`下载文件
* `curl -o newfile.txt http://example.com/file.txt`下载文件并指定保存的文件名
* `curl -T file.txt ftp://example.com/ --user username:password`FTP协议上传文件
* `curl http://example.com`发送一个HTTP GET请求
* `curl -X POST -d "param1=value1&param2=value2" http://example.com/resource`发送一个HTTP POST请求，`-d`指定POST的参数
* `curl -L http://example.com`需要显式地允许重定向
* `curl -H "Content-Type: application/json" http://example.com`设置HTTP请求头

## 软件管理

### 包管理器

（根据不同的包管理器选择）  
搜索软件（根据大概的名字，从软件源中查询某个软件包的确切名字）： `apt search` 、 `yum search` 等  
搜索本地软件（dpkg数据库）： `dpkg -l | grep` （ `ii` 表示已安装； `rc` 表示曾安装过，卸载后残留配置文件；其他状态可以通过直接运行 `dpkg -l` 查看抬头说明）  
安装软件： `apt install` 、 `yum install` 、 `dpkg -i` 等  
卸载软件： `apt remove` 、 `yum remove` 、 `dpkg -r` 等  
升级软件： `apt upgrade` 、 `yum upgrade` 等  
更新软件源： `apt update` 、 `yum update` 等

#### apt源配置

配置文件在 `/etc/apt/sources.list`

（各种发行版都可选）
* 中科大源：https://mirrors.ustc.edu.cn/help/ubuntu.html
* 清华源：https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/
* 阿里源：https://developer.aliyun.com/mirror/
* 网易源：https://mirrors.163.com/.help/ubuntu.html

更新apt源后，运行： `sudo apt update`

### 切换同一软件的多个安装版本

* `update-alternatives --list java`查看某个工具的多个可替换安装版本
* `update-alternatives --config java`列出所有可替换版本并让用户选择版本进行切换
* `update-alternatives --display java`列出所有符号链接（太详细，建议直接用--list）
* `update-alternatives --auto java`将版本选择设置为自动模式
* `update-alternatives --set editor /usr/bin/vim.basic`进入手动模式，并指定默认运行该版本

某一个安装版本后标明的“自动模式”意味着：如果当前是自动模式(Status: auto)，则系统自动选择安装版本运行，而该版本将会是系统选择的版本  
如果当前是手动模式(Status: manual)，则标明的“自动模式”没有意义

#### 手动添加/删除一个版本

 `update-alternatives --install <主链接路径> <名字> <候选程序路径> <优先级>`

`update-alternatives --remove <名字> <候选程序路径>` （只是从alternatives管理中删除，不会删除可执行文件）

* 主链接路径：一般是/usr/bin/java，是用户实际使用的命令路径（命令会去/usr/bin下寻找java）
* 名字：这是alternatives管理器内部使用的名字（例如java）
* 候选程序路径：你要添加的实际程序路径，可执行的文件已安装在此处
* 优先级：决定自动模式下的选择，数字越大，优先级越高

例如：

```bash
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-21-openjdk-amd64/bin/java 100
```

意思是：把/usr/lib/jvm/java-21-openjdk-amd64/bin/java添加为java的一个候选实现，并建立与/usr/bin/java的管理链接。如果在自动模式下，优先级为100

运行后，系统会创建符号链接：
* /etc/alternatives/java -> /usr/lib/jvm/java-21-openjdk-amd64/bin/java
* /usr/bin/java -> /etc/alternatives/java

## Makefile编译

* `make -j(核心数)`根据Makefile编译项目，可以指定使用的核心数量。运行make时，如果报错形如bison: not found，那就sudo apt install bison，谁not found就装谁；如果是fatal error: libelf.h: No such file or directory这种，装libelf-dev，软件包跟个-dev的是对应的开发包，开发包就是带头文件和静态库的

## 虚拟机

VMware linux虚拟机挂载共享文件夹:
 `sudo vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other`

也可以用root用户只执行 `vmhgfs-fuse .host:/ /mnt/hgfs` ，没有allow_other的话普通用户就访问不了
