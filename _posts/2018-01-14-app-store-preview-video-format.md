---
layout: post
title: 易于提交的App Store Preview视频格式
create_date: 2018-01-14
modify_date: 2019-10-01
excerpt: 反正我是为了提交App Store的Preview视频操碎了心。
--- 
反正我是为了提交App Store的Preview视频操碎了心。

总之是这个链接拯救了我：

<a href="https://stackoverflow.com/questions/25820601/unable-to-load-app-preview-in-itunes-connect">https://stackoverflow.com/questions/25820601/unable-to-load-app-preview-in-itunes-connect</a>

这个方法使用了一个叫做<a href="https://handbrake.fr/">HandBrake</a>的视频格式转换软件。

除了链接中提到的之外，其实还有几点需要注意的。我把所有的都列一下好了：
* Format：MP4 File
* Framerate(FPS): 30, Constant Framerate
* Quality: Constant Quality, RF: 0. （其实RF这里可以，可以调整一下，是导出质量）
* 切换到Picutre页签。
* 注意Storage Size还有Display Size要与Source保持一致。如果不一致，则需要调整Anamorphic和Cropping选项。
* 切换到Audio页签。
* Samplerate设置为44.1。
这样基本上导出来的格式就比较稳了。
