---
title: frida-ios-dump
date: 2019-10-24 13:25:57
tags:
---

转自: https://www.jianshu.com/p/12407c198ff0

frida-ios-dump作者为MonkeyDev作者，该工具基于frida提供的强大功能通过注入js实现内存dump然后通过python自动拷贝到电脑生成ipa文件。

一、环境配置 - iOS环境

1. 安装OpenSSH

打开cydia

搜索安装OpenSSH

2. 安装frida

打开cydia 添加源：http://build.frida.re

打开刚刚添加的源 安装Frida

二、环境配置 - Mac环境

1. 安装HomeBrew

用于安装wget、usbmuxd 等。

/usr/bin/ruby -e"$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

2. 安装Wget

wget是一个从网络上自动下载文件的自由工具。

brew install wget

3. 安装pip

pip 是Python 包管理工具，用于安装Python模块。

wget https://bootstrap.pypa.io/get-pip.pysudo python get-pip.py

4. 安装matplotlib

sudo pip install matplotlib

5. 安装usbmuxd

主要用于在USB协议上实现多路TCP连接,将USB通信抽象为TCP通信。

brew install usbmuxd

三、环境配置 - frida

1. 安装frida

Frida是一款基于python + javascript 的hook与调试框架。

它允许你将 JavaScript 的部分代码或者你自己的库注入到 windows、macos、linux、iOS、Android，以及 QNX 的原生应用中，同时能完全访问内存和功能。

sudo pip install frida --ignore-installed six -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com

如果长时间没反应，建议代理VPN后，再次执行，或多等待下。

2. 安装 frida-dump-iOS

基于frida，核心砸壳工具

sudo mkdir /opt/dump &&cd/opt/dump && sudo gitclonehttps://github.com/AloneMonkey/frida-ios-dump

如果提示目录不存在，可以手动建立。

3. 安装脚本依赖环境

sudo pip install -r /opt/dump/frida-ios-dump/requirements.txt --upgrade

4. 修改dump.py参数

用于自动连接ssh

open /opt/dump/frida-ios-dump/dump.py

找到如下几行(32~35)：

User ='root'Password ='alpine'Host ='localhost'Port = 2222

按需修改 如把Password 改成自己的（一般默认都是alpine）

5. 设置别名

方便直接调用dump.py

在终端输入：open ~/.bash_profile在末尾新增下面一段：aliasdump.py="/opt/dump/frida-ios-dump/dump.py"

使别名生效：source ~/.bash_profile

四、砸壳演示

步骤：

iOS 设备，USB 连接电脑。

打开Mac终端，输入iproxy 2222 22把当前连接设备的22端口(SSH端口)，映射到电脑的2222端口。

新建终端页面，输入ssh -p 2222 root@127.0.0.1 连接iOS设备。

dump.py -l 查看需要砸壳的应用。

dump.py 应用名或bundle id进行砸壳.

目标文件在user下

