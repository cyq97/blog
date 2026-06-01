# 使用主机名进行 ssh 连接



虚拟机一般没有配置静态 IP，使用同样的 IP 访问可能哪天就失效了，而使用主机名就不用担心这个事情了。



## 环境

宿主机：win10

虚拟机软件：VMware

虚拟机：ubuntu, **网络 - 桥接模式**



## 配置

ubuntu 端设置

1. 老习惯，更新

   `sudo apt-get update` 

   `sudo apt-get upgrade -y`

2. 使用 `hostname` 查看主机名

3. 安装 Samba

   `sudo apt install samba -y`

4. 启动并启用 NetBIOS 名称服务 (nmbd) 和 SMB 服务 (smbd)

   `sudo systemctl enable --now smbd nmbd`

5. 验证. 在 win 主机上 ping 主机名



## ssh 连接

在 windows cmd 中输入 `ssh 用户名@主机名` 连接