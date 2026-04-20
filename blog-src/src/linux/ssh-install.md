# 安装 SSH 服务



1. **更新系统**

   ```shell
   sudo apt update
   sudo apt upgrade -y
   ```

   

2. **检查 ssh 服务**

   ```shell
   sudo systemctl status ssh
   ```

   注意此命令输出信息中 `Active` 的状态，如果显示 `inactive (dead)`则表示 ssh 已经安装了但未启用，则可以执行 *第 6 步* 设置开机自动启动 ssh 服务。

   

3. **安装 OpenSSH Server**

   如果显示未安装 ssh 服务，则需要执行这一步

   ```shell
   sudo apt install openssh-server
   ```

   

4. **配置 OpenSSH Server**

   编辑 `/etc/ssh/sshd_config` 来配置 ssh

   常见的设置：

   - Port：可以将其更改为不同的端口号以获得额外的安全性
   - PermitRootLogin：为了安全起见，最好通过将其设置为 no 来禁用 root 登录
   - PasswordAuthentication：如果使用公钥身份验证，请将其设置为 no
   - AllowUsers：要限制对特定用户的 SSH 访问，请在此处添加用户名，并用空格分隔



5. **重新启动 OpenSSH Server**

   如果修改了 OpenSSH Server 的配置或者刚刚才安装，执行这一步。

   ```shell
   sudo systemctl restart ssh
   ```

   

6. **设置开机自动启动 OpenSSH Server**

   ```shell
   sudo systemctl enable ssh
   ```



7. **ssh 远程连接这条电脑**

   可在 windows cmd 使用

   ```shell
   ssh 用户名@机器IP
   ```

