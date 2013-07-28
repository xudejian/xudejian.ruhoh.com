---
title: 使用travis持续集成github blog pages
date: '2013-07-28'
author: Dejian Xu
description: 使用travis持续集成github blog pages
categories: 'CI/travis'
tags:
  - 持续集成
  - CI
  - travis
  - github
---

## 想要一个GitHub Blog吗？

你可以将博客发布在github的主机上。

还没有github的blog页面吗？Github有详细的文档，请看这里
[https://help.github.com/articles/what-are-github-pages]
(https://help.github.com/articles/what-are-github-pages)
，和jekyll的[Step By Step](http://jekyllbootstrap.com/)

jekyll的作者推荐你尝试一下[ruhoh](http://ruhoh.com/)，
您看到的这个blog就是用ruhoh生成的。

github的默认支持jekyll，对ruhoh不支持，但是TA支持纯静态页面，
所以可以使用任意的框架，生成blog的静态页面，然后部署到github上。

如果你使用jekyll的话，每次文件到github库，github会为您自动部署，
不需要您自己设置什么。

## 使用ruhoh来发布你的blog

使用ruhoh来生成你的blog非常简单，如果你还没看过ruhoh的说明，
请先移步[ruhoh](http://ruhoh.com/)。

当手工编译blog后，将compiled文件夹下的文件，
发布到`YOUR-GITHUB-USERNAME.github.io`这个库，然后你的blog就建立好了。

## 持续集成 自动化部署

想象一下，每次我要发博文时，都有写好博文，然后编译，再push到github上。
每次都要这样来一次，写几次博客后，就再没兴致了。

使用过持续集成工具吗？脑补一下，你旁边跟着一位仆人，每次你把写好的博文递给他，
“请帮我发布这篇稿子!”，美死了。

## 使用Travis持续集成来自动部署

在github上使用travis来做持续集成，非常便利，只要在你的项目根目录里加上`.travis.yml`，
然后在travis网站打开你相应的库，就算完成了。

### 如何配置travis

在配置travis时，我遇到不少问题，这里先罗列一些我参考过的页面。

[Auto-deploying to My Octopress Blog With Travis-CI]
(http://www.harimenon.com/blog/2013/01/27/auto-deploying-to-my-octopress-blog/)

[travis docs build configuration]
(http://about.travis-ci.org/docs/user/build-configuration/)

[travis-deploy](https://github.com/travis-ci/travis-deploy)

[deploying to heroku from travis-ci](http://www.neilmiddleton.com/deploying-to-heroku-from-travis-ci/)

[introducing continuous deployment to heroku](http://about.travis-ci.org/blog/2013-07-09-introducing-continuous-deployment-to-heroku/)

[生成与在travis中使用密钥文件的方法](https://gist.github.com/floydpink/4631240)

您从这些页面中肯定能找到正确的配置方法。

### 可能遇到的问题

1. Mac 环境下，需要使用GNU版的coreutils工具包

  在生成加密的travis环境变量时，会需要base64
  split等命令，gnu版工具跟mac自带的工具，在参数上有差异，
  如果您的电脑上跑不通，请尝试gnu版coreutils。

2. travis encrypt

  生成加密环境变量时，需要处在您的项目文件夹下，
  跟您的`.travis.yml`处在同一目录，或者使用`-r`指定github库地址。

3. bundle install

  最近travis有更新，安装ruby相关gem时，会使用`bundle install --deployment`，
  这样会把依赖包安装到项目文件夹下`./vendor/bundle`，这样ruhoh在编译的时候，
  会把这个文件夹下的文件当博文来编译，最终编译失败。
  只需要增加一项配置即可解决，`bundler_args: ''`

### 我的配置文件

这里是我的travis配置文件，
[`.travis.yml`](https://github.com/xudejian/xudejian.ruhoh.com/blob/master/.travis.yml)，
欢迎参考。

### 其他问题

请在评论处留言。
