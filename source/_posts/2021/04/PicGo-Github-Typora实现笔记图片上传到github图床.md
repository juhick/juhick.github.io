---
title: PicGo+Github+Typora实现笔记图片上传到github图床
date: 2021-04-03 21:26:29
tags:
 - 个人博客
 - 图床
categories:
math:
---

# PicGo+Github+Typora实现笔记图片上传到github图床

## GIthub新建仓库并获取Token

首先创建一个公有库，因为若是私有库的话在外部是访问不到图片的

![](https://raw.githubusercontent.com/juhick/picJuhick/master/2021/04/20210404000450.png)

<!-- more -->

点击用户里的设置

![](https://raw.githubusercontent.com/juhick/picJuhick/master/2021/04/20210404000600.png)

点击开发者选项

![](https://raw.githubusercontent.com/juhick/picJuhick/master/2021/04/20210404000643.png)

点击个人token

![](https://raw.githubusercontent.com/juhick/picJuhick/master/2021/04/20210404000755.png)

点击生成新的token

![](https://raw.githubusercontent.com/juhick/picJuhick/master/2021/04/20210404000836.png)

只需要选择repo选项即可

![](https://raw.githubusercontent.com/juhick/picJuhick/master/2021/04/20210404000905.png)

将token保存下来，因为只有这一次机会可以复制，不要错过！！！

![](https://raw.githubusercontent.com/juhick/picJuhick/master/2021/04/20210404000941.png)

## 设置PicGo

然后下载[PicGo](https://github.com/Molunerfinn/PicGo/releases)，选择适合自己的版本即可

安装完成后设置Github图床

![](https://raw.githubusercontent.com/juhick/picJuhick/master/2021/04/20210404001032.png)

然后就可以测试是否可以上传了，因为某些原因(你懂的)，直接上传可能会失败，可以配置代理，就不再赘述，自行摸索。

## 配置Typora

打开偏好设置

![](https://raw.githubusercontent.com/juhick/picJuhick/master/2021/04/20210404001441.png)

进行如下图所示的设置即可

![](https://raw.githubusercontent.com/juhick/picJuhick/master/2021/04/20210404001554.png)

~~但由于未知原因，我直接在Typora内插入时会失败，只能通过PicGo客户端上传，之后解决的话再修改~~

发现了是因为网的原因有的图片在本地删除了云端还保存着，将冗余图片删除即可

