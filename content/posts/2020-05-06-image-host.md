---
title: "快捷三式 - Markdown 图床工作流"
date: 2020-05-06T07:19:42+08:00
categories: 智能操作流
tags:
  - 技术
  - win10
  - 图床
mathjax: false
codes: []
comments: true
vertical: false
---

## 绪论

如何在文档中高效的插入图片是每个 Markdown 写作者都要面对的问题。如果每张图片都手动上传云端图床，然后再 Markdown 文件中手动输入，就实在太慢了。实际工作中，博文里需要配图时通常需要的是窗口截图，这让图片插入工作又加了一道工序。可以说，对于一位 Markdown 写作者来说，图片工作流无非就是 `截图 + 上传` 两大流程，具体工序拆解如下：

1. 截图并对其进行标注及保存
2. 截图后自动上传到预先设定的图床工具
3. 上传后自动复制 Markdown 格式的图片链接到剪贴板

在 Windows 平台上本人摸索出了一套适合自己的图床工作流，使用 `ShareX + PicGo + 腾讯云COS` 三大利器配合快捷键完成「高效截图+快速上传」功能。

- [ShareX](https://getsharex.com/)：一款支持屏幕截图及文件共享的生产力工具，能满足各种截图操作，并支持截图的标注及上传，但目前上传路径多为外国站点，自定义上传目的地中对腾讯云 COS 的支持较为复杂，不够友好。所以很遗憾，我们还需要借助其他工具。
- [PicGO](https://molunerfinn.com/PicGo/)：一个用于快速上传图片并获取图片 URL 链接的工具，基本支持国内各大图床网站，专精上传功能。
- [腾讯云 COS](https://cloud.tencent.com/document/product/436/11366)：对象存储（Cloud Object Storage，COS）是腾讯云提供的一种存储海量文件的分布式存储服务，支持各种文件的云端存取，作为图床绰绰有余。

## ShareX：全能截图工具

### 全面化 - 各种截图我都行

ShareX 这款软件，有全面化和自动化两大特点，在「截图」部分中，从整个屏幕截图，到滚动截图，再到自动截图，ShareX 都能做到，甚至还支持屏幕录制以及 GIF 录制。

Share 还有一个杀手锏功能，就是分区域截图。有的时候你需要截取托盘内弹出来的几个窗口，但是截图的时候免不了会多处那么几个多余的部分，影响美观。ShareX 拥有多区域截图功能，那些你不需要的区域，可以直接变成透明。

首先，你需要启用这个功能：右键点击 ShareX 图标，点击「任务设置」，在左边「区域截图」中勾选使用多区域模式即可。截图效果如下图所示：

<img src="https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200506105828.png" alt="20200506105828" style="zoom:50%;" />

### 自动化 - 从截图到处理的无缝体验

ShareX 截图好之后，会直接将原图传递到内置的图像编辑器。在这里，你可以对你的截图做各种各样的操作，包括 **标注，添加文字，添加马赛克** 等等。

例如，在 ShareX 的右键菜单中的「截图后」菜单打开「打开图像编辑器」选项。这样，截图完成后，ShareX 就会立即将截图传递到截图编辑器。这样就可以把图像编辑完成后直接保存。

<img src="https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200506110421.png" alt="20200506110421" style="zoom:50%;" />

### 截图操作工作流

我自身设置的 ShareX 的工作流是这样的：

1. 按下快捷键 `Ctrl + Shift + X` 快速截图
2. 根据事先设定的「截图后」菜单中选中需要的功能，ShareX 自动添加水印、并自动保存到我设定的文件夹下
3. 将处理过后的图片复制到剪贴板

## PicGO：图床上传神器

PicGO 是一个开源软件，项目地址：https://github.com/Molunerfinn/PicGo/releases，支持多个图床，包括 sm.ms 图床、七牛图床、GitHub 图床、又拍云图床等：

<img src="https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200506114514.png" alt="20200506114514" style="zoom:50%;" />

同时，**PicGO 支持直接上传剪贴板中复制的图片**，正因为有这个便捷的功能，使得 PicGo 能够和 ShareX 无缝对接。

### 设置腾讯云 COS

首先要在图床设置中加入你自己的存储桶，用于存放图片：

<img src="https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200506123727.png" alt="20200506123727" style="zoom:50%;" />

至于上图中各个参数的获取，在下文对腾讯云 COS 的介绍中会涉及，在此不作赘述。

### 设置上传快捷键

点击 PicGO 设置 - 修改上传快捷键即可进行快捷键的修改，按照自己的习惯进行设置即可，我设置的是 `Alt + P`。

### 设置返回链接格式

PicGo 提供了几种常用的链接返回格式，同时也支持自定义上传成功后返回的图片链接格式：

<img src="https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200506115438.png" alt="20200506115438" style="zoom:50%;" />

点击 **PicGo 设置** - **自定义链接格式** - 点击设置：

<img src="https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200506115732.png" alt="20200506115732" style="zoom: 50%;" />

至此，PicGo 也设置完毕了，需要上传图片时，只需按下设定的快捷键即可。当然， PicGo 还有不少实用的小功能，比如上传前重命名文件之类的，还可以安装一些插件来添加一些功能，有兴趣的可以自行探索。

## COS：文件存储利器

需要注意的是，COS 有 v4 和 v5 两个版本，两个版本的认证签名、上传 url 等都 **完！全！不！同！**，不过不用担心，在 PicGo 中可以一键切换 v4\v5 版本，两者配置几乎一致。

<img src="https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200506123727.png" alt="20200506123727" style="zoom:50%;" />

1. 获取你的 APPID、SecretId 和 SecretKey

访问：https://console.cloud.tencent.com/cam/capi

![20200506121114](https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200506121114.png)

2. 获取 bucket 名以及存储区域代号

访问：https://console.cloud.tencent.com/cos5/bucket，创建一个存储桶。然后找到你的存储桶名和存储区域代号：

![20200506121215](https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200506121215.png)

然后记得点击 `设为默认图床`，这样上传才会默认走的是腾讯云 COS。

## 快捷三式

现在，ShareX 和 PicGO 都已经设置好，两者各设置了一个快捷键，当我写文章需要截图并插入时，只需要进行「快捷三式」：

1. 按下快捷键 **Ctrl + Shift + X**，对截取的图片进行标注，然后回车
2. 按下快捷键 **Alt + P**，PicGo 将会自动把 ShareX 截图后复制在剪贴板的图片上传到腾讯云 COS，然后将返回的图片 url 链接复制在剪贴板
3. 在需要插入图片的位置，按下快捷键 **Ctrl + V** ，粘贴剪贴板里的图片链接

三个快捷键高效搞定 Markdown 写作插入图片。

> Tips：如果网站上图片的显示过大，想缩小一些，有没有什么简便的方法呢？
>
> - **Typora** 编辑器可以直接对插入的图像进行缩放处理，方便快捷，谁用谁知道！

![20200506120626](https://image-host-1255524710.cos.ap-beijing.myqcloud.com/20200506120626.png)
