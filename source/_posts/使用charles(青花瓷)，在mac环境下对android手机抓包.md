title: 使用Charles(青花瓷)，在Mac环境下对android手机抓包
date: 2015-11-21 13:50:57
tags: [Mac, 工具]
---

####1.下载工具
安装抓包工具 Charles , 到[官网](http://www.charlesproxy.com/)可下载到最新版本

<!-- more -->

####2.用安装了charles的电脑，代理待抓包anroid手机的网络连接
首先查看Mac的网络IP地址；打开电脑Charles软件后，接着打开手机设置，进入当前wifi连接，进入wifi设置界面，找到`代理`选项，设置`代理`为手动，将服务器填为电脑的IP，端口默认为8888（在charles的proxy setting中可以改这个端口号）。这时Charles弹出确认框，点击Allow按钮即可。然后就可以通过电脑抓包
手机的http请求了。

####3.中文乱码问题解决
在charles的content/info.plist 中 的vmoption 添加-Dfile.encoding=UTF-8
![charles](http://7xnr36.com1.z0.glb.clouddn.com/charles.png)



####HTTPS抓包:

如果不进行下面的设置，https的reqeust和response都是乱码，设置完之后https就可以抓包了。

#####手机端设置
下载Charles证书[http://www.charlesproxy.com/ssl.zip](http://www.charlesproxy.com/ssl.zip)，解压后导入到手机中，然后设置->安全->凭据存储->从存储设备安装，选中证书。

v3.10之前的版本使用一个SSL根证书。仍然可以[下载证书](http://www.charlesproxy.com/assets/legacy-ssl/ssl.zip)(安装在移动设备上)。注意,这些证书将不会在v3.10以及之后的版本有效。


如果你运行的是v3.10或之后,请到帮助菜单,根据子菜单SSL Proxying提示操作。


#####电脑端设置
1、在Charles的工具栏上点击设置按钮，选择Proxy Settings…

切换到SSL选项卡，选中Enable SSL Proxying。（别急，选完先别关掉）

2、SSL选项卡的Locations里填写要抓包的域名和端口，点击Add按钮，在弹出的表单中Host填写域名。比如填api.instagram.com，Port填443