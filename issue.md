服务器配置公钥认证

**本地windows生成SSH密钥对**

```powershell
ssh-keygen
```

**获取公钥**

这个文件的名字是 `id_rsa.pub` 或者 `id_ed25519.pub`

```powershell
Get-Content ~/.ssh/id_rsa.pub
```

将公钥复制下来，打开远程服务器的 `~/.ssh/authorized_keys` 文件，将公钥粘贴进去

```powershell
nano ~/.ssh/authorized_keys
```

**服务器配置允许公钥认证**

在某些情况下，服务器可能被配置为不允许使用公钥认证。查看服务器的 `/etc/ssh/sshd_config` 文件，确保 `PubkeyAuthentication` 选项被设置为 `yes`。

如果你在 `/etc/ssh/sshd_config` 文件中看到 `#PubkeyAuthentication yes` 这一行，那么 `PubkeyAuthentication` 实际上被注释掉了，因为它前面有一个 `#` 符号。这意味着服务器并未启用公钥认证。

你需要去掉这一行前面的 `#` 符号，以启用公钥认证。你可以使用文本编辑器（如 `nano` 或 `vi`）打开 `/etc/ssh/sshd_config` 文件，找到 `#PubkeyAuthentication yes` 这一行，删除前面的 `#` 符号，然后保存并关闭文件。

完成这一步之后，你需要重启 SSH 服务以使更改生效。在大多数 Linux 系统中，你可以使用以下命令重启 SSH 服务：

```bash
sudo service ssh restart
```

或

```bash
sudo systemctl restart sshd
```

这样，你的服务器就启用了公钥认证，你应该可以使用 SSH 公钥进行身份验证了。
