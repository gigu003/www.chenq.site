---
title: "修改ggplot2图例相关设置"
author: "Qiong Chen"
date: '2020-03-27'
slug: ggplot2-lengend
tags:
- ggplot2
categories:
- R
---

ggplot2中的legend包括四个部分: legend.tittle, legend.text, legend.key, legend.backgroud。针对每一部分有四种处理方式：
element_text()绘制标签和标题，可控制字体的family, face, colour, size, hjust, vjust, angle, lineheight,当改变角度时，序将hjust调整至0或1.
element_rect()绘制主要供背景使用的矩形，你可以控制颜色的填充（fill）和边界的colour, size, linetype
element_blank()表示空主题，即对元素不分配相应的绘图空间。该函数可以山区我们不感兴趣的绘图元素。使用之前的colour=NA，fill=NA,让某些元素不可见，但仍然占绘图空间。
element_get()可得到当前主题的设置。
theme()可在一幅图中对某些元素进行局部性修改，theme_update()可为后面图形的绘制进行全局性的修改。

不加Legend

```
p+theme(legend.position='none');
```
删除legend.tittle
```
p+theme(legend.title=element_blank())
```
图例（legend）的位置
图例（legend）的位置和对齐使用的主题设置legend.position来控制，其值可为right,left,top,bottom,none(不加图例，或是一个表示位置的数值。这个数值型位置由legend.justfication给定的相对边角位置表示（取0和1之间的值），它是一个长度为2的数值型向量：右上角为c(1,1),左下角为c(0,0)
```
p+theme(legend.position=”left”)
 ```

修改legend.tittle内容
```
p+scale_colour_hue("what does it eat?",breaks=c("herbi","carni","omni",NA),labels=c("plants","meat","both","don't know"));
```
修改尺寸大小
```
p+theme(legend.background=element_rect(colour="purple",fill="pink",size=3,linetype="dashed"));
p+theme(legend.key.size=unit(2,'cm'));
p+theme(legend.key.width=unit(5,'cm'));
p+theme(legend.text = element_text(colour = 'red', angle = 45, size = 10, hjust = 3, vjust = 3, face = 'bold'))
```

颜色的修改以及一致性：
```
library(RColorBrewer);
newpalette<-colorRampPalette(brewer.pal(12,"Set3"))(length(unique(eee$name)));
p+scale_fill_manual(values=newpalette);
p+geom_bar(position="stack",aes(order=desc(name)))
```