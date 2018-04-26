## Android 7.1.2 - vivo X9 debug 调试安装APK错误

时间： 2018-4-26

### 1：问题描述

#### 环境配置

* android版本 7.1.2 
* 手机型号：vivo X9

#### 现象描述

在Android7.0上通过Debug调试App的时候，安装Debug App失败，手机提示解析软件包错误，AndroidStudio 提示 Installation failed with message Failed to finalize session。

![](https://i.imgur.com/OiOG6rP.jpg)

当把AndroidStudio setting 里面的Instant run关掉之后，vivo X9出现如下信息。

![](https://i.imgur.com/lFGsTeu.jpg)

#### 问题分析

Android7.0 需要关闭 Instant run功能。而 vivo X9对某些不信任来源的APP安装限制了必须登录vivo账号。

#### 解决办法

* Android7.0 在Android Studio -> Settings -> Instant run 关闭此功能。

* vivo x9 还需要登录vivo账号
