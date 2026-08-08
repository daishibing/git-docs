# SSH 协议访问远程仓库

Git 访问远程仓库主要有两种方式：

- HTTPS
- SSH

SSH（Secure Shell）是一种安全的远程连接协议

Git 使用 SSH 协议连接远程仓库时，通过 SSH Key（密钥对）进行身份认证，验证通过后即可访问远程仓库，无需每次输入账号密码

# 生成 SSH Key

使用 `ed25519` 算法生成 SSH 密钥：

```shell
# 创建保存 SSH Key 的目录
mkdir -p ~/.ssh/account-A

# 创建 SSH Key
ssh-keygen -t ed25519 -C "<email@example.com>" -f ~/.ssh/account-A/id_ed25519
```

参数说明：

- `-t ed25519`：指定密钥类型
- `-C`：添加密钥备注信息，用于标识该密钥，可以填写 GitHub 邮箱或其他用于识别该密钥的信息
- `-f`：指定 SSH Key 保存路径，一般存放在 `~/.ssh/` 目录下，多账号场景建议通过不同目录进行管理

生成后文件：

- `~/.ssh/account-A/id_ed25519`：私钥，需要保护
- `~/.ssh/account-A/id_ed25519.pub`：公钥，可以上传到 GitHub

# 查看公钥并上传至 GitHub

查看公钥：

```shell
cat ~/.ssh/account-A/id_ed25519.pub
```

复制内容到：

```text
GitHub → Settings → SSH and GPG keys → New SSH key
```

# 配置 SSH config

创建文件：

```shell
vim ~/.ssh/config
```

配置内容：

```text
# 账号 A
Host github-account-A
    HostName github.com
    User git
    IdentityFile ~/.ssh/account-A/id_ed25519
    IdentitiesOnly yes

# 账号 B
Host github-account-B
    HostName github.com
    User git
    IdentityFile ~/.ssh/account-B/id_ed25519
    IdentitiesOnly yes
```

参数说明：

- `Host`：定义 SSH 连接匹配规则，通常作为连接时使用的别名
- `HostName`：实际连接的服务器地址
- `User`：SSH 登录用户名，GitHub 连接远程仓库时固定使用 `git`
- `IdentityFile`：私钥路径
- `IdentitiesOnly yes`：只使用 `IdentityFile` 指定的密钥，不尝试 SSH agent 或其他默认密钥

# 测试 SSH 连接

执行：

```shell
ssh -T git@github-account-A
```

> 注意：这里使用的是 `Host` 配置中的别名，而不是直接使用 github.com

成功提示：

```text
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

> 说明 SSH Key 认证成功，但 GitHub 不提供通过 SSH 登录服务器 Shell 的功能


