---
title: Http请求各内容说明
date: 2019-02-18
categories: 
tags: [https,http,请求结构]
description: 
toc: true
---
由于在接口测试中，会用到诸多http的设置和内容，而如何看接口的信息呢？如下以chrome下的接口内容做说明，说明下http接口请求的内容：
<!--more-->
F12打开开发者工具，在网页做一些操作，DevTools里就会有所有的请求出现，包括接口请求和其他各种资源请求譬如图片，css样式，js等；
如下为一个接口请求：
![](https://img-blog.csdn.net/2018101917513247?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RmMDEyOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
如上图所示，左侧为接口栏，右侧为接口内容栏：
右侧主要包括Headers，Preview，Response，Cookies和Timming栏；

## 1、Headers页说明

### (1)、General


General为通用头部，内容包括如下几个部分：

`Request URL`：请求地址，注意里边的?之后的部分是参数，?之前的是地址，在使用HttpClient之类的做接口请求的时候如果输入的url里包含参数了，就不要再单独附加参数进去了。

`Request Method`：请求方法，为http协议规定的那些请求方法，如get，put，post，delete等，这个即为接口测试的方法；

`Status Code`：为返回状态码，此处也是http协议规定的那些返回状态码，可以用于接口自动化测试的验证点；

`Remote Address`：请求的远程地址，这个做接口测试不需要使用；

`Referrer Policy`：referrer策略，目前包含八种，详情参考Referrer Policy；

### (2)、Response Headers

Response Headers为服务器返回给客户端的header，内容大致说明如下：

`Accept-Ranges`：WEB服务器表明自己是否接受获取其某个实体的一部分（比如文件的一部分）的请求，即断点续传功能。bytes：表示接受，none：表示不接受。

`Accept-Charset`：浏览器申明自己接收的字符集。

`Cache-Control`：缓存控制，共分五种，分别为：public(可以用 Cached 内容回应任何用户)、private（只能用缓存内容回应先前请求该内容的那个用户)、no-cache(可以缓存，但是只有在跟WEB服务器验证了其有效后，才能返回给客户端)、max-age(本响应包含的对象的过期时间)、ALL: no-store(不允许缓存)。

`Connection`：处理完这次请求后，是断开连接还是继续保持连接。包括如下三种：close（连接已经关闭）、keepalive（连接保持着，在等待本次连接的后续请求）、***Keep-Alive：***如果浏览器请求保持连接，则该头部表明希望 WEB 服务器保持连接多长时间（秒）。例如：Keep-Alive：300。

`Content-Encoding`：WEB服务器表明自己使用了什么压缩方法（gzip，deflate）压缩响应中的对象。例如：Content-Encoding：gzip。

`Content-Language`：WEB 服务器告诉浏览器自己响应的对象的语言。

`Content-Length`：WEB 服务器告诉浏览器自己响应的对象的长度。例如：Content-Length: 26012。

`Content-Type`：WEB 服务器告诉浏览器自己响应的对象的类型。例如：Content-Type：application/xml。

`Date`：当前的GMT时间。你可以用setDateHeader来设置这个头以避免转换时间格式的麻烦。

`Etag`：就是一个对象（比如URL）的标志值，就一个对象而言，比如一个 html 文件，如果被修改了，其 Etag 也会别修改，所以，ETag 的作用跟 Last-Modified 的作用差不多，主要供 WEB 服务器判断一个对象是否改变了。比如前一次请求某个 html 文件时，获得了其 ETag，当这次又请求这个文件时，浏览器就会把先前获得的 ETag 值发送给 WEB 服务器，然后 WEB 服务器会把这个 ETag 跟该文件的当前 ETag 进行对比，然后就知道这个文件有没有改变了。

`Expires`：指明应该在什么时候认为文档已经过期，从而不再缓存它，对于过期了的对象，只有在跟WEB服务器验证了其有效性后，才能用来响应客户请求。是 HTTP/1.0 的头部。例如：Expires：Sat, 23 May 2009 10:02:12 GMT。

`Last-Modified`：WEB 服务器认为对象的最后修改时间，比如文件的最后修改时间，动态页面的最后产生时间等等。例如：Last-Modified：Tue, 06 May 2008 02:42:43 GMT。

`Server`：一种标明Web服务器软件及其版本号的头标。例如：Server: Apache/2.0.46(Win32)。

`Set-Cookie`：顾名思义，添加cookie。

`Location`：表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302。

`Refresh`：表示浏览器应该在多少时间之后刷新文档，以秒计。

`Content-Disposition`：告诉浏览器以下载方式打开资源，值的例子为：attachment; filename=aaa.zip。
### (3)、Request Headers

此部分为客户端需要提供的给web服务器的header，其字段和含义如下所示：

`Accept`：告诉WEB服务器自己接受什么介质类型，/ 表示任何类型，type/* 表示该类型下的所有子类型，type/sub-type。例如：image/webp,image/apng,image/,/*;q=0.8。

`Accept-Charset`：浏览器申明自己接收的字符集。

`Accept-Encoding`：浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法 （gzip, deflate, br）。

`Accept-Language`：浏览器申明自己接收的语言语言跟字符集的区别。中文是语言，中文有多种字符集，比如big5，gb2312，gbk等等。例如：zh-CN,zh;q=0.9。

`Connection`：处理完这次请求后，是断开连接还是继续保持连接，此为客户端的header，故值包含两种如下：close（告诉WEB服务器或者代理服务器，在完成本次请求的响应后，断开连接，不要等待本次连接的后续请求了）、keepalive（告诉WEB服务器或者代理服务器，在完成本次请求的响应后，保持连接，等待本次连接的后续请求）。

`Cookie`：发送给web服务器的cookie内容。

`Host`：客户端指定自己想访问的WEB服务器的域名/IP 地址和端口号。

`Referer`：告诉服务器该页面从哪个页面链接的

`User-Agent`：浏览器表明自己的身份（是哪种浏览器）

`If-Modified-Since`：如果包含了GET请求，导致该请求条件性地依赖于资源上次修改日期。如果出现了此头标，并且自指定日期以来，此资源已被修改，应该反回一个304响应代码

`Cache-Control`：缓存控制，此处包含四种：no-cache（不要缓存的实体，要求现在从WEB服务器去取）、max-age：（只接受 Age 值小于 max-age 值，并且没有过期的对象）、max-stale：（可以接受过去的对象，但是过期时间必须小于max-stale 值）、min-fresh：（接受其新鲜生命期大于其当前 Age 跟 min-fresh 值之和的缓存对象）。

`Upgrade-insecure-requests`：申明浏览器支持从 http 请求自动升级为 https 请求，并且在以后发送请求的时候都使用 https。

### (3)、Query String Parameters

此部分即为该请求需要传送的参数，里边的内容是接口内部定义的。

## 2、Preview


此部分为预览，即服务器返回的内容的预览，如果是个html字符串，或者是图片之类的就可以在此处看到预览内容，如果是json字符串或者js等此处就为空；

## 3、Response


此部分为服务器返回的请求结果，内容多种多样，如果没返回数据则此页面无结果；

## 4、Cookies


此处是当前浏览器内的cookie的内容；

## 5、Timming
`Queueing` 
请求文件顺序的的排序 

浏览器有线程限制的，发请求也不能所有的请求同时发送，

从添加到待处理队列 

到实际开始处理的时间间隔标示

`Stalled` 
是浏览器得到要发出这个请求的指令到请求可以发出的等待时间，一般是代理协商、以及等待可复用的TCP连接释放的时间，不包括DNS查询、建立TCP连接等时间等

`DNS Lookup `
时间执行DNS查找。每个新域pagerequires DNS查找一个完整的往返。 DNS查询的时间，当本地DNS缓存没有的时候，这个时间可能是有一段长度的，但是比如你一旦在host中设置了DNS，或者第二次访问，由于浏览器的DNS缓存还在，这个时间就为0了。

`Initial connection `
建立TCP连接的时间，就相当于客户端从发请求开始到TCP握手结束这一段，包括DNS查询+Proxy时间+TCP握手时间。

`Request sent `
请求第一个字节发出前到最后一个字节发出后的时间，也就是上传时间

`Waiting(TTFB) `
请求发出后，到收到响应的第一个字节所花费的时间(Time To First Byte),发送请求完毕到接收请求开始的时间;这个时间段就代表服务器处理和返回数据网络延时时间了。服务器优化的目的就是要让这个时间段尽可能短。

`Content Download `
收到响应的第一个字节，到接受完最后一个字节的时间，就是下载时间