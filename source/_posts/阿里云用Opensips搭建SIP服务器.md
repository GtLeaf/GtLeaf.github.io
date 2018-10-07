---
title: 阿里云用Opensips搭建SIP服务器
date: 2018-09-26 10:57:19
tags:
- opensips
---
#环境：
ubuntu 16
opensips 2.3.2


# 调试
我们需要用抓包来获取请求信息
选用ngrep来抓包——[ngrep语法](http://man.linuxde.net/ngrep)

`apt-get install ngrep`

开始监听5060端口：

`ngrep –p –q –W byline port 5060`

如果opensips使用前台调试，这时候只有一个窗口，无法再开启ngrep进行抓包了
所以我们可以通过screen解决问题，安装screen:

`apt-get install screen`

开启一个新的对话框：

`screen -R sip`

可以在sip窗口进行各种操作，通过Ctrl+a然后按d挂起,返回原窗口
通过下面的命令切回sip窗口：

`screen -r sip`

如果在sip窗口意外关闭会话程序，用上面的命令无法切换到sip窗口，使用以下命令：

`screen -d mc`
`screen -r mc`

在sip窗口输入`exit`，即可关闭sip窗口。
现在我们就可以即用ngrep抓包，又能在前台运行opensips了。

# 安装 rtpproxy
博客地址：https://www.aliyun.com/jiaocheng/434021.html
```
git clone -b master https://github.com/sippy/rtpproxy.git
git -C rtpproxy submodule update --init --recursive
cd rtpproxy
./configure
make
```
安装配置rtpproxy 
$ sudo apt-get install rtpproxy 
$ sudo vi /etc/default/rtpproxy 
修改为如下内容


# Defaults for rtpproxy 
# The control socket. 
#CONTROL_SOCK="unix:/var/run/rtpproxy/rtpproxy.sock" 
# To listen on an UDP socket, uncomment this line: 
CONTROL_SOCK=udp:127.0.0.1:22222 
# Additional options that are passed to the daemon. 
EXTRA_OPTS="" 
LISTEN_ADDR=xxx.xxx.xxx.xxx #你自己的IP地址 
EXTRA_OPTS="-l ${LISTEN_ADDR}" 
启动rtpproxy 
$ sudo killall rtpproxy 
$ sudo /etc/init.d/rtpproxy start 
如果rtpproxy启动失败,请检查/etc/init.d/rtpproxy脚本DAEMON路径是否正确,默认DAEMON=/usr/sbin/$NAME,可能要改为DAEMON=/usr/bin/$NAME
````

# 错误
## 1.403 Preload Route denied
解决：在opensips.cfg中加入alise=公网ip

## 2.处于同一局域网，拨打电话能接通，但没有声音。

## 3.一方挂断电话，另一方需等待36秒才挂断。
解决：在opensips.cfg中，ip地址处加上 "as 公网ip"
格式：listen=udp:私网ip:端口号 as 公网ip:端口号
例如：listen=udp:172.17.223.155:5061 as 101.201.236.255:5061