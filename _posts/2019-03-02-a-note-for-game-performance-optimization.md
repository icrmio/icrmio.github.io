---
layout: post
title: 游戏性能优化备忘
create_date: 2019-03-02
modify_date: 2019-10-01
excerpt: 几个检查要点
--- 
排除GPU问题
只要：
1. 尽量不使用shader，或者只使用简单的shader。
2. 不要画太多内容（三角形所占总像素面积）。

那么一般就不会是GPU导致的性能问题。

减少CPU占用
1. 尽量使用批量绘制。
2. 尽量减少draw call次数。
3. 尽量使用占内存更小的纹理。（RGBA4444+抖动）
4. 不在屏幕内的物体，应设置为不显示。

减少GPU占用
绘制大图的时候，把大图拆分成多个多边形，或者使用polygon sprite meshes，以减少透明区域，可降低GPU占用，但是相应的会提升CPU占用。
