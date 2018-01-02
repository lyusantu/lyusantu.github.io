---
layout: post
title: "利用Github Pages建立个人博客"
description: "傻瓜式个人博客搭建教程"
date: 2017-12-29
tags: [github Pages, jekyll, blog]
comments: true
share: true
---

## 前言

之前的博客折腾了许久,现在比较渴望一种简单快捷的方式.

写博客的目的很简单,并非为了分享,只是希望在这一过程中能够获取到更好的表达能力,也是锻炼自己不断进步的一个过程.

对于博客,没有特别想要花钱的欲望,所以决定将这个博客部署在Github Pages上,参考别人的教程,吸取经验,在这里也分享一下这一过程.

## 了解Github

GitHub是一个面向开源及私有软件项目的托管平台,  因为只支持git作为唯一的版本库格式进行托管, 故名gitHub. 通俗点来说,我们可以把我们想要存储的文件都放到Github中,同时我们也可以将我们存储到Github中的文件进行共享.

## 了解Github Pages

Github Pages的初衷是为了给开发者提供一个私人页面,可以用做个人主页,进行介绍或分享自己的idea,听起来很无私,所以这就是我选择Github Pages的理由咯. 其实还是因为免费且没有流量限制 <del> (穷) </del>..

## 使用Github

首先没有账户的话可以先进行账户的注册,有账户的可以跳过`注册Github账户`这一步

## 注册Github账户

打开Github官网 [https://github.com/](https://github.com/), 注册一个账户

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20171230205340.jpg)

如上图,红框内`username`对应你的昵称,`Email`和`Password`则作为你的登录账户,填写完毕并在无重复情况下点击绿色按钮`Sign up for Github`就可以进入到下一步.

![](http://oih7sazbd.bkt.clouddn.com/STEP2.jpg)

如上图,如果没有特别的需要,直接点击下方绿色按钮`Continue`继续操作.

![](http://oih7sazbd.bkt.clouddn.com/step3.jpg)

如上图,进入到了一个类似于问卷调查的页面,可以选择填写并点击下方绿色按钮`Submit`进行提交,或直接点击下方绿色按钮`submit`旁的`skip this step`跳过这一步骤.

接下来,需要激活我们的账户才可以操作我们的`Github`. 进入到注册账户时在第一个页面输入的邮箱收件箱里,打开`Github`发送来的邮件,点击邮件内容中的`Verify email address`进行邮箱的验证,验证成功后,就可以正式进入我们的主题.

## 创建仓库

进入创建仓库的页面 [https://github.com/new](https://github.com/new),这个仓库将作为我们存放博客内容的地方.

![](http://oih7sazbd.bkt.clouddn.com/createrepo.png)

如上图,这是我们用来创建仓库的页面. 需要注意的地方就是`Repository name`这一栏以及下面的选择框`Initialize this repository with a READEME`,其它选项都是可选的,可根据个人需要进行选择,此处不做介绍. `Repository name`这一栏是有格式要求的,不能乱取名,格式应为`用户名.github.io`,此处我们取名为`none168.github.io`,同时我们需要勾选`Initialize this repository with a READEME`这一栏,完成后点击下面的绿色按钮`Create repository`就成功创建了仓库,创建成功后会直接进入仓库页面.

## 设置仓库

![](http://oih7sazbd.bkt.clouddn.com/settings.png)

如上图,这是我们创建好的仓库页面. 我们需要设置我们的`Github Pages`,点击图中红框标记的`settings`进入设置页面.

![](http://oih7sazbd.bkt.clouddn.com/gitpages.png)

如上图,进入设置页面后,将页面向下拖动至`Github Pages`选项,然后点击图中红框标注的按钮`Choose a theme`进入选择主题页面.

![](http://oih7sazbd.bkt.clouddn.com/selecttheme.png)

如上图,这是选择主题的默认页面,因为等下需要进行页面的选取,所以在此处直接点击图片红框标记的按钮`Select theme`选择这个默认主题即可.

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180102170605.png)

如上图,选择主题成功后,顶部会提示主题已设置为`XX`,同时下面会出现两个需要我们填写的文件,分别是标题及内容,此处默认即可,直接点击下方红框内标注的绿色按钮`Commit changes`进行提交即可.

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180102170803.png)

如上图,此时会发现比刚创建成功的仓库多了一个`_config.yml`的配置文件,这个时候,我们的页面就已经成型了.

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180102171150.png)

如上图,我们可以通过访问`用户名.github.io`预览页面,确定可以显示出页面后,就可以正式进入主题,进行我们的博客搭建.

## 工具安装

接下来,就需要用到我们的工具了. 此处以`windows`系统举例,首先在[https://desktop.github.com/](https://desktop.github.com/)下载`github`桌面版,下载成功后自行安装.

打开`github desktop`应用程序,登录你的`github`账户,就可以继续我们接下来的步骤.

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180102172524.png)

如上图,登录成功后,我们点击左上角的`File`,在下拉框中选择`Clone repository`来进行仓库的克隆.

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180102172612.png)

如上图,点击`Clone repository`会打开当前窗口,我们需要点击我们需要克隆的项目,然后在下方第二个红框内选择要克隆到哪里,成功选择目录后,点击下面的蓝色按钮`Clone`进行仓库的克隆,然后等待克隆完成即可.

## 选择主题

在仓库克隆的这个阶段,我们就可以选择我们的博客风格.前段技术良好可以考虑自己参与主题的建设,前端技术不怎么在行,则不建议主题更换的太频繁,我们可以进入到`jekyll`主题选择界面进行选择喜欢的主题:

[http://jekyllthemes.org/](http://jekyllthemes.org/)

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180102173346.png)

如上图,这是网站一开始进来的页面,我们就以红框内标注的第一套模板作为案例,点击进入这套模板

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180102173411.png)

如上图,这是模板点击进来的页面,我们直接点击图中红框标注的`download`按钮进行主题的下载即可.

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180102174020.png)

如上图,进入之前我们克隆的仓库的文件夹,将仓库内的所有文件全部删除(除了隐藏文件夹`.git`),然后把下载出来的主题压缩包进行解压,将解压出来的文件夹内的所有文件,全部剪切到我们的仓库里.

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180102174203.png)

如上图,简单介绍一下整个主题的结构.
- includes: 这个文件夹里的内容作为一种通用的内容会应用到博客的每一个页面.
- layouts: 存放每个页面的设计,一般包含default.html和posts.html.
- posts: 存储博文,语法统一采用`markdown`.
- config.xml: 博客的基本配置信息,包含博客名称、博主基本信息等.

## 博客的上传

 完成上面的步骤,我们的博客基本已经配置完毕,只需要根据个人需要修改对应的内容即可. 如果需要编写自己的博文,则创建`.md`文件并以`markdown`语法编辑并保存到`_posts`目录下即可. 想要同步到`github`上,就需要用到我们的`github desktop`应用程序了.

 ![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180102174839.png)

 如上图,在更新过我们的仓库文件后,打开`github desktop`,会发现左侧的`Changes`栏目下多了一些内容,包含几个文件进行了更改并展示出了这些文件,点击这些文件,则可以在右侧看到更改的内容. 想要将这些变动提交到`github`中,我们只需要在左侧的下面输入`Summary`描述后,点击蓝色按钮`Commit to master`即可提交我们的更改,提交成功后,点击顶部的`Fetch origin`进行`push`即可将我们的更改同步至`github`中.

 ## END
