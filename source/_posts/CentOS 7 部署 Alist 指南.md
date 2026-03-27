---
title: CentOS 7 部署 Alist 指南
tags: 技术分享
date: 2025-10-11 17:01:01
---

### CentOS 7 部署 Alist 指南

Alist 是一个支持多种存储的文件列表程序，后端基于 `gin`，前端使用 `react`。本文记录在 CentOS 7 环境下的部署过程。

**环境:**
- 系统：CentOS 7
- 工具：FinalShell (或其他 SSH 客户端)
- 权限：`sudo` 或 `root`

---

#### 一、部署方式

提供三种方式：一键脚本、手动安装、Docker。

**方式一：一键脚本安装**

这是最快的方法。

```bash
curl -fsSL "https://nn.ci/alist.sh" | bash -s install
```

- 默认安装路径为 `/opt/alist`。
- 如需自定义路径，例如安装到 `/root`：
  ```bash
  curl -fsSL "https://nn.ci/alist.sh" | bash -s install /root
  ```

**方式二：手动安装**

1.  从 [AList Release](https://github.com/alist-org/alist/releases) 下载对应架构的文件，如 `alist-linux-amd64.tar.gz`。
2.  解压并赋予执行权限。
    ```bash
    tar -zxvf alist-linux-amd64.tar.gz
    chmod +x alist
    ```
3.  运行程序。
    ```bash
    ./alist server
    ```

**方式三：Docker 部署**

1.  安装 Docker。
    ```bash
    sudo yum install -y yum-utils device-mapper-persistent-data lvm2
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    sudo yum install -y docker-ce docker-ce-cli containerd.io
    
    sudo systemctl start docker
    sudo systemctl enable docker
    ```
2.  拉取镜像并运行容器。
    ```bash
    sudo docker run -d --name alist -p 5244:5244 -v /mydata/alist/data:/opt/alist/data --restart=unless-stopped xhofe/alist:latest
    ```
    - `-p 5244:5244`: 端口映射。
    - `-v /mydata/alist/data:/opt/alist/data`: 数据目录挂载，请根据实际情况修改宿主机路径。

---

#### 二、防火墙配置

CentOS 7 默认使用 `firewalld`，必须开放 5244 端口。

```bash
sudo firewall-cmd --zone=public --add-port=5244/tcp --permanent
sudo firewall-cmd --reload
```

验证端口是否开放：

```bash
sudo firewall-cmd --list-ports
```

---

#### 三、服务启动与密码获取

**一键脚本/手动安装:**

1.  启动服务并设置开机自启：
    ```bash
    sudo systemctl start alist
    sudo systemctl enable alist
    ```
2.  查看管理员密码：
    ```bash
    sudo /opt/alist/alist admin
    ```

**Docker 部署:**

- 查看容器日志获取密码：
  ```bash
  sudo docker logs alist
  ```

---

#### 四、访问与配置

1.  浏览器访问 `http://<CentOS IP>:5244`。
2.  使用 `admin` 和获取到的密码登录。
3.  登录后，在管理面板修改密码。
4.  在“存储”页面添加你的网盘或本地目录。

---

#### 五、常见问题

1.  **无法访问 Web 界面**
    - 检查防火墙规则。
    - 检查 VMware 网络模式（桥接或 NAT 端口转发）。
    - 在虚拟机内执行 `curl http://localhost:5244` 确认服务状态。

2.  **忘记管理员密码**
    - **非 Docker**: `sudo /opt/alist/alist admin set <新密码>`
    - **Docker**: `sudo docker exec -it alist ./alist admin set <新密码>`

3.  **更新 Alist**
    - **一键脚本**: `curl -fsSL "https://nn.ci/alist.sh" | bash -s update`
    - **手动**: 下载新版本，替换可执行文件，重启服务。
    - **Docker**: `sudo docker pull xhofe/alist:latest`，然后重新创建容器。
