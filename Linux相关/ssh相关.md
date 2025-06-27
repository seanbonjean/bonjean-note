# ssh相关

ssh连接是 ***用户A*** 连接 ***用户B***，通过user@host来定位某一机器上的用户，如：

```bash
ssh root@192.168.1.11
```

因此，.ssh文件夹是用户级别的，root用户和其他用户都有，在各自的家目录下

注：

1. 所有user@host的格式都可以通过在hosts文件中添加别名来简化，若在hosts文件中添加如下内容：

```bash
192.168.1.11 myserver
```

则可以使用

```bash
ssh root@myserver
```

来连接到目标用户

2. 用户A和用户B名称相同的情况下，可以简化为只使用机器名，如：

```bash
ssh 192.168.1.11
```

## ssh连接过程

虽然ssh连接不一定是连接服务器，但下文还是统一把被连接的机器称为服务器，以方便理解

1. 连接发起时，服务器就会发出自己的公钥以证明自己是可信任的服务器，防止中间人攻击。后续密钥交换过程的数据传输通道会用这个公钥进行加密

   这一点也体现在：初次连接服务器时，会提示你确认服务器的公钥指纹，如果确认，则会把公钥保存到本地的 `.ssh/known_hosts` 文件中，下次连接时，服务器仍然会发送公钥，客户端会先查询known_hosts文件，有一致的公钥就不再询问

2. 由于非对称加密太慢，ssh连接会尽量使用对称加密来传输数据。因此这一步就会先进行密钥交换，生成对称加密的密钥（即会话密钥，将在该次会话结束后销毁）。密钥交换过程使用服务器公私钥进行非对称加密通信。生成了对称加密的密钥后，后续的数据传输就会用对称加密通信
3. 认证用户身份，有两种方式：密码认证和公钥认证（后者就是常说的免密登录）。服务器先生成一个随机的“挑战”字符串（通常是一个随机的数字或哈希值），并将其发送给客户端。客户端会使用密码或私钥对挑战进行加密，并发送回服务器。服务器使用密码或公钥对加密后的挑战进行解密，以验证客户端的身份

   这里使用的是客户端的公私钥，也即“私钥签名，公钥鉴权”。允许登录该服务器的用户公钥保存在服务器的 `.ssh/authorized_keys` 文件中

   挑战的目的是为了确保每次认证都独立，不容易受到重放攻击：每次认证都会生成一个新的挑战，这样即使攻击者记录了某次成功的认证过程，也无法在后续使用相同的认证信息进行伪造。服务器通过挑战证明客户端知道正确的密码，而不会暴露密码本身

由上述过程可以发现，一个ssh连接会用到两对公私钥，服务器的公私钥用于防止中间人攻击，进行“公钥加密，私钥解密”，客户端的公私钥用于验证用户身份，进行“私钥签名，公钥鉴权”

## .ssh目录结构

.ssh目录通常包含以下文件：

* `config`：（作为客户端）记录了连接到服务器的默认配置
* `known_hosts`：（作为客户端）保存了信任的已知服务器公钥
* `authorized_keys`：（作为服务器）保存了允许通过公钥认证免密登录到服务器的客户端公钥
* `id_rsa.pub`、`id_ed25519.pub`等：客户端使用的公钥，用于解密和验证签名
* `id_rsa`、`id_ed25519`等：客户端使用的私钥，用于签名和加密

通过 `ssh-keygen` 命令可以生成客户端使用的公私钥对，并保存在 `.ssh` 目录下  
生成特定算法的公私钥对可以通过 `-t` 参数指定：

```bash
ssh-keygen -t ed25519
```

同时，还有一个地方保存了服务器使用的公私钥对： `/etc/ssh/ssh_host_*` ，用于防止中间人攻击。在 `/etc/ssh` 目录下的公私钥文件名形如：

* `ssh_host_rsa_key.pub`
* `ssh_host_rsa_key`
* `ssh_host_ecdsa_key.pub`
* `ssh_host_ecdsa_key`
* `ssh_host_ed25519_key.pub`
* `ssh_host_ed25519_key`

通过 `sudo ssh-keygen -A` 命令生成服务器使用的公私钥对，并保存在 `/etc/ssh` 目录下

## ~/.ssh/config文件

当需要经常连接到某台服务器时，可以配置config文件

```config
Host myserver  # 设置主机别名，可以通过ssh myserver命令直接连接
  HostName 192.168.X.X  # 主机地址，可以是IP或域名
  Port 22  # 端口号
  User user  # 用户名
  IdentityFile ~/.ssh/id_rsa  # 指定私钥文件路径，私钥的密码短语（如有）会在连接时询问（你不会觉得密码短语能明文存在这吧？）
  # windows下路径：C:\Users\用户名\.ssh\私钥文件名
  # linux下路径：~/.ssh/私钥文件名
  IdentitiesOnly yes  # 仅使用IdentityFile指定的密钥，不尝试加载其他密钥
  ForwardAgent yes  # 允许连接到的远程主机使用本地主机上的私钥连接其他主机
  # 例如：在远程主机执行git pull时，能直接使用本地主机的Github私钥
```

使用 `ssh myserver` 命令，根据config中的配置直接连接到服务器

## /etc/ssh/sshd_config文件

***注意：*** 先检查是否安装了openssh-server，不然找不到sshd_config文件

```bash
apt search openssh-server
apt install openssh-server
```

在 `/etc/ssh/sshd_config` 文件中，可以配置连接到该服务器的一些设置，比如：

* `Port 22` ssh监听的端口
* `PermitRootLogin no` 是否允许root用户登录
* `LoginGraceTime 2m`或`LoginGraceTime 120` 登录超时时间，超过该时间未登录则断开连接
* `MaxAuthTries 3` 登录失败次数，超过该次数则断开连接
* `MaxSessions 10` 最大同时连接数
* `PasswordAuthentication no` 是否允许密码认证
* `PubkeyAuthentication yes` 是否允许公钥认证
* `ChallengeResponseAuthentication no` 是否允许挑战响应认证（不影响公钥认证，因为公钥认证是通过对挑战进行签名来验证客户端身份的，没有“响应”）

注意这个文件是在 `/etc/ssh` 目录下的，而不是在用户的家目录下的 `.ssh` 目录下；配置文件是 `sshd` 服务的配置文件，而不是 `ssh` 客户端的配置文件 `ssh_config`

## 常用命令（客户端）

注意：确保.ssh目录权限为700（chmod -R 700 .ssh/），authorized_keys文件权限为600（chmod 600 .ssh/authorized_keys）

把客户端公钥发送到服务器authorized_keys文件中（实现免密登录），过程中会需要输入目标服务器的密码

```bash
ssh-copy-id sean@192.168.1.11
```

或者手动把客户端公钥添加到服务器authorized_keys文件中（需要登录到服务器手动cat）

建立ssh连接

```bash
ssh sean@192.168.1.11
```

scp复制本地文件到服务器（或者反过来），如果配置了免密登录，则不需要输入密码

```bash
scp /path/to/local/file sean@192.168.1.11:/path/to/remote/file
```
