---
title: hexo+typora 插入图片的简便解决方案
date: 2021-01-21 15:04:00
tags:
 - Hexo
 - 插入图片
categories:
mathjax:
typora-root-url: ../
---

因为我不想使用图床来存图片，所以直接将图片存放到了source文件夹下

在 source 文件夹下新建 imadges 文件夹

然后在 Typora 偏好设置的图片选项下将路径设置如下

![image-20210121153204959](images/hexo-typora-%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87%E7%9A%84%E7%AE%80%E4%BE%BF%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88/image-20210121153204959.png)

同时，再文章头部添加`typora-root-url: ../`即可(发布时需要把图片路径前边的../去掉)

则每次插入图片会自动复制到 images 文件夹下，就可以实现方便的插入图片了