---
title: 微信小程序自定义底部tab组件 #文章标题
date: 2019-1-12 12:47:44 #文章生成时间
categories:  #文章分类目录 可以省略
tags: [小程序,tab,自定义组件] #文章标签 可以省略
description:   #你对本页的描述 可以省略
---

底部导航栏这个功能是非常常见的一个功能，基本上一个完成的app，都会存在一个导航栏，那么微信小程序的导航栏该怎么实现呢 <!--more-->
<pre><code>"tabBar": {  
16    "color": "#a9b7b7",  
17    "selectedColor": "#11cd6e",  
18    "borderStyle": "black" ,
19    "list": [{  
20      "selectedIconPath": "images/yi1.png",  
21      "iconPath": "images/yi.png",  
22      "pagePath": "pages/index/index",  
23      "text": "第一页"  
24    }, {  
25      "selectedIconPath": "images/er1.png",  
26      "iconPath": "images/er.png",  
27      "pagePath": "pages/logs/logs",  
28      "text": "第二页"  
29    },{ 
30      "selectedIconPath": "images/san1.png",  
31      "iconPath": "images/san.png",  
32      "pagePath": "pages/mine/mine",  
33      "text": "第三页"  
34    }
</code></pre>
不论B端还是C端，绑定同一个首页，在这个页面当中加载tabbar，原理是将需要绑定tabbar的页面类似选项卡的形式来模拟tabbar。

![](https://pic4.zhimg.com/80/v2-9d072c4bd892ea67935962d01084512f_hd.jpg)

1 tabbar组成
2 tabBar  指底部的 导航配置属性
3 color  未选择时 底部导航文字的颜色
3 selectedColor  选择时 底部导航文字的颜色
5 borderStyle  底部导航边框的样色
6 list   导航配置数组
7 selectedIconPath 选中时 图标路径
8 iconPath 未选择时 图标路径
9 pagePath 页面访问地址
10 text  导航图标下方文字

同时要注意，每个页面的json文件都不能去掉navigationBarTitleText这个属性。否则会报错

前方干货：
tabbar在app.json中配置，但是此文件不可动态修改，采用方法是将tabbar作为一个插件来使用，通过微信号获取信息，使用手机号登录，后台返回当前访客是属于用户还是商户，然后在页面中动态加载tabbar插件。

如果是在每个页面中都引用tabbar，在页面切换的时候会有严重的频闪，并且tabbar会不停隐藏显示，这样子的用户体验感觉是十分不友好的。所以采用单页面形式。

但是为了避免因为数据量大而导致的阻塞。所以在每个tabbar的content页面中采用条件判断来走按需加载，这样能够避免在列表页面不会有停顿空白。
这个页面组件中，我做了根据登录的用户角色信息不同而展示不同的tabbar，从而引导向不同的页面。
tabbar.js:
<pre><code>data: {
    business: [
      { //B端tab数据
        title: '首页',
        icon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/home_tabbar.png",
        selectedIcon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/home_tabbar_checked.png"
      },
      {
        title: '库存管理',
        icon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/buy_tabbar.png",
        selectedIcon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/buy_tabbar_checked.png"
      },
      {
        title: '发布',
        selectedIcon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/publish_normo.png",
        icon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/publish_normo.png",
      },
      {
        title: '客户管理',
        icon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/customer_normol.png",
        selectedIcon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/customer_focus.png"
      },
      {
        title: '我的',
        icon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/me_tabbar.png",
        selectedIcon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/me_tabbar_checked.png"
      }
    ],
    consumer: [{ //C端tab数据
      title: '首页',
      icon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/home_tabbar.png",
      selectedIcon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/home_tabbar_checked.png"
    },
    {
      title: '买车',
      icon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/buy_tabbar.png",
      selectedIcon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/buy_tabbar_checked.png"
    },
    {
      title: '我的',
      icon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/me_tabbar.png",
      selectedIcon: "https://img.58cdn.com.cn/images/car/activitypic/weichebao/me_tabbar_checked.png"
    }
    ],
  },</code></pre>
tabbar.wxml
<pre><code><wxs src="./tabbar.wxs" module="tabbarTool" />
<view class="ha-tab-bar  {{telModelData?'ha-tab-iphoneX-bar':'ha-tab-default-bar'}}">
  <view wx:for="{{terminalType === 'C' ? consumer :  business}}" wx:key="{{index}}" class="ha-tab-item" style="width:33.33%;" >
    <view class="ha-tab-content {{checkIdx == index ? 'checked' : ''}} " data-index="{{index}}" data-text="{{item.title}}" bindtap="onTabbarItemTap">
      <view class="ha-tab-icon">
        <image class='tab-icon' src="{{item.icon}}"></image>
        <image class='checked' src="{{item.selectedIcon}}"></image>
      </view>
      <view class='ha-tab-title'>
        <text>{{item.title}}</text>
      </view>
    </view>
  </view>
</view></code></pre>
tabbar.wxss:
<pre><code>/**
  * 带上 ha-前缀 是为了避免冲突
*/

.ha-tab-bar {
  flex: 1;
  justify-content: space-around;
  width: 100%;
  display: flex;
  flex-direction: row;
  background-color: #fff;
  border-top: 1px solid #d2d2d2;
  /* 定义在底部 */
  position: fixed;
 
  left: 0;
  height: 100rpx;
  z-index: 1000;
}
.ha-tab-default-bar{
  bottom: 0;
}
.ha-tab-iphoneX-bar{
  padding-bottom: 40rpx;
  bottom: 0;
}

.ha-tab-item {
  display: inline-block;
  flex-direction: column;
  align-content: center;
  justify-content: center;
  position: relative;
  padding-top: 16rpx;
  padding-bottom: 2rpx;
  vertical-align: center;
}

.ha-tab-content {
  display: inline-block;
  flex-direction: column;
  align-content: center;
  justify-content: center;
  position: relative;
  width: 100%;
  height: 100%;
}

.ha-tab-icon {
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 1;
  /* width: 60rpx; *//* height: 60rpx; */
}

.ha-tab-icon .checked {
  display: none;
}

.ha-tab-icon.checked .checked {
  display: block;
}

.ha-tab-icon.checked .tab-icon {
  display: none;
}

.ha-tab-content.checked .checked {
  display: block;
}

.ha-tab-content.checked .tab-icon {
  display: none;
}

.ha-tab-content.checked .ha-tab-title {
  color: #0083d2;
}

image {
  width: 48rpx;
  height: 40rpx;
  /* background-color: lightcyan; */
}

.ha-tab-title {
  display: flex;
  line-height: 40rpx;
  text-align: center;
  justify-content: center;
  font-family: PingFangSC-Regular;
  font-size: 22rpx;
  color: #666;
}

.ha-badge-container {
  width: 80rpx;
  height: 50rpx;
  position: absolute;
  /* background-color: lightblue; */
}

.ha-tab-bubble {
  background-color: #e64340;
  text-align: center;
  justify-content: flex-start;
  align-content: center;
  position: absolute;
  top: -14rpx;
  right: -6rpx;
  width: 32rpx;
  height: 32rpx;
  line-height: 26rpx;
  border-radius: 16rpx;
}

.ha-tab-count {
  color: white;
  font-size: 20rpx;
}

.ha-tab-title-selected {
  color: #3782f1;
}
</code></pre>
想要哪个页面来引用，使用下面组件引用结构：
<pre><code>
<tabbar bindtap='taphome' terminalType="{{customertype}}" checkIdx="0" telModel-data="{{telModel}}"></tabbar>
</code></pre>
实现成果如下图

![](https://pic4.zhimg.com/80/v2-e68414b26d334aecb5409ed6d72633df_hd.jpg)

C端用户

![](https://pic3.zhimg.com/80/v2-733649afdd51536429fa37b97249ca1a_hd.jpg)

B端商户

并且在当前页面的js中设置customertype的值，从而加载当前的用户角色应
该对应的页面。注意：telModel-data这个属性是为了兼容iphoneX，当手机上是长款屏幕的话就会有一个底部40rpx的空白高度，如下图：

![](https://pic2.zhimg.com/80/v2-fe04b9509ec089df433da3f217decf89_hd.jpg)

iphoneX
