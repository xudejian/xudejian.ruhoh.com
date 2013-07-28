---
title: 在jenkins上访问github私有库
date: '2013-07-28'
author: Dejian Xu
description: 在jenkins上访问github私有库
categories: 'CI/jenkins'
tags:
  - 持续集成
  - CI
  - jenkins
  - github
---

#### 必装插件

[GitHub Plugin](https://wiki.jenkins-ci.org/display/JENKINS/GitHub+Plugin)

#### 设置 GitHub Web Hook

在jenkins的系统设置里，找到Github Web Hook这一章，推荐你使用自动模式。
填上你的GitHub Credentials信息，username和OAuth token，没有OAuth token，
来这里找[https://github.com/settings/applications](https://github.com/settings/applications)

在需要访问github私有库的地方，都需要将响应的ssh公钥提交给你的库设置，或者depoly
key，或者oauth token，总之要能正常访问。

在jenkins任务中，你可以选上`Build when a change is pushed to GitHub`，
每有代码提交到github时，就会自动触发你的jenkins任务。
为保稳妥，你可以勾上`Poll SCM`，定一个周期任务，定期检查github更新，
以免漏了什么更新。

#### 参考资料

[GitHub Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Github+Plugin)
[Personal API tokens](https://github.com/blog/1509-personal-api-tokens)
