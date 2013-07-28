---
title: 透过jenkins部署服务器
date: '2013-07-28'
author: Dejian Xu
description: 透过jenkins部署服务器
categories: 'CI/jenkins'
tags:
  - 持续集成
  - CI
  - jenkins
---
如果你也在使用jenkins，这里介绍两款ssh相关插件，你肯定会喜欢上的。

[SSH Slaves plugin](https://wiki.jenkins-ci.org/display/JENKINS/SSH+Slaves+plugin)

[Publish Over SSH Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Publish+Over+SSH+Plugin)

我们在jenkins上做持续集成任务，不免要访问其他服务器，
每次拿用户名密码肯定不是个好主意，
每次在远端服务器上运行命令，不论是在那台服务器上跑jenkins客户端，
还是自己搞一套ssh远程执行脚本，都不是好主意，麻烦一大堆。

* SSH Slaves plugin

  最近更新后，增加了几个贴心功能，TA可以帮你维护你的服务器列表。
  在你用的时候，只要选择一下即可。

* Publish Over SSH Plugin

  原来我们在远端服务器执行shell时，都要自己来负责命令的状态，
  有了这个插件，你就可以像在jenkins服务器上运行命令一样，
  在任意一台服务器上跑脚本啦。
  TA也有一个列表，不过TA是一个用户服务器列表，在你运行命令时，只需要选择一下，
  就可以登录到指定用户的服务器上运行。

  有个小提示，在脚本任意一行(不是最后一行)，发生错误时，TA默认没有退出，
  而是继续运行。
  需要在系统配置页面，打开TA的高级配置项，然后勾上那个选项才行，
  否则，你需要在你的脚本里自行加上一句`set -xe`。

**参考资料**

[SSH Slaves plugin](https://wiki.jenkins-ci.org/display/JENKINS/SSH+Slaves+plugin)

[Publish Over SSH Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Publish+Over+SSH+Plugin)

iTech's Blog

>[Jenkins入门总结](http://www.cnblogs.com/itech/archive/2011/11/23/2260009.html)

>[Jenkins的Linux的Slave的配置](http://www.cnblogs.com/itech/archive/2011/11/10/2244690.html)

>[Jenkins插件之Publish Over SSH/CIFS/FTP](http://www.cnblogs.com/itech/archive/2011/11/21/2257377.html)

Julian Higman's Blog

>[Using Jenkins to run remote deployment scripts over SSH](http://julianhigman.com/blog/2012/02/25/using-jenkins-to-run-remote-deployment-scripts-over-ssh/)
