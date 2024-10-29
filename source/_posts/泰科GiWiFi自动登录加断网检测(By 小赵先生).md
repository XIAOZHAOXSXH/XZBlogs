---
title: 泰科GiWiFi自动登录加断网检测(By 小赵先生)
abbrlink: 6d921db4
date: 2024-01-16 19:55:38
---

# OpenWRT 自动登录脚本教程（By 小赵先生）

**在泰科如果你使用GiWiFi，经常会遇到网络断开的情况，需要手动重新登录。所以我写了这个教程教大家如何进行自动登录以及断网检测自动重新登录。另外，寄WiFi狗都不用！**

## 1. 脚本简介

**该脚本主要执行以下操作：**

* **使用** `ping`命令检查网络连接。
* **如果网络断开，它会尝试通过提交登录表单来重新登录。**

## 2. 如何使用

### 2.1 下载脚本

**首先，从以下链接下载脚本：**

[下载 auto\_login\_script.sh](sandbox:/mnt/data/auto_login_script.sh)

**或者保存以下源代码：**

```
 #!/bin/sh
 
 # By 小赵先生
 # 适用于泰科GiWiFi，其他学校请自行查看Form并进行修改
 # 函数：检查网络连接
 check_network() {
     ping -c 1 8.8.8.8 > /dev/null 2>&1
     return $?
 }
 
 # 函数：登录操作
 login() {
     # 以下是登录页面的URL，你可能需要根据实际情况进行修改
     LOGIN_URL="http://login.example.com/login"
 
     # 隐藏字段的值，根据上面的HTML内容提取
     SIGN="h45POCfxljomxsV3HOOP2mD8HOvp6+XBnI1j8u0vq/4hBSn8z9qy9UvwxMjB0GX7xu87sSK5uLZq1p4S1nnSGpK7PqM2TbudRbvoC4pfrn5DHeAr1RHf4iCoQ+APUTky7d/tmqWg31xjtNAHz4z3r2RgFxiaKXCKIWO9CJYphLccphmibCXr3npfa0sKktRDXCYl4/emN/sB1YXvCmLJIA0sTt3xHR15GlgiPq40kSwyW0M8xPE574ynigqfmOfnjqdVAxYpESDCmEtiiRq9Yg=="
     STA_IP="10.16.1.60"
     NAS_NAME="tskj"
     REQUEST_IP="10.16.9.240"
     IV="c7a419caabb56bbc"
     LOGIN_TYPE="1"
     ACCOUNT_TYPE="2"
 
     # 用户名和密码，你稍后可以填写
     USER_ACCOUNT="YOUR_USERNAME_HERE" # 请在此处填写你的用户名
     USER_PASSWORD="YOUR_PASSWORD_HERE" # 请在此处填写你的密码
 
     # 使用curl提交表单
     curl -X POST "$LOGIN_URL" \
          -d "sign=$SIGN" \
          -d "sta_ip=$STA_IP" \
          -d "nas_name=$NAS_NAME" \
          -d "request_ip=$REQUEST_IP" \
          -d "iv=$IV" \
          -d "login_type=$LOGIN_TYPE" \
          -d "account_type=$ACCOUNT_TYPE" \
          -d "user_account=$USER_ACCOUNT" \
          -d "user_password=$USER_PASSWORD"
 }
 
 # 主逻辑
 check_network
 if [ $? -ne 0 ]; then
     echo "Network is down. Trying to login..."
     login
 else
     echo "Network is up. No action required."
 fi
```

### 2.2 设置权限

**下载并将脚本保存到你的OpenWRT路由器后，确保为其设置执行权限：**

```
 chmod +x /path/to/auto_login_script.sh
```

### 2.3 配置脚本

**使用文本编辑器打开脚本，并找到以下行：**

```
 USER_ACCOUNT="YOUR_USERNAME_HERE" # Fill in your username here
 USER_PASSWORD="YOUR_PASSWORD_HERE" # Fill in your password here
```

**请用你的登录信息替换** `YOUR_USERNAME_HERE`和 `YOUR_PASSWORD_HERE`。

### 2.4 设置定时任务

**为了使该脚本每天自动执行，你需要添加一个** `cron`任务。你可以通过编辑OpenWRT上的 `/etc/crontabs/root`文件来添加一个新的cron任务。例如，要使脚本每天上午7点自动执行，你可以添加以下行：

```
 0 7 * * * /path/to/auto_login_script.sh
```

### 2.5 手动运行

**你也可以手动运行此脚本来检查网络连接并在需要时登录：**

```
 sh /path/to/auto_login_script.sh
```

## 3. 总结

**这个脚本可以让OpenWRT路由器始终保持在线，并实现自动登录，节省用户时间。如需多设备登录防检测，我后续会出新的教程。**
