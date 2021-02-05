---
title: 利用 GitHub + Hexo + Next 从零搭建一个博客
date: 2021-01-21 14:54:59
tags:
 - Hexo
 - Next
 - 个人博客
categories:
 - 博客
mathjax:
---

## Github 建库+配置SSH

> 因为要采用 github pages，所以先创建名为 “用户名.github.io” 的库

> 使用`ssh-keygen -t -rsa -C "电子邮箱"`生成SSH key
>
> 然后在用户主目录下会生成 .ssh 文件夹，将其中 .pub 后缀的内容添加到 Github 即可

<!-- more -->

## 安装 Node.js

推荐安装长期支持版本

直接去[官网](https://nodejs.org/zh-cn/)下载即可

## 切换国内源

`npm config set registry http://registry.npm.taobao.org`

使用该命令即可切换为国内源，提升下载速度

## 安装 Hexo

使用 `npm install -g hexo-cli`命令即可安装

## 初始化项目

首先使用 hexo 命令行在本地创建一个项目，先在本地可以跑通

`hexo init {name}`

name 即为我们的项目名，根据自己需要修改

执行结束后就会在当前目录下生成 Hexo 的初始化文件，如下图所示

![image-20210121150123443](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121150123443.png)

我们暂且不用关心这些文件都是什么，先整个跑起来观察一下

在该目录下使用`hexo generate`即可将 Hexo 编译为 html 代码

该命令就会生成如下文件，包含了 js、css、font 等内容，保存在项目根目录的 public 文件夹下

![image-20210121100347546](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121100347546.png)

 然后我们利用 Hexo 提供的 server 命令把博客在本地运行起来

命令为`hexo server`

![image-20210121100529862](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121100529862.png)

运行后 Hexo 就会运行在本地 4000 端口上了，可以打开看一下，如下图所示

![image-20210121100629375](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121100629375.png)

这样我们仅仅使用了三个命令就搭建出了一个博客的骨架

## 部署

然后我们对这个初始化的项目进行一下部署，放到 Github Pages 上面验证一下其可用性。成功之后再进行后续的修改，这样可以防止我们花费大量时间搭建之后发现不能部署，再重新整就会浪费时间

在部署之前，我们需要设置一下项目的部署地址

打开项目目录下的 _config.yml 文件，找到 Deployment 这个地方，把刚才新建的 Repository 的地址贴过来，然后指定分支为 master 分支

修改完成如下图所示：

![image-20210121101102281](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121101102281.png)

仓库要设置为自己的仓库地址

另外我们还需要额外安装一个支持 Git 的部署插件，名字叫做 hexo-deployer-git

在项目目录下执行安装命令 `npm install hexo-deployer-git --save`

安装完成后，运行部署命令`hexo deploy`

![image-20210121101451628](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121101451628.png)

出现该提示即部署成功了，这时候我们访问一下 GitHub Repository 同名的链接，比如我的 https://juhick.github.io ，这时候我们就可以看到跟本地一模一样的博客内容了

我们打开 Github 就可以看到如下内容

![image-20210121102223531](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121102223531.png)

这实际上是博客文件夹下面的 public 文件夹下的所有内容，Hexo 把编译之后的静态页面内容上传到 GitHub 的 master 分支上面去了。

因为我们编译之后的网页使用了 master 分支，那么我们源码则需要放到另一个分支，上传命令如下：

```bash
git init
git checkout -b source
git add -A
git commit -m "init blog"
git remote add origin git@github.com:{username}/{username}.github.io.git
git push origin source
```

这里就是新建了一个 source 分支来存放源码

成功之后，可以到 GitHub 上再切换下默认分支，比如我就把默认的分支设置为了 source，当然不换也可以。

## 配置站点信息

> 完成如上内容之后，实际上我们只完成了博客搭建的一小步，因为我们仅仅是把初始化的页面部署成功了，博客里面还没有设置任何有效的信息。下面就让我们来进行一下博客的基本配置，另外换一个好看的主题，配置一些其他的内容，让博客真正变成属于我们自己的博客吧。

 修改根目录下的 _config.yml 文件，找到 Site 区域，这里面可以配置站点标题 title、副标题 subtitle 等内容、关键字 keywords 等内容，比如我的就修改为如下内容：

![image-20210121102933951](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121102933951.png)

这里参照格式修改为自己的，这样就完成了基本信息的配置，完成之后可以看到信息已经修改

![image-20210121103117191](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121103117191.png)

## 修改主题

> 目前来看，整个页面的样式个人感觉并不是那么好看，想换一个风格，这就涉及到主题的配置了。目前 Hexo 里面应用最多的主题基本就是 Next 主题了，个人感觉这个主题还是挺好看的，另外它支持的插件和功能也极为丰富，配置了这个主题，我们的博客可以支持更多的扩展功能，比如阅览进度条、中英文空格排版、图片懒加载等等。

下边就来安装 Next 这个主题，主题的 [Github 地址](https://github.com/theme-next/hexo-theme-next)，我们可以直接把 master 分支 clone 下来。在项目根目录下执行如下命令

`git clone https://github.com/theme-next/hexo-theme-next themes/next`

然后我们需要修改下博客所用的主题名称，修改项目根目录下的 _config.yml 文件，找到 theme 字段，修改为 next 即可，修改如下：

![image-20210121103756633](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121103756633.png)

然后重新开启本地服务，就可以看到 next 主题切换成功了

![image-20210121104338586](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121104338586.png)

## 主题配置

> 现在我们已经成功切换到 next 主题上面了，接下来我们就对主题进行进一步地详细配置吧，比如修改样式、增加其他各项功能的支持，下面逐项道来。 Next 主题内部也提供了一个配置文件，名字同样叫做 _config.yml，只不过位置不一样，它在 themes/next 文件夹下，Next 主题里面所有的功能都可以通过这个配置文件来控制，下文所述的内容都是修改的 themes/next/_config.yml 文件。

### 样式

Next 主题还提供了多种样式，风格都是类似黑白的搭配，但整个布局位置不太一样，通过修改配置文件的 scheme 字段即可

这里选用了 Pisces 样式

![image-20210121104933472](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121104933472.png)

刷新之后可以看到对应的页面

![image-20210121104953906](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121104953906.png)

### favico

favicon 就是站点标签栏的小图标，默认是用的 Hexo 的小图标，如果我们有站点 Logo 的图片的话，我们可以自己定制小图标。 但这并不意味着我们需要自己用 PS 自己来设计，已经有一个网站可以直接将图片转化为站点小图标，站点链接为：[https://realfavicongenerator.net](https://realfavicongenerator.net/)，到这里上传一张图，便可以直接打包下载各种尺寸和适配不同设备的小图标。 图标下载下来之后把它放在 themes/next/source/images 目录下面。 然后在配置文件里面找到 favicon 配置项，把一些相关路径配置进去即可，示例如下：

![image-20210121105631632](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121105631632.png)

配置完成之后刷新页面，整个页面的标签图标就被更新了。

### avatar

avatar 这个就类似站点的头像，如果设置了这个，会在站点的作者信息旁边额外显示一个头像

比如我有一张 avatar.png 图片如下：

![avatar](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/avatar.png)

将其放置到 themes/next/source/images/avatar.png 路径，然后在主题 _config.yml 文件下编辑 avatar 的配置，修改为正确的路径即可。

![image-20210121110050335](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121110050335.png)

这里有 rounded 选项是是否显示圆形，rotated 是是否带有旋转效果，大家可以根据喜好选择是否开启。

效果如下：

![image-20210121110117823](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121110117823.png)

### rss

博客一般是需要 RSS 订阅的，如果要开启 RSS 订阅，这里需要安装一个插件，叫做 hexo-generator-feed，安装完成之后，站点会自动生成 RSS Feed 文件，安装命令如下：

`npm install hexo-generator-feed --save`

在项目根目录下运行这个命令，安装完成之后不需要其他的配置，以后每次编译生成站点的时候就会自动生成 RSS Feed 文件了。

### code

作为程序猿，代码块的显示还是需要很讲究的

可以在配置文件中的 codeblock 区块下设置自己喜欢的样式

![image-20210121110623804](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121110623804.png)

### back2top

字面意思，回到顶部，可以在 back2top 区块设置

![image-20210121110756732](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121110756732.png)

依次为是否启用，是否在侧边栏显示按钮，是否显示阅读百分比

### reading_progress

阅读进度，在页面顶端的黑条

![image-20210121111022793](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121111022793.png)

可以在 reading_progress 区块设置是否开启，以及设置喜欢的样式

![image-20210121111107432](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121111107432.png)

### bookmark

书签，可以根据阅读历史记录，在下次打开页面的时候快速帮助我们定位到上次的位置，大家可以根据喜好开启和关闭

配置如下：

![image-20210121111313077](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121111313077.png)

### github_banner

在一些技术博客上，大家可能注意到在页面的右上角有个 GitHub 图标，点击之后可以跳转到其源码页面，可以为 GitHub Repository 引流，大家如果想显示的话可以自行选择打开

配置如下：

![image-20210121111504461](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121111504461.png)

根据需要自行修改，效果如下：

​	![image-20210121150225357](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121150225357.png)

### gittalk

> 由于 Hexo 的博客是静态博客，而且也没有连接数据库的功能，所以它的评论功能是不能自行集成的，但可以集成第三方的服务。 Next 主题里面提供了多种评论插件的集成，有 changyan | disqus | disqusjs | facebook_comments_plugin | gitalk | livere | valine | vkontakte 这些。 作为一名程序员，我个人比较喜欢 gitalk，它是利用 GitHub 的 Issue 来当评论，样式也比较不错。 首先需要在 GitHub 上面注册一个 OAuth Application，链接为：https://github.com/settings/applications/new，注册完毕之后拿到 Client ID、Client Secret 就可以了。 首先需要在 _config.yml 文件的 comments 区域配置使用 gitalk

![image-20210121112405020](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121112405020.png)

主要是 comments.active 字段选择对应的名称即可。 然后找到 gitalk 配置，添加它的各项配置：

![image-20210121112534324](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121112534324.png)

配置完成之后 gitalk 就可以使用了，点击进入文章页面，就会出现如下页面：

![image-20210121112722998](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121112722998.png)

GitHub 授权登录之后就可以使用了，评论的内容会自动出现在 Issue 里面。

### pangu

> 我个人有个强迫症，那就是写中文和英文的时候中间必须要留有间距，一个简单直接的方法就是中间加个空格，但某些情况下可能习惯性不加或者忘记加了，这就导致中英文混排并不是那么美观。 pangu 就是来解决这个问题的，我们只需要在主题里面开启这个选项，在编译生成页面的时候，中英文之间就会自动添加空格，看起来更加美观。 具体的修改如下：

![image-20210121112813035](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121112813035.png)

### math

> 可能在一些情况下我们需要写一个公式，比如演示一个算法推导过程，MarkDown 是支持公式显示的，Hexo 的 Next 主题同样是支持的。 Next 主题提供了两个渲染引擎，分别是 mathjax 和 katex，后者相对前者来说渲染速度更快，而且不需要 JavaScript 的额外支持，但后者支持的功能现在还不如前者丰富，具体的对比可以看官方文档：https://theme-next.org/docs/third-party-services/math-equations。 所以我这里选择了 mathjax，通过修改配置即可启用：

![image-20210121112952638](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121112952638.png)

> mathjax 的使用需要我们额外安装一个插件，叫做 hexo-renderer-kramed，另外也可以安装 hexo-renderer-pandoc，命令如下：

```
npm un hexo-renderer-marked --save
npm i hexo-renderer-kramed --save
```

另外还有其他的插件支持，大家可以到官方文档查看。

### pjax

> 可能大家听说过 Ajax，没听说过 pjax，这个技术实际上就是利用 Ajax 技术实现了局部页面刷新，既可以实现 URL 的更换，有可以做到无刷新加载。 要开启这个功能需要先将 pjax 功能开启，然后安装对应的 pjax 依赖库，首先修改 _config.yml 修改如下：

![image-20210121113222284](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121113222284.png)

然后安装依赖库，切换到 next 主题下，然后安装依赖库：

```
cd themes/next
git clone https://github.com/theme-next/theme-next-pjax source/lib/pjax
```

这样 pjax 就开启了，页面就可以实现无刷新加载了。 另外关于 Next 主题的设置还有挺多的，这里就介绍到这里了，更多的主题设置大家可以参考官方文档：https://theme-next.org/docs/。

## 文章

如何添加文章呢？只需要使用 Hexo 提供的 new 命令即可，比如要新建一篇 [hello world] 的文章，命令如下：

`hexo new hello-world`

创建的文章会出现在 `source/_posts` 文件夹下，是 MarkDown 格式。 在文章开头通过如下格式添加必要信息：

![image-20210121113650483](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121113650483.png)

开头下方撰写正文，MarkDown 格式书写即可。 这样在下次编译的时候就会自动识别标题、时间、类别等等，另外还有其他的一些参数设置，可以参考文档：https://hexo.io/zh-cn/docs/writing.html。

## 标签页

现在我们的博客只有首页、文章页，如果我们想要增加标签页，可以自行添加，这里 Hexo 也为我们提供了这个功能，在根目录下执行命令：

`hexo new page tags`

执行这个命令之后会自动帮我们生成一个 source/tags/index.md 文件，内容就只有这样子的：

![image-20210121114628693](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121114628693.png)

我们可以自行添加一个 type 字段来指定页面的类型:

![image-20210121114810036](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121114810036.png)

然后再在主题的 _config.yml 文件将这个页面的链接添加到主菜单里面，修改 menu 字段如下：

![image-20210121115217009](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121115217009.png)

这样重新本地启动看下页面状态，效果如下：

![image-20210121115311153](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121115311153.png)

## 分类页

分类功能和标签类似，一个文章可以对应某个分类，如果要增加分类页面可以使用如下命令创建：

`hexo new page categories`

然后同样地，会生成一个 source/categories/index.md 文件。 我们可以自行添加一个 type 字段来指定页面的类型：

![image-20210121115650753](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121115650753.png)

然后再在主题的 _config.yml 文件将这个页面的链接添加到主菜单里面，修改 menu 字段如下：

![image-20210121115927053](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121115927053.png)

这样页面就会增加分类的支持，效果如下：

![image-20210121120257495](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121120257495.png)

## 搜索页

很多情况下我们需要搜索全站的内容，所以一个搜索功能的支持也是很有必要的。 如果要添加搜索的支持，需要先安装一个插件，叫做 hexo-generator-searchdb，命令如下：

`npm install hexo-generator-searchdb --save`

然后在项目的 _config.yml 里面添加搜索设置如下：

![image-20210121120626168](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121120626168.png)

然后在主题的 _config.yml 里面修改如下：

![image-20210121120709093](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121120709093.png)

这里用的是 Local Search，如果想启用其他是 Search Service 的话可以参考官方文档：https://theme-next.org/docs/third-party-services/search-services。

## 404页面

另外还需要添加一个 404 页面，直接在根目录 source 文件夹新建一个 404.md 文件即可，内容可以仿照如下：

![image-20210121120956169](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121120956169.png)

这里面的一些相关信息和链接可以替换成自己的。 增加了这个 404 页面之后就可以 完成了上面的配置基本就完成了大半了，其实 Hexo 还有很多很多功能，这里就介绍不过来了，大家可以直接参考官方文档：https://hexo.io/zh-cn/docs/ 查看更多的配置。

## 自定义域名

将页面修改之后可以用上面的脚本重新部署下博客，其内容便会跟着更新。 另外我们也可以在 GitHub 的 Repository 里面设置域名，找到 Settings，拉到下面，可以看到有个 GitHub Pages 的配置项，如图所示：

![image-20210121121114011](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121121114011.png)

下面有个 custom domain 的选项，输入你想自定义的域名地址，然后添加 CNAME 解析就好了。 另外下面还有一个 Enforce HTTPS 的选项，GitHub Pages 会在我们配置自定义域名之后自动帮我们配置 HTTPS 服务。刚配置完自定义域名的时候可能这个选项是不可用的，一段时间后等到其可以勾选了，直接勾选即可，这样整个博客就会变成 HTTPS 的协议的了。 另外有一个值得注意的地方，如果配置了自定义域名，在目前的情况下，每次部署的时候这个自定义域名的设置是会被自动清除的。所以为了避免这个情况，我们需要在项目目录下面新建一个 CNAME 文件，路径为 source/CNAME，内容就是自定义域名。 比如我就在 source 目录下新建了一个 CNAME 文件，内容为：

![image-20210121121223956](../images/%E5%88%A9%E7%94%A8-GitHub-Hexo-Next-%E4%BB%8E%E9%9B%B6%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%8D%9A%E5%AE%A2/image-20210121121223956.png)

这样避免了每次部署的时候自定义域名被清除的情况了。 以上就是从零搭建一个 Hexo 博客的流程，希望对大家有帮助。