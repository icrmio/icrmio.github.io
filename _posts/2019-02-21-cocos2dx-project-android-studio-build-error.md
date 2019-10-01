---
layout: post
title: Cocos2dx项目Android Studio打包趟坑记录
create_date: 2019-02-21
modify_date: 2019-10-01
excerpt: 下载安装JDK 8并删除JKD 10。
--- 
问题描述：

游戏一直用Windows环境打包Android版本，临时需要用Mac打包，但是报错。

解决方法：

Mac下默认安装的是JDK 10，换成JDK 8即可。（也可能与Android Studio或者Gradle的版本有关系）

下载安装JDK 8并删除JKD 10。

小技巧：

Android Studio的报错信息不一定有用，直接在Android Studio的命令行（Terminal）下输入：./gradlew build 来编译，这样输出的错误信息更有参考价值。
