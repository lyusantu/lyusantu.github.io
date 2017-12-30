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

首先没有账户的话可以先进行账户的注册,有账户的可以跳过这一步

#### 注册Github账户

打开Github官网 [https://github.com/](https://github.com/), 注册一个账户

![](http://oih7sazbd.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20171230205340.jpg)

如上图,红框内`username`对应你的昵称,`Email`和`Password`则作为你的登录账户,填写完毕并在无重复情况下点击绿色按钮`Sign up for Github`就可以进入到下一步.

![](http://oih7sazbd.bkt.clouddn.com/STEP2.jpg)

如上图,如果没有特别的需要,直接点击下方绿色按钮`Continue`继续操作.

![](http://oih7sazbd.bkt.clouddn.com/step3.jpg)

如上图,进入到了一个类似于问卷调查的页面,可以选择填写并点击下方绿色按钮`Submit`进行提交,或直接点击下方绿色按钮`submit`旁的`skip this step`跳过这一步骤.

接下来,需要激活我们的账户才可以操作我们的`Github`. 进入到注册账户时在第一个页面输入的邮箱收件箱里,打开`Github`发送来的邮件,点击邮件内容中的`Verify email address`进行邮箱的验证,验证成功后,就可以正式进入我们的主题.

#### 创建仓库
 
