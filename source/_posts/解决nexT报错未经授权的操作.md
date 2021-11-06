---
title: 解决nexT报错未经授权的操作
date: 2021-01-27 11:13:08
tags: [前端知识, bug折腾]
---

**2021/9/23更新**：因为valine的安全问题，hexo官方已经移除了对valine的支持。

昨天将我的小博客从typecho搬到了hexo，在使用`hexo-theme-next`主题后，发现在nexT所有的配置文件里面都找不到valine评论系统的配置项，然后去valine官网看了下，valine官网是支持nexT主题的，再去nexT官网时，就发现了`hexo-theme-next`的[valine插件](https://github.com/next-theme/hexo-next-valine)，照着这里面的教程做了一遍后，博客文章的评论系统算是加载出来了，但是401报错，如下图 Code 401: 未经授权的操作，请检查你的AppId和AppKey

<!-- more -->

![未经授权的操作请检查你的AppId和AppKey](Code401未经授权的操作请检查你的AppId和AppKey.png)

开始我以为就是我appId或者appKey填错了，结果我仔细复制了几次都没有成功，接下来说说解决办法，这个方法是参考了[CSDN 解决Valine评论系统Code : undefined [410 GET https://avoscloud.com/1.1/classes/Comment]问题](https://blog.csdn.net/weixin_43743997/article/details/104713252)，我将配置文件中的`serverURLs`修改为我自己绑定的域名：https://comments.ieleven.xyz 或者这个默认的域名：https://avoscloud.com就解决了问题，如果你的valine也出现了 Code 401: 未经授权的操作，请检查你的AppId和AppKey，不妨试试。

![修改配置文件serverURLs配置项](修改配置文件serverURLs配置项.png)

