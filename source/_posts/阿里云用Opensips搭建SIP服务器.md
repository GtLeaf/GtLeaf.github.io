---
title: 阿里云用Opensips搭建SIP服务器
date: 2018-09-26 10:57:19
tags:
- opensips
---

# 调试
我们需要用抓包来获取请求信息
选用ngrep来抓包
`apt-get install ngrep`
[ngrep语法](http://man.linuxde.net/ngrep)
`ngrep –p –q –W byline port 5060`
开始监听5060端口
如果opensips使用前台调试，这时候只有一个窗口，无法再开启ngrep进行抓包了
所以我们可以通过screen解决问题
安装screen:
`apt-get install screen`
开启一个新的对话框：
`screen -R sip`
可以在sip窗口进行各种操作，通过Ctrl+a然后按d挂起,返回原窗口
通过下面的命令切回sip窗口
`screen -r sip`
如果在sip窗口意外关闭会话程序，用上面的命令无法切换到sip窗口，使用以下命令
`screen -d mc`
`screen -r mc`
在sip窗口输入`exit`，即可关闭sip窗口。
现在我们就可以即用ngrep抓包，又能在前台运行opensips了。

# 错误
## 1.403 Preload Route denied
解决在opensips.cfg中加入alise=公网ip