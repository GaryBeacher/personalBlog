---
title:  Charles抓包HTTP/HTTPS
categories:  
tags: [Charles,抓包]
description:  
date:  2018-09-05
toc: true
---
抓包分析数据在移动开发中十分重要，可以帮助我们更快的了解数据构成，提高开发效率。但是在苹果要求上线的App必须使用HTTS之后，HTTPS数据包的抓取分析较为麻烦，在此总结了在mac上使用Charles抓包的详细步骤。
<!--more-->
首先我们下载最先版本的Charles
官网下载：https://www.charlesproxy.com/download/
免费版下载：http://xclient.info/search/s/charles/

## 一、开启网络请求记录，设置系统网络代理
安装Charles之后，我们选择Proxy->Start Recording，开始记录网络请求，然后勾选MacOS Proxy(和其他的代理对象如：Mozilla Firefox Proxy火狐浏览器）,将系统代理设置通过Charles Proxy。

这里写图片描述

此时打开系统偏好设置->网络->高级，我们可以看到本机HTTP和HTTPS请求被代理到127.0.0.1，端口号是8888。至此，我们已经完成了基本的网路请求设置，通过此Mac发起的HTTP请求，我们都可以通过Charles分析。

这里写图片描述

注：在Charles关闭的时候，这里的web代理和安全web代理也会变成无勾选状态。保证无代理时，Mac也能够访问网络。

## 二、iPhone数据包的抓取
###  1.打开Charles的代理功能
为了使用Charles抓取到iPhone设备的数据包，我们首先要打开Charles的代理功能。选择Proxy ->Proxy Setting，设置Port:8888，选择Enable TransParent HTTP Proxying。

这里写图片描述

###  2.获取本机电脑IP
接下来我们要将手机的网络代理IP设置为Charles运行所在的电脑IP，获取本机电脑的IP方法如下：
方法一：Mac电脑上使用Control +空格键，并输入Terminal 可以进入控制台，然后键入 ifconfig en0命令 ，我们查看到当前电脑的IP地址。

这里写图片描述

方法二：通过Charles查看本机的IP地址：打开Charles ->Help->Local IP Address

这里写图片描述

###  3. 设置手机网络代理IP
我们依次打开iphone “设置->无线局域网”，点击当前连接Wifi右侧的详情按钮。这里显示了当前连接Wifi的基本信息，我们需要将这里底部的HTTP代理改为手动，然后填上Charles运行所在电脑的IP和端口号8888。如图：
这里写图片描述

此时，iPhone的网络代理就设置完成了，手机上请求将会被代理到mac上，我们可以很方便的通过Charles查看到手机应用发起的网络请求数据包。

## 三、抓取HTTPS数据包
相对于HTTP类的网络请求，HTTPS请求更加安全，这也使得抓取这类的数据包进行分析要麻烦一些。抓取HTTPS请求数据包进行分析，关键的步骤如下：

###  1.安装Charles根证书
打开charles,依次点击Help -> SSL Proxying -> Install Charles Root Certificate，安装根证书

这里写图片描述

###  2.设置证书信任
在安装证书之后，我们查看钥匙串。选择所有项目，我们会看到一个带有红叉标记不被信任的Charles证书。Charles证书默认是不信任的，需要我们手动设置。右键->显示简介->点击信任，我们如图设置始终信任。

这里写图片描述

###  3.设置 SSL 代理
打开charles应用，选择Proxy->SSL Proxying Settings,我们在这里设置SSL Proxy,点击面板上的add，如下图：

这里写图片描述在这里我们设置主机地址Host是*,使用通配符表示检测所有网络请求。然后设置端口号是443

###  4.iOS设备安装证书
最后我们还需要在iOS设备上安装证书。点击 Charles 的顶部菜单，选择 Help –> SSL Proxying–> Install Charles Root Certificate on a Mobile Device or Remote Browser，然后就可以看到 Charles 显示如下弹窗：

屏幕快照 2017-01-09 下午2.18.11.png

然后我们需要打开safari ,输入网址： http://charlesproxy.com/getssl，
这时候会弹出安装证书的界面，我们点击安装证书，如图：
这里写图片描述
目前为止，我们就完成了Charles抓取HTTPS数据包的所有设置了。查看Charles,我们可以看到数据包的内容了。

###  5.失败请求的处理
iOS10.3之后，在上述设置完成之后，所有的https请求都会失败。提示错误：Failure SSLHandshake: Received fatal alert: unknown_ca 和You may need to configure your browser or application to trust the Charles Root Certificate.
原因：charles的根证书虽然已经在安装列表中,但在iOS 10.3之后,安装新的自定义证书默认是不受信任的。如果要信任已安装的自定义证书,需要手动打开开关以信任证书。
解决：设置->通用->关于本机->证书信任设置-> 找到charles proxy custom root certificate然后信任该证书即可. 模拟器也是这样处理。
