# Markdown里面使用mermaid画流程图（基础）

之前有介绍如何在Markdown里面使用[flowchart.js](http://flowchart.js.org/)插件支持画[流程图](http://blog.csdn.net/subson/article/details/75126945)。Markdown编辑器Typora同样支持使用[mermaid](https://mermaidjs.github.io/)插件来进行画图。

## Graph

关键字graph表示一个流程图的开始，同时需要指定该图的方向。例如

> graph LR 
>    A –> B

表示如下一个从左到右的图。

![这里写图片描述](http://img.blog.csdn.net/20170921172303371?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3Vic29u/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

流程图的定义仅由graph开始，但是方向的定义不止一种。

1. TB（ top bottom）表示从上到下
2. BT（bottom top）表示从下到上
3. RL（right left）表示从右到左
4. LR（left right）表示从左到右
5. TD与TB一样表示从上到下

## 节点

有以下几种节点和形状：

1. 默认节点  A 
2. 文本节点  B[bname] 
3. 圆角节点  C(cname)
4. 圆形节点  D((dname))
5. 非对称节点  E>ename]
6. 菱形节点  F{fname}

以上大写字母表示节点，name表示它的名字，如下图。默认节点的A同时表示该节点和它的名字，例如上图的A和B。

![这里写图片描述](http://img.blog.csdn.net/20170921172428672?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3Vic29u/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 连线

节点间的连接线有多种形状，而且可以在连接线中加入标签：

1. 箭头连接  A1–>B1
2. 开放连接  A2—B2
3. 标签连接  A3–text—B3 或者 A3—|text|B3
4. 箭头标签连接  A4–text –>B4 或者 A4–>|text|B4
5. 虚线开放连接  A5.-B5 或者 A5-.-B5 或者 A5..-B5
6. 虚线箭头连接  A6.->B6 或者 A6-.->B6
7. 标签虚线连接  A7-.text.-B7
8. 标签虚线箭头连接  A8-.text.->B8
9. 粗线开放连接  A9===B9
10. 粗线箭头连接  A10==>B10
11. 标签粗线开放连接  A11==text===B11
12. 标签粗线箭头连接  A12==text==>B12

![这里写图片描述](http://img.blog.csdn.net/20170921172628344?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3Vic29u/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170921172642935?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3Vic29u/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 例子

[Markdown里面的流程图](http://blog.csdn.net/subson/article/details/75126945)中的使用flow画的第一个流程图，同样可以使用mermaid来画，如下图：

![这里写图片描述](http://img.blog.csdn.net/20170921172720627?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3Vic29u/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)