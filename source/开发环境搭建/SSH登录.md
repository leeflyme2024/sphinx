## 1.修改密码

在 Linux 系统中修改 `root` 账号的密码，可以按照以下步骤操作：

```bash
sudo su            # 切换到root用户
passwd             # 修改root密码
Enter new UNIX password:  # 输入新密码
Retype new UNIX password: # 再次输入新密码
passwd: password updated successfully  # 提示密码更新成功
```

## 2.使能root账户登录

编辑 SSH 配置文件 `sshd_config`，允许 `root` 用户登录：

```bash
vi /etc/ssh/sshd_config
```

找到以下行并修改：

```plaintext
#PermitRootLogin prohibit-password
```

改为：

```plaintext
PermitRootLogin yes
```

或者，如果你希望使用密钥认证：

```plaintext
PermitRootLogin without-password
```
