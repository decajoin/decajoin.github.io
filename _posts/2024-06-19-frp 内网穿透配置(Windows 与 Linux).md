---
title: frp 内网穿透配置(Windows 与 Linux)
tags:
  - Linux
---

## 前期准备

需要一台具有**公网 IP 的服务器**（可以外网访问的服务器）

需要被穿透的内网设备

配置主要分两部分：

- frp 服务端，布局在具有公网的 IP 的服务器
- frp 客户端，布局在内网设备

### frp 安装

在服务器端和客户端分别安装 frp 并赋予相关执行权限

frp 一键安装命令，参考[秋风的 frp 一键安装脚本](https://www.right.com.cn/forum/thread-7668427-1-1.html)

```bash
sudo bash -c "$(curl -sS https://gitee.com/autumn2868/qf-frp/raw/master/qf_frp-install.sh)"
```

赋予执行权限：

```bash
sudo chmod +x /etc/qffrp/frpc
```

## 服务器端配置

### 服务器 frp 配置

打开服务器端 frp 配置文件：

```bash
sudo vim /etc/qffrp/frps.ini
```

将文件内容修改为如下：

复制的时候记得删除所有注释（可能会引起未知错误）

```bash
[common]
# frp监听的端口，默认是7000，可以改成其他的
bind_port = 7000
# 授权码，这个token之后在客户端会用到
token = 1234

# frp管理后台端口，请按自己需求更改
dashboard_port = 7012
# frp管理后台用户名和密码，请改成自己的
dashboard_user = admin
dashboard_pwd = admin

# frp日志配置
log_file = /var/log/frps.log
log_level = info
log_max_days = 3
```

启动服务器端 frp 服务：

```bash
# 启动命令
systemctl start frps
# 重启命令
systemctl restart frps
# 状态查看命令
systemctl status frps
```

启动后状态如图

![image-20240619113414722](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/image-20240619113414722.png)

### 服务器防火墙配置

云服务器需要再后台开放端口。对于腾讯云服务器，需要在后台进入防火墙配置，然后添加规则，开放对应端口。

![img](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1718771617186-04c36b5b-5123-4f61-b5d3-c4bdfeb90022.png)

还需要再命令行中配置开放端口

首先查看已开放端口

```bash
sudo firewall-cmd --list-all
```

![img](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1726494079915-bd12dfc2-110e-4327-93ff-dd732407eb6e.png)

检查对应的端口是否打开，打开需要的端口（比如现在我们需要打开 7002 端口），然后可以再检查端口开放情况，发现 7002 端口已经打开。

```bash
sudo firewall-cmd --add-port=7002/tcp --permanent
sudo firewall-cmd --reload
```

![img](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1726494182863-a3fa7313-6e11-46d0-b389-5c29c36e02cf.png)

### 验证是否启动成功

在浏览器中输入：http://服务器公网 IP:frp 后台端口号，如：http://X.X.X.X:7012

输入用户名和密码（frps.ini 中配置的）出现下图即可说明服务端成功启动

![image-20240619115152292](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/image-20240619115152292.png)

## 客户端配置

### 客户端 frp 配置

打开服务器端 frp 配置文件：

```bash
sudo vim /etc/qffrp/frpc.ini
```

将文件内容修改为如下：

复制的时候记得删除所有注释（可能会引起未知错误）

```bash
# frp客户端
[common]
server_addr = 服务器公网ip地址
 # 与frps.ini的bind_port一致
server_port = 7000
 # 与frps.ini的token一致
token = 1234

# 配置ssh服务
[ssh]
type = tcp
# 也可以是当前设备局域网内的其它IP
local_ip = 127.0.0.1
local_port = 22
# 这个自定义，之后再ssh连接的时候要用
remote_port = 7001
```

启动服务器端 frp 服务：

```bash
# 启动命令
systemctl start frpc
# 重启命令
systemctl restart frpc
# 状态查看命令
systemctl status frpc
```

启动后状态如图

![image-20240619115516178](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/image-20240619115516178.png)

### 客户端防火墙配置

防火墙开放端口(remote_port)：

```bash
sudo ufw allow 7001/tcp
sudo ufw reload
```

也可以直接关闭防火墙

```bash
sudo ufw disable
```

记得在服务器端防火墙也开启对应端口

### 测试穿透是否成功

```bash
ssh -p 端口号 用户名@服务端ip
```

这里用的是客户端的用户名和服务器的公网 ip，端口号用的 frpc.ini 文件中的 remote_port

下图是成功连接的示例

![image-20240619121013866](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/image-20240619121013866.png)

PS. 在配置内网穿透之前强烈建议首先在局域网环境测试 ssh 是否正常使用

## Windows 系统下的穿透
上述都是 Linux 环境下的 frp 的安装和使用，下面介绍一下 Windows 下的安装和使用。

### frp  安装
下载 frp，我这里使用的是 v.0.45.0，[下载地址](https://github.com/fatedier/frp/releases/tag/v0.45.0)

### 客户端 frp 配置
默认你的 frp 是云服务器，关于云服务器端的配置参见上文。所以，这里我们只需要对客户端进行配置。

![](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1733298824633-712200ca-bec3-4634-a7cd-fcb41fa0e11d.png)

打开 frpc.ini 进行设置，文件内容设置如下（其实和 Linux 下的客户端配置完全一样）

```bash
# FRP客户端
 [common]
 server_addr = 服务器公网ip地址
  # 与frps.ini的bind_port一致
 server_port = 7000
  # 与frps.ini的token一致
 token = 1234

 # 配置ssh服务
 [ssh]
 type = tcp
 # 也可以是当前设备局域网内的其它IP
 local_ip = 127.0.0.1
 local_port = 22
 # 这个自定义，之后再ssh连接的时候要用
 remote_port = 7001
```

配置完成后，在这个文件夹路径打开终端，运行下面命令即可进行连接

```bash
./frpc.exe -c ./frpc.ini
```

运行结果如下，代表穿透成功

![](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1733299087142-d23b252a-e489-4f9c-8985-4c97c0692e6f.png)
