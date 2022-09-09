---
title: 二次和三次bezier贝塞尔样条插值平滑曲线（严格数学推导）
date: 2022-09-04 11:55:11
tags: [前端知识]
---
前言。前几天几天终于鼓起勇气尝试用**高等数学**的方法求解大二时遇到的曲线平滑问题了，在上班闲暇之余找到了比较完美的解决方法，同时完成了对该方法的数学证明。特别声明该方法提出者为Rob Spencer，原文[关于样条插值](http://scaledinnovation.com/analytics/splines/aboutSplines.html)，我只是学习以及对其进行了简单的数学论证。我相信原作者在提出这个方法时也是进行过论证的，只是文章里没有提及罢了。本文同步发布在[掘金](https://juejin.cn/post/7139087706197327902)。

## 问题描述
有一组离散的点[ [x0, y0], [x1, y1], [x2, y2], ... , [xn, yn] ]，**有什么简单的方式使点与点之间平滑连接**，且在[xi, yi]处也是平滑的（一阶导数存在）。如下图，直线直接连接在[xi, yi]处不平滑。

[代码片段](https://code.juejin.cn/pen/7139084811930435618)

想要实现平滑连接就是要选择合适的插值方法对两个离散点之间进行插值，上面的demo中就是线性插值。但是线性插值在[xi, yi]处是不平滑的，所以最终的曲线也是不平滑的。计算机领域插值用的最多的是贝塞尔插值，前端canvas和svg也支持贝塞尔插值，所以问题可以转换为对n个离散点进行贝塞尔插值，也就是找到若干个合适的控制点。
<!--more-->
## 确定控制点

这里介绍一下Rob Spencer的方法。整体思路：x1与x2之间使用二次贝塞尔插值；x5与x6之间使用贝塞尔插值；x2与x3之间使用三次贝塞尔插值；x3与x4之间使用三次贝塞尔插值，想知道为什么这样做看完后看数学论证就懂了。下面先从计算x2与x3之间的控制点说起。

### 确定x2与x3之间的贝塞尔曲线
x2与x3之间使用三次贝塞尔插值，意味着需要四个控制点。因为需要贝塞尔曲线过x2和x3这两个端点，又因为三次贝塞尔曲线（参数方程如下图）在t=0和t=1时所对应的点刚好是控制点p0和p3，故我们还需要确定两个控制点。欠缺贝塞尔曲线知识的务必先看[Bézier curve](https://en.wikipedia.org/wiki/B%C3%A9zier_curve)。
![三次贝塞尔曲线参数方程](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/79b2a236faac46f097fb2154de21d8cf~tplv-k3u1fbpfcp-watermark.image?)

过x2做平行于x1和x3的直线AB。

![QQ截图20220904000317.jpg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/023aea2ebb714f9da98981b58afb04c2~tplv-k3u1fbpfcp-watermark.image?)
则控制点z0与其他点有如下关系：
![QQ截图20220904001346.jpg](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bbffe651d0c740698f73ba302d426306~tplv-k3u1fbpfcp-watermark.image?)
控制点z1与其他点有如下关系：
![QQ截图20220904001408.jpg](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ff7a62afa024e328914b048a0bad07c~tplv-k3u1fbpfcp-watermark.image?)
其中z1和z2位于直线AB上。a为0到1的可自定义的参数，后续可以更改参数调整平滑效果。这样就能在x2处算出两个控制点，使用同样的方法，在x3处也能算出两个控制点，同理x4和x5也各能算出两个控制点。这样就能使用控制点x2、z1、z2和x3确定x2与x3之间的三次贝塞尔曲线了。同理x3与x4之间、x4与x5之间的三次贝塞尔曲线也可以被确定。
[代码片段](https://code.juejin.cn/pen/7139170177962213387)

### 确定x1与x2之间的贝塞尔曲线
通过上面的计算，我们已经得到了控制点z1，所以x1、z1和x2这三个控制点确定的就是x1与x2之间的二次贝塞尔曲线。

## 平滑效果
### 绘制三次贝塞尔区间
[代码片段](https://code.juejin.cn/pen/7139228803293216782)

### 绘制二次贝塞尔区间
[代码片段](https://code.juejin.cn/pen/7139349456771776543)

## 数学论证
离散点x0、x1、……、x6以及控制点z1、……、z8如下图所示。
![QQ截图20220904104654.jpg](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a93a0d612d054ad585d6ead0be45d45c~tplv-k3u1fbpfcp-watermark.image?)
### 证明新曲线在x3、x4处平滑
要证明曲线在某点处平滑实际上就是证明该点所在的两条曲线的一阶导数存在且相等，举个例子，例如x3处，x3同时在曲线x2 x3上和x3 x4上，就需要这两段曲线在x3处的一阶导数存在且相等。对三次贝塞尔曲线求一阶导数有：
![bda9197c2e77c17d90839b951cb0035d79c8d417.svg](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d6d0584f2da74362b9a97bf6a2c2841d~tplv-k3u1fbpfcp-watermark.image?)
对于于曲线x2 x3，当上述一阶导数中的参数t=1时，对应的点就是x3，化简上式子：
![QQ截图20220904110438.jpg](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a539fce690d0414eab0d2756eac0f0df~tplv-k3u1fbpfcp-watermark.image?)
对于于曲线x3 x4，当上述一阶导数中的参数t=0时，对应的点就是x3，化简上式子：
![QQ截图20220904110703.jpg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a9c22202bcbd4abeb1e0ec02181dc862~tplv-k3u1fbpfcp-watermark.image?)
由于直线z4 x3与直线z3 x3位于同一直线，由于斜率相等，显然有：
![QQ截图20220904111030.jpg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/726eee8895744984961581d51a6ee863~tplv-k3u1fbpfcp-watermark.image?)
故在x3处一阶导数存在且相等，同理x4处也存在且相等。
### 证明新曲线在x2、x5处平滑
对曲线x1 x2对应的参数方程求一阶导，导入t=1，与对曲线x2 x3求一阶导带入t=0时相等，故曲线在x2处平滑，对于x5同理。

**全部得证。**

## 后续计划
1. 写个能平滑用户输入点的demo；
2. 实现墨迹笔锋效果；
3. 采用插件模式重新封装墨迹js库。
