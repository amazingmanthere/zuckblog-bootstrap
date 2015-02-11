---
layout: post
---

# Windows Phone平台使用Fiddler抓包

Fiddler是一款非常出色的Web调试工具，能够监视和调试http请求，功能强大。在Windows Phone平台下，我们可以利用Fiddler来监视调试网络数据，提高工作效率。

## 下载安装Fiddler

Fiddler的下载地址在[这里](http://fiddler2.com/get-fiddler)，下载完成后，一步步按照向导安装即可。

## 配置Fiddler  

1. 进入Tools->Fiddler Options->Connection，勾选上Allow remote cmputers to connect，点击确定。 
2. 在Fiddler左下方的命令输入框里，输入`prefs set fiddler.network.proxy.registrationhostname [PCName]`，如：`prefs set fiddler.network.proxy.registrationhostname XiaoMing-PC`（右键计算机，点选属性即可查看电脑名称）。 
3. 重启Fiddler，完成配置。

## 监视Windows Phone虚拟机  

按照上面的步骤配置后，启动Windows Phone即可监视Windows Phone虚拟机。

## 监视Windows Phone真机 

1. 监视Windows Phone真机，要求真机和主机在同一个局域网内，并且要手动配置一下真机的wifi代理。 
2. 首先进入到已连接上的wifi网络的编辑界面。进入设置，点击wifi，选择已连接上的wifi网络就可进入到编辑界面。 在编辑界面下，开启代理，输入主机在局域网中的IP地址，输入端口号，端口号与主机上的Fiddler端口一致，默认为8888，点击确定，完成编辑。
3. 启动Fiddler，Fiddler就能监视真机的网络请求。

## 参考

1. <http://www.spikie.be/blog/post/2013/01/04/Windows-Phone-8-and-Fiddler.aspx>