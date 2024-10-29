---
title: Vultr搭建V2ray（By 小赵先生）
encrypted: true
password: Zwx@666888
abbrlink: 518067ff
date: 2024-01-16 19:42:47
---

# Vultr搭建V2ray（By 小赵先生）

* **优惠：！！！**[Vultr Match Promo Code for Vultr Cloud Hosting - Vultr.com](https://www.vultr.com/zh/match/?lang=zh)
* **Vultr最新优惠：**[https://www.vultryhw.cn/vultr-lastest-promo/](https://www.vultryhw.cn/vultr-lastest-promo/)
* **官网地址：**[https://www.vultr.com](https://www.vultr.com/?ref=7164730)
* **VULTRMATCH优惠码100美金，首次充值使用**

![QQ截图20221008232616](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/QQ%E6%88%AA%E5%9B%BE20221008232616.png)

![image-20221008232810541](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221008232810541.png)

![image-20221008233117637](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221008233117637.png)

**公网IP和账号密码都在这里，用这个连接服务器**

![image-20221008234831711](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221008234831711.png)

**namesilo购买域名配置解析，配置刚才创建的服务器ip**

![image-20221008235019716](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221008235019716.png)

**配置成这个样子**

![image-20221008235230161](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221008235230161.png)

**在这里配置解析加速，这里跟上一次那里一样**

![image-20221008235436963](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221008235436963.png)

**复制这两个名称服务器，去namesilo里面自定义名称服务器**

![image-20221008235837961](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221008235837961.png)

![image-20221008235944009](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221008235944009.png)

**在这里粘贴**

**提交**

**回到服务器**

**ssh连接上服务器**

![image-20221009000717877](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009000717877.png)

**连接成功**

**修改系统时间：**

```
 dpkg-reconfigure tzdata
```

![image-20221009001031934](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009001031934.png)

**鼠标上下键选择Asia**

**回车**

![image-20221009001231302](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009001231302.png)

**选择上海时间**

**回车**

![image-20221009001309421](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009001309421.png)

**显示这样，输入date -R查看当前时间**

## 1、开启防火墙

### Debian/Ubuntu 放行端口

**例如要放行80 443端口**

```
 iptables -I INPUT -p tcp --dport 80 -j ACCEPT
 
 iptables -I INPUT -p tcp --dport 8443 -j ACCEPT
 
 iptables -I INPUT -p tcp --dport 37915 -j ACCEPT
```

![image-20221009001443123](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009001443123.png)

**然后保存放行规则**

```
 iptables-save
```

![image-20221009001514864](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009001514864.png)

**保存成功**

**reboot重启一下**

```
 reboot
```

![image-20221009001633624](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009001633624.png)

**重新连接至服务器**

**2、申请SSL证书**

```
 apt update -y       # Debian/Ubuntu 命令
 
 apt install -y curl   # Debian/Ubuntu 命令
 
 apt install -y socat  # Debian/Ubuntu 命令
```

![image-20221009002220184](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009002220184.png)

**执行成功**

```
 curl https://get.acme.sh | sh
 
 ~/.acme.sh/acme.sh --register-account -m 1102524049@qq.com #这里可以设置你想要设置的邮箱
```

![image-20221009002431249](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009002431249.png)

**这样就是安装成功了**

## **这一步操作注意！！！**

**执行第一条命令**

```
 #故障真实邮箱成功率上升 
 ~/.acme.sh/acme.sh --issue -d test.laoxuaaa.life --standalone #这个地方域名填写你购买的那个，每台服务器不一样
 
 改变你的解析域
 ~/.acme.sh/acme.sh --installcert -d test.laoxuaaa.life --key-file /root/private.key #这里域名也注意改动
```

![image-20221009002709102](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009002709102.png)

**如果执行第一条命令这个样子**

**打开cmd**

![image-20221009002915395](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009002915395.png)

**ping一下域名，成功**

**关掉刚刚的ssh窗口，重新连接服务器**

![image-20221009003004017](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009003004017.png)

## 重新放开一下端口

```
 iptables -I INPUT -p tcp --dport 80 -j ACCEPT
 
 iptables -I INPUT -p tcp --dport 8443 -j ACCEPT
 
 iptables -I INPUT -p tcp --dport 37915 -j ACCEPT
 
 iptables -I INPUT -p tcp --dport 443 -j ACCEPT
```

**重新执行**

```
~/.acme.sh/acme.sh --issue -d test.laoxuaaa.life --standalone
```

![image-20221009003355927](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009003355927.png)

![image-20221009003510966](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009003510966.png)

**这次就成功了**

**然后执行刚刚第二条命令**

```
~/.acme.sh/acme.sh --installcert -d test.laoxuaaa.life --key-file /root/private.key --fullchain-file /root/cert.crt
```

![image-20221009003539139](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009003539139.png)

**成功**

## 3、安装X-ui面板

```
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

![image-20221009004120599](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009004120599.png)

**这里会让你设置账号密码，自定义就行**

**端口写8443**

![image-20221009004228348](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009004228348.png)

**回车**

![image-20221009004249534](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009004249534.png)

**安装完成**

**可以访问ip（公网服务器ip）：8443访问面板**

![image-20221009004356949](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009004356949.png)

**这里是刚刚设置的账号密码**

![image-20221009004447113](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009004447113.png)

**成功进入面板**

**点击左侧面板设置**

```
#公钥路径：
/root/cert.crt
#密钥文件路径:
/root/private.key
```

**直接复制上去**

![image-20221009004647781](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009004647781.png)

**保存设置**

![image-20221009004709854](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009004709854.png)

**之后重启面板**

**选择可以使用域名访问**

![image-20221009004822665](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009004822665.png)

![image-20221009004833161](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009004833161.png)

**成功访问**

**登录面板**

![image-20221009004910964](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009004910964.png)

**点击入站列表，点击加号新建连接**

![image-20221009005143547](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009005143547.png)

**按照我这样设置**

**备注和路径可以自定义，域名填你解析到这台服务器那个**

![image-20221009005233146](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009005233146.png)

**查看链接**

**可以访问了，速度也不错，不过还是配置上bbr加速更好**

![image-20221009005648983](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009005648983.png)

**部署bbr前已经跑满了我的校园网**

**开始部署bbr**

```
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```

![image-20221009010036872](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009010036872.png)

**选择2**

**回车**

![image-20221009010109636](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009010109636.png)

**出现这个界面，方向键右键选no回车**

![image-20221009010150864](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009010150864.png)

**输入y重启，重新手动连接服务器**

**再输入一遍命令**

![image-20221009010247880](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009010247880.png)

**显示已安装，输入7启动加速**

![image-20221009010328813](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009010328813.png)

**启动完成**

**重新放通一下端口**

```
iptables -I INPUT -p tcp --dport 80 -j ACCEPT

iptables -I INPUT -p tcp --dport 8443 -j ACCEPT

iptables -I INPUT -p tcp --dport 37915 -j ACCEPT

iptables -I INPUT -p tcp --dport 443 -j ACCEPT
```

![image-20221009010833024](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221009010833024.png)

**8k视频都秒开**

**——By小赵先生**
