---
title: 体验react18新特性
date: 2022-04-10 19:54:09
tags: [前端知识]
---

### 采用增量更新

react17只有在事件处理函数内才会使用增量更新，react18中所有更新操作均为增量更新模式，且引入更新优先级概念，setState(新状态, 优先级)。

<!-- more -->

### Suspense

### startTransition

会先渲染高优先级的更新，再把所有更新再执行一次。可以使用useDeferredValue降低state优先级。

### useTransition

利用双缓冲，在Suspense内，直接切换内容，而无需显示loading。

##基础知识

### 并发更新

有优先级的可中断的更新。

### 双缓冲

一个缓冲用于执行操作，另一个直接用于显示，操作完成二者交换。
