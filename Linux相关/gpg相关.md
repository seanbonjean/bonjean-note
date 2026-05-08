# gpg相关

GPG (Gnu Privacy Guard) 是一个开源的加密软件，遵循OpenPGP (Pretty Good Privacy) 标准

## 功能

与普通公私钥一样，GPG key有文件加密、数字签名等功能；除此之外，GPG还有如下功能：
1. 信任链 (Web of Trust)：A给B的公钥签名，C原本不信任B，但C信任A，因此C间接地信任了B
2. 主/子密钥：GPG允许一个主密钥有多个子密钥，子密钥可以有各自不同的用途
3. 有效期：可以为密钥指定有效期，过期的密钥将不能再被使用，但仍可以用公钥验证到期前签过的签名；有效期可以手动延长，永久有效的密钥也可以撤销
4. 绑定个人信息 (USER-ID)：GPG密钥和个人信息 (名字和邮箱组成的USER-ID) 强绑定

## 相关命令

`gpg --full-generate-key` ：生成密钥对

`gpg --delete-secret-keys [KEY-ID]` ：删除主私钥和关联的所有子私钥 (命令中如果不带复数"s"，则只删除KEY-ID对应私钥)

`gpg --delete-keys [KEY-ID]` ：删除主公钥和关联的所有子公钥 (删除公钥的前提是对应私钥已被删除)

`gpg --list-secret-keys --keyid-format=long` ：列出所有密钥对

列出的密钥中：  
rsa4096/[KEY-ID]：前面为加密算法，后面为密钥的KEY-ID  
标有 `sec` 的是主密钥，标有 `ssb` 的是子密钥  
每个密钥拥有各自的功能标识：
* `S` (Sign)：签名数据、git commit等
* `E` (Encrypt)：加密数据
* `C` (Certify)：给其他key签名
* `A` (Authentication)：身份认证 (例如ssh等协议)

并且，每个USER-ID都有一个信任等级，比如，自己创建的密钥的信任等级为 `u` (ultimate)

`gpg --edit-key [KEY-ID]` ：编辑密钥对：进入gpg交互界面

1. `addkey`：添加子密钥
2. `trust`：设置信任等级
3. `sign`：为他人USER-ID签名
4. `key [KEY-ID]`：选中密钥
5. `delkey`：删除密钥 (需要先选中密钥)
6. `expire`：重新设置主密钥有效期（选中子密钥以设置子密钥有效期）
7. `list`：再次列出密钥对
8. `save`：保存并退出

`gpg --armor --export [KEY-ID] > [公钥文件.asc]` ：导出公钥 (主/子密钥皆可)

`gpg --armor --export-secret-keys [KEY-ID] > [私钥文件.asc]` ：导出私钥

`gpg --armor --export-secret-subkeys [KEY-ID] > [子私钥文件.asc]` ：导出子私钥

`gpg --import [密钥文件]` ：导入密钥

## 吊销证书

吊销证书用来撤销GPG key：如果私钥丢失、被泄露或者忘记密码，就可以用这个证书通知别人这个key已经不再可信  
吊销证书不包含私钥，只包含KEY-ID和撤销签名的声明

使用 `gpg --import [吊销证书.rev]` 导入吊销证书，就能标记该key已不再可信  
然后，需要将吊销证书上传到公钥服务器： `gpg --keyserver hkps://keys.openpgp.org --send-keys [KEY-ID]` ，以告知他人；或将吊销证书直接分享给他人使用 `gpg --import [吊销证书.rev]` 导入
