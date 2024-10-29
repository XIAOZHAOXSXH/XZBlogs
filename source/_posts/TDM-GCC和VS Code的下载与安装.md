---
title: TDM-GCC和VS Code的下载与安装
abbrlink: 82f5c987
date: 2024-01-16 21:32:30
---

# TDM-GCC和VS Code的下载与安装

## TDM-GCC的下载与安装

1. ### [点我跳转TDM-GCC官网](https://jmeubank.github.io/tdm-gcc/)

   ![image.png](/upload/image.png)
2. ### 下载最新Win64位版本

   ![image-bqby.png](/upload/image-bqby.png)
3. ### 双击运行安装程序

   ![image-bwsz.png](/upload/image-bwsz.png)


   > 点击Create安装GCC
   >

   ![image-ilpi.png](/upload/image-ilpi.png)

   > 安装64位版本
   >

   ![image-vsbu.png](/upload/image-vsbu.png)

   > 选择安装位置
   >

   ![image-yiia.png](/upload/image-yiia.png)

   > 可全选
   >

   > 点击Install进入安装进程
   >

   ![image-tasa.png](/upload/image-tasa.png)

   > 安装成功，Next
   >

   ![image-icif.png](/upload/image-icif.png)

   > 安装完成
   >
4. ### CMD输入`gcc --version`测试安装结果

   ![image-eoqv.png](/upload/image-eoqv.png)


   > 如图正确显示版本号视为安装成功！
   >

## VS Code的下载与安装

[点我跳转VS Code官网下载](https://code.visualstudio.com/)

![image-zkht.png](/upload/image-zkht.png)

> 建议下载Windows稳定版进行安装

> 安装过程不在赘述

![image-zczk.png](/upload/image-zczk.png)

> 安装成功如图

### 安装所需拓展插件

![image-ilua.png](/upload/image-ilua.png)

![image-rllt.png](/upload/image-rllt.png)

![image-giuv.png](/upload/image-giuv.png)

运行以下测试代码，如图则运行成功。

~~~c
#include <stdio.h>

int main()
{
    printf("Hello World!\n");
    return 0;
}

~~~
