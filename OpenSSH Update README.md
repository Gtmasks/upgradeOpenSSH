# upgradeOpenSSH
一键升级CentOS 7上的OpenSSH到8.0p1
## 使用方法
```
./upgradeOpenSSH
```
## 脚本运行完后
```
source /etc/profile.d/path.sh
或者重新开一个新的ssh连接
然后即可检查ssh是否升级成功
ssh -V
```

```bash
预编译安装
./configure --prefix=/opt/openssh8.0p1 --sysconfdir=/etc/ssh --with-ssl-dir=/opt/openssl1.0.2t --with-pam --with-tcp-wrappers --host=x86_64 
```

```bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0604 for '/opt/openssh8.0p1/etc/ssh_host_ed25519_key' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.


##############################################
```

```bash
以上报错是由于错误的权限导致，密钥文件不允许其他用户有访问权限，有两种解决办法，一是，修改配置文件，直接注释掉密钥文件，注释如下：  /opt/openssh8.0p1/etc/sshd_config
# HostKey for protocol version 1
# HostKey /etc/ssh/ssh_host_key
# HostKeys for protocol version 2
# HostKey /opt/openssh8.0p1/etc/ssh_host_rsa_key
# HostKey /etc/ssh/ssh_host_dsa_key
# HostKey /opt/openssh8.0p1/etc/ssh_host_ecdsa_key
# HostKey /opt/openssh8.0p1/etc/ssh_host_ed25519_key

二是，修改指定文件的权限
chmod 600  /opt/openssh8.0p1/etc/ssh_host_ed25519_key
```

使用systemctl start sshd 正常启动未报错，但是客户端无法连接，且报 权限禁止 

**`/bin/bash: Permission denied`**

也是密钥文件权限导致。

可以通过执行`sshd 的绝对路径`，根据上面的warring 修改文件权限。