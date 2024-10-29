---
title: 【MT3000玩法分享】Alist+DDNSTO搭建个人随身影库
abbrlink: 39ad2b17
date: 2024-01-16 19:13:54
---

# 【MT3000玩法分享】Alist+DDNSTO搭建个人随身影库

支持**本地存储、阿里云盘、115网盘、夸克网盘、OneDrive、Google Drive** 等

今天折腾了一下配合ddnsto实现外网访问功能

以下以**阿里云盘**为例

首先你需要在固件里安装**Alist和DDNSTO**

如果不想折腾可以刷我固件

[**【2023.2.8更新】自编译MT3000固件_官方UI_argon主题 - MT3000,MT2500,MV1000 - forum.gl-inet.cn - Powered by Discuz!**](https://forum.gl-inet.cn/forum.php?mod=viewthread&tid=2782)

首先进入luci界面，找到**Alist**

勾选**启用并点击读取获取初始账号密码**

我固件初始账号和密码都为**admin**

点击右下角**保存并应用**

会出现以下按钮

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/193233lvoi1zphzrrr15vv.png)

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/193353zgl1e58i8gv97l1l.png)

点击打开WEB界面

会进入Alist的WEB界面

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/193459hwteutkaj51ivsia.png)

初次进入不会有列表，我这里是已经挂载后

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/193608kg2bx1bp6ujgjn6x.png)

点击登录

输入刚才读取的初始账号密码

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/193655fnnp16tpnvhf86nv.png)

登录后点击管理进行挂载

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/193941xwi56xuydrwsish5.png)

可以在这里更改初始账号密码及启用双重验证

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/194103ce5vduudvzfjo3ev.png)

点击添加开始挂载网盘

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/194217nojb751jb1td1ata.png)

驱动我这里选择阿里云盘，请根据需要自行选择

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/194316c3nbbkb7j7ccbbc2.png)

挂载路径自己起名字，开启web代理

打开阿里云盘网页端获取刷新令牌

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/194432t3acnrv9nv5n3cvp.png)

复制这里refresh_token粘贴到刷新令牌处

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/194555mctroscs5tae8t55.png)

打开秒传和内部上传

点击添加

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/194634sz66dneekheh768k.png)

状态显示work即为挂载成功

可以访问主页看看效果

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/194754pmchcu4dyotaa2yo.png)

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/194802pzllml1l5z1lixrq.png)

可以在线观看，65G的4k片源也无压力

接下来返回luci界面进行DDNSTO配置

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/194944fwq9u3xog6wwqwxu.png)

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/194947gonco8t2ppxsr8yu.png)

勾选启用点击保存并应用

前往DDNSTO控制台获取Token

https://www.ddnsto.com/app/#/login

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/195121kbepblb9x92xawpa.png)

复制这个返回luci粘贴，点击保存并应用，第一次登录控制台会提示你添加设备，按教程操作即可

![img](https://myblogs-1304241272.cos.ap-hongkong.myqcloud.com/images/195238jxm4kxvk9baaew19.png)

控制台中点击添加域名映射，此处自定义即可

稍等一分钟就可用域名进行外网访问

--End--
