---
title: WireGuard 服务端纯手工部署与配置指南（By 小赵先生）
tags: 技术分享
abbrlink: 36000
date: 2026-03-27 15:10:01
---
# WireGuard 服务端纯手工部署与配置指南

本文档旨在提供一份详尽的 WireGuard 服务端手动部署教程。通过规避自动化脚本的“黑盒”操作，帮助开发者精准掌控网络路由配置与底层逻辑。

## 0. 部署前置条件

在开始部署之前，请确保具备以下条件：

- **操作系统**：本文以 Ubuntu 22.04 / Debian 12 为例（CentOS 系命令略有不同，但核心逻辑一致）。
- **权限**：具备 `root` 权限或拥有 `sudo` 提权能力。
- **网络环境**：服务器需拥有独立的公网 IP。
- **安全组/防火墙**：已在云服务商控制台放行 WireGuard 所需的 UDP 端口（本文默认使用 `51820` 端口）。

## 1. 开启内核 IPv4 流量转发

WireGuard 依赖 Linux 内核的路由转发功能来实现 VPN 内网与公网的数据通信。

1. 编辑 `sysctl` 配置文件：

   ```bash
   sudo nano /etc/sysctl.conf
   ```
   
2. 在文件末尾添加或取消注释以下行：

   ```toml
   net.ipv4.ip_forward=1
   ```
   
3. 使配置立即生效：

   ```bash
   sudo sysctl -p
   ```
   
   *预期输出应包含 `net.ipv4.ip_forward = 1`。*

## 2. 安装核心依赖包

WireGuard 的核心模块已集成于 Linux 5.6+ 内核中，只需安装用户态工具及流量转发所需的 `iptables`。

```bash
sudo apt update
sudo apt install wireguard wireguard-tools iptables -y
```

## 3. 生成服务端密钥对

WireGuard 采用非对称加密机制。需要为服务端生成一对公私钥。

1. 切换至 WireGuard 配置目录并确保权限安全：

   ```bash
   sudo su
   cd /etc/wireguard
   umask 077
   ```
   
2. 生成私钥（Private Key）与公钥（Public Key）：

   ```bash
   wg genkey | tee server_privatekey | wg pubkey > server_publickey
   ```
   
3. 查看并记录密钥内容（后续配置文件中需要使用）：

   ```bash
   cat server_privatekey
   cat server_publickey
   ```

## 4. 确认服务器公网网卡名称

在编写配置文件前，**必须**确认当前服务器连接公网的物理网卡名称。这是配置 NAT 转发规则的关键。

执行以下命令查看默认路由网卡：

```bash
ip route | grep default
```

*预期输出示例：`default via 192.168.1.1 dev eth0 proto dhcp ...`*

在上述示例中，`eth0` 即为公网网卡名称。请记住你服务器的实际网卡名称（可能是 `ens3`、`eth0` 等）。

## 5. 编写服务端配置文件

创建并编辑 WireGuard 虚拟网卡配置文件 `wg0.conf`。

```bash
nano /etc/wireguard/wg0.conf
```

将以下内容填入文件中，并**务必替换尖括号 `< >` 及大写提示部分**为你实际的参数：

```toml
[Interface]
# 虚拟局域网内的服务端 IP 地址与子网掩码（建议保留 10.0.0.1/24）
Address = 10.0.0.1/24

# 服务端私钥（填入第 3 步中 cat server_privatekey 的输出内容）
PrivateKey = <在此处填入服务端私钥>

# 监听的 UDP 端口
ListenPort = 51820

# 开启 NAT 转发（注意：请将 eth0 替换为第 4 步中获取到的实际公网网卡名称！）
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

# ----------------------------------------------------
# 以下为预留的客户端配置区域（Peer）。每增加一个客户端，就追加一个 [Peer] 块。

[Peer]
# 客户端的公钥（需在客户端设备上生成）
PublicKey = <在此处填入客户端公钥>

# 分配给该客户端的内网 IP 地址（唯一，不可与其他客户端冲突）
AllowedIPs = 10.0.0.2/32
```

保存并退出（在 nano 中按 `Ctrl+O`，`Enter`，然后 `Ctrl+X`）。

## 6. 启动服务与设置开机自启

配置完成后，使用 `wg-quick` 工具启动虚拟网卡。

1. 启动 `wg0` 接口：

   ```bash
wg-quick up wg0
   ```

   *若配置正确，无任何报错提示。*

2. 验证运行状态：

   ```bash
wg
   ```

   *预期将输出 `interface: wg0` 的当前状态、监听端口及公钥信息。*

3. 将其注册为 systemd 服务，实现开机自启：

   ```bash
   systemctl enable wg-quick@wg0
   ```

## 7. 客户端接入说明（补充）

服务端部署完毕后，客户端的接入逻辑如下：

1. **生成客户端密钥**：在客户端设备（如 PC、手机）上生成客户端私钥与公钥。
2. **服务端注册客户端**：将客户端的**公钥**及分配的**内网 IP**（如 `10.0.0.2/32`）填入服务端的 `wg0.conf` 的 `[Peer]` 块中，并执行 `wg-quick down wg0 && wg-quick up wg0` 重启接口。
3. **客户端配置**：在客户端配置文件中填写以下核心参数：
   - `PrivateKey` = 客户端自身私钥
   - `Address` = 10.0.0.2/24
   - `PublicKey` (Peer区域) = 服务端公钥
   - `Endpoint` = 服务端公网 IP:51820
   - `AllowedIPs` = 0.0.0.0/0 (代表所有流量全局代理) 或 10.0.0.0/24 (仅代理内网流量)

至此，WireGuard 服务端的核心部署已全部完成。