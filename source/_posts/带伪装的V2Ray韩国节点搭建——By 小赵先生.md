---
title: 带伪装的V2Ray韩国节点搭建——By 小赵先生
encrypted: true
password: Zwx@666888
abbrlink: b49f5259
date: 2024-01-16 19:46:57
---

# 带伪装的V2Ray韩国节点搭建——By 小赵先生

## 创建服务器

[https://dash.99stack.com/](https://dash.99stack.com/)

**进入服务器管理页面**

**创建一个服务器，经过我测试，对于韩国的节点搭建，选择Centos7 64位系统**

![image-20220930234056657](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930234056657.png)

**配置如上就可**

![image-20220930234246964](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930234246964.png)

## Xshell连接服务器

**创建成功后需要等待一段时间，有时创建成功后，也需要一段时间才能连接上服务器**

![image-20220930234602768](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930234602768.png)

**连接服务器不再赘述，这里我用的我上传的公钥，结束演示后我会删除我本地的密钥，只有你能控制服务器**

**公钥名：TEST**

**公钥值：ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAy/L3pOkX+BDMwnCznLXg9WS+g+aryG+nrf88No/ixH1JUHMK5rdXeg6i6GT+N0OCwXIxxr450KyWi8z6wvr0YhuJZ9mTmqhhqxriUNrzy9PzMZZyn2cpGsmKO9RG0ts3YoXM1xO8K52CVG79BXT5fjuftEyXLsQSsr587C6KSrom8pytwLhoiPx7QwJU2S9Ax9JlDET27Mkw+tpRfb2lc4cGrsWCAuMCi4/PgKDCFNjkCeXQLAGLH1ZUDnmiMA2TPdR8CI/GCVTyThyUV/EutpXMSBQi54PGuwhSpjlkLZLt3Y9OgMuSwrBhlI3Qt1ig/Nz2fCYB0kjHDJ48SFkxkw== rsa 2048-093022**

**公钥密码：test666888**

![image-20220930234830581](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930234830581.png)

**连接成功**

## 购买域名

[从这个网站购买域名（点我）](https://www.namesilo.com/)

![image-20220930235017385](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930235017385.png)

**直接搜索，购买.top的一般比较便宜，最便宜的就够了**

**优惠码如下，减1美元**

* **wht1off**
* **happyholiday**
* **holidaygift**
* **leb1off**
* **pianyivps**
* **oldtang**

[或者用这个链接进行查询（点我）](https://www.uphezi.com/namesilo-coupon/)

![image-20220930235511601](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930235511601.png)

**购买完成后点击这个**

![image-20220930235550182](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930235550182.png)

**点这个1**

![image-20220930235610111](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930235610111.png)

## 配置域名解析

![image-20220930235657249](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930235657249.png)

**点这个蓝球**

![image-20220930235757330](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930235757330.png)

**把这里原本域名公用时的解析全部删掉**

![image-20220930235854118](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20220930235854118.png)

**删完之后点这个大A**

![image-20221001000114151](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001000114151.png)

**然后点submit提交**

**然后再点A，**

![image-20221001000257216](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001000257216.png)

**这次填www，后边还是服务器IP**

**提交**

## 配置域名解析加速

**然后到cloudflare配置解析加速**

[点这个跳转](https://dash.cloudflare.com/)

![image-20221001000503999](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001000503999.png)

**添加站点**

![image-20221001000542258](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001000542258.png)

**这里填你刚刚购买的域名**

![image-20221001000737228](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001000737228.png)

**免费的就行**

**点继续**

![image-20221001000815362](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001000815362.png)

**添加记录**

![image-20221001000900340](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001000900340.png)

**前边填刚刚购买的域名，后边填服务器IP**

![image-20221001001159360](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001001159360.png)

**这里关掉**

**保存就行**

**ok了这里，可以配置服务器了**

## 服务器v2ray节点搭建

**1、检查时间  date -R **

![image-20221001001440202](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001001440202.png)

**2、修改时间  cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime **

![image-20221001001514297](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001001514297.png)

**ok了**

**开始安装v2ray服务**

```
 source <(curl -sL https://multi.netlify.app/v2ray.sh) --zh
```

![image-20221001002314015](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001002314015.png)

**安装成功**

**回退版本**

```
 v2ray update v4.45.2
```

![image-20221001002427667](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001002427667.png)

**成功**

**输入v2ray进行管理**

**输入1再输入1启动v2ray**

![image-20221001002518363](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001002518363.png)

**启动成功**

**输入3再输入5更改传输协议**

**选择3，websocket**

![image-20221001002647229](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001002647229.png)

**在这里输入刚刚你购买的域名**

![image-20221001002728261](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001002728261.png)

**ok了**

**然后配置BBR加速，这里我使用的是BBR-Plus**

**输入命令（你服务器之前实测不能用BBR，这里我魔改了一下脚本哈哈哈）**

```
 cd /usr/src && wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```

![image-20221001002955602](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001002955602.png)

**输入2开始安装脚本**

![image-20221001003023084](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001003023084.png)

**等待一会儿，安装成功后会提示你重启VPS，输入Y重启，重新连接服务器**

![image-20221001003253464](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001003253464.png)

**重启**

```
 cd /usr/src
 ./tcp.sh
```

**输入7使用BBR-Plus进行加速**

![image-20221001003526244](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001003526244.png)

![image-20221001003533620](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001003533620.png)

**加速成功**

**输入v2ray查看配置**

![image-20221001003612617](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001003612617.png)

**至此配置完成，速度相当不错，服务器也已经伪装成网站，可在浏览器输入购买的域名查看**

![image-20221001003725523](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/image-20221001003725523.png)

***——By 小赵先生***
