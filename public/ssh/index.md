# VPS配置之五SSH

> 为便于VPS管理设置，我们需要开启SSH，安全起见，SSH需要在/etc/ssh/sshd_config中进行以下设置。

## 在服务器端设置
* 禁止root用户登录，找到`PermitRootLogin yes`改为no
* 禁止密码登录，找到`PasswordAuthentication yes`，改为no。
* 改变缺省端口，找到`Port 22`改为其他端口
## 在客户端设置
* 使用ssh-keygen生成
默认保存于ssh目录中,id_rsa 为私钥，id_rsa.pub 为公钥。。
```
ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/p3terx/.ssh/id_rsa):
Created directory '/home/p3terx/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/p3terx/.ssh/id_rsa.
Your public key has been saved in /home/p3terx/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:qssp3ZnX0bgxbSUOlecZllcDAjX4nqjL3hA/HRtoGd8 p3terx@hk2
The key's randomart image is:
+---[RSA 2048]----+
|         .++ o.oo|
|         .  = = o|
|         ... + + |
|          *.o +  |
|       .S+oX.E   |
|       .+.*oO    |
|   . ..+.+ O     |
|  ...o=.+ +      |
|   .=..=..       |
+----[SHA256]-----+
``` 
* 使用ssh-copy-id上传公钥
```
ssh-copy-id -i ~/.ssh/id_rsa.pub -p Port User@HostName 
```
{{< admonition >}}
ssh-copy-id 命令相当于执行了以下复杂的手动操作：
* 复制公钥文件中的内容
```
cat ~/.ssh/id_rsa.pub
```
* 登录到远程主机
```
ssh User@HostName -p Port
```
* 创建 ~/.ssh 目录
```
mkdir -p ~/.ssh
```
* 把公钥文件写入到 ~/.ssh/authorized_keys
```
vim ~/.ssh/authorized_keys
```
* 设置权限
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
{{< /admonition >}}
* 使用ssh-key登录
```
ssh User@HostName -p Port
```


