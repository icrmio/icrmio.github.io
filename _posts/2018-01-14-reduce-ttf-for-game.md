---
layout: post
title: 为游戏缩减TTF字体文件
create_date: 2018-01-14
modify_date: 2019-10-01
excerpt: 找到几个方案，最终决定用FontPruner来实现。
--- 
找到几个方案，最终决定用FontPruner来实现。其它方案可参考下方的相关资料。

FontPruner的执行需要电脑支持Python和Jave的运行。
# 提前准备
## 1. 文本合并
首先，为了方便FontPruner的执行，我们要把所有的文本信息保存到一个txt文件中。

实现文本合并有很多方法，这里我采用了Notepad++以及它自带的插件Combine来实现。
### 步骤：
* 在Notepad++中打开所有要包含的文件。
* 为了以后使用方便，我们可以把当前打开的文件保存成一个Session（相当于工程文件）。
* 执行Combine插件，并保存为alltext.txt。

## 2. 运行准备
* 在FontPruner的目录下创建input目录，并将alltext.txt拷贝进来。
* 如果需要追加其它字符，可以在input目录下创建另外的txt，并将要追加的字符保存进去。
* 一定要注意txt的编码格式统一，否则转换会失败。
* 将字体文件拷贝到FontPruner目录下。
* 在FontPruner的目录下创建output目录。

# 开始导出
打开命令行执行如下命令：
```
cd C:\Tools\FontPruner-master
python FontPruner.py --inputPath=input --inputFont=SourceHanSansK-Regular.ttf --tempPath=output
```
如果没有报错，那就导出成功了。

但是不知道为什么，FontPruner会过滤掉空格和Tab符号。为了解决这一点需要以下操作：

打开output\intermediate\unChineseOutPut.txt文件，在开头手动加入一个空格和一个Tab符号。

然后执行：
```
java -jar bin\sfnttool.jar -c output\intermediate\ChineseOutPut.txt output\intermediate\unChineseOutPut.txt SourceHanSansK-Regular.ttf output\output\SourceHanSansK-Regular.ttf
```
这次的导出文件中就会包含空格了。

# 相关资料
<a href="https://github.com/GameBuildingBlocks/FontPruner">https://github.com/GameBuildingBlocks/FontPruner</a>

<a href="http://font-spider.org/">http://font-spider.org/</a>

<a href="http://ecomfe.github.io/fontmin/">http://ecomfe.github.io/fontmin/</a>

<a href="http://fontstore.baidu.com/static/editor/index.html">http://fontstore.baidu.com/static/editor/index.html</a>
