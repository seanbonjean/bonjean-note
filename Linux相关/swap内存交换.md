# swap内存交换

## 查看swap状态

```bash
swapon --show
```

如果没有任何输出，说明目前没启用swap  
`free -h` 的输出中也会显示swap，如果显示为0，也说明目前没启用swap

## 创建swap

### 创建swap分区

需要在硬盘分区时就准备好swap分区，因此这里不介绍相关方法。具体实现可以参考arch linux的安装指南

### 创建swap文件

假设在根目录下创建一个2G的swap文件

```bash
# 创建一个大小为2G的swap文件
sudo fallocate -l 2G /swapfile

# 设置权限（很重要，否则会有安全隐患）
sudo chmod 600 /swapfile

# 格式化为swap
sudo mkswap /swapfile

# 启用swap
sudo swapon /swapfile
```

最后在 `/etc/fstab` 最后添加一行 `/swapfile none swap sw 0 0` ，设置开机自动挂载swap

## swap使用倾向

Linux内核通过swappiness参数决定在多大程度上使用swap（0 = 尽量少用，100 = 积极使用），默认值为60

查看swap使用倾向：

```bash
cat /proc/sys/vm/swappiness
```

设置临时的swappiness：

```bash
sudo sysctl -w vm.swappiness=60
```

设置永久有效的swappiness：修改 `/etc/sysctl.conf` 中的 `vm.swappiness=60`

## 禁用swap

### 临时禁用swap

```bash
sudo swapoff -a
```

### 永久禁用swap

注释掉 `/etc/fstab` 中关于swap的行（例如: `/swap.img none swap sw 0 0` ）
