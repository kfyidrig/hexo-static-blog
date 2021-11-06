---
title: html表格使用总结
date: 2021-06-29 12:32:44
tags: [前端知识]
---

**HTML**的 **`table`** 元素表示表格数据 ，即通过二维数据表表示的信息。表格元素允许一下顺序的子元素：

- 一个可选的 [`<caption>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/caption) 元素
- 零个或多个的`<colgroup>`元素
- 一个可选的`<thead>`元素
- 下列任意一个:
  - 零个或多个`<tr>`元素 
  - 零个或多个` <tbody>`元素
- 一个可选的`<tfoot>`元素

<!-- more -->

### 表格标题元素

**HTML `<caption>` 元素** (or *HTML 表格标题元素*) 展示一个表格的标题， 它常常作为 [`<tbody>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/table) 的第一个子元素出现，同时显示在表格内容的最前面，同样可以被CSS样式化。例子：

<table>
    <caption>Superheros and sidekicks</caption>
    <tr>
        <td>name</td>
        <td>Batman</td>
        <td>Robin</td>
        <td>The Flash</td>
        <td>Kid Flash</td>
    </tr>
    <tr>
        <td>Skill</td>
        <td>Smarts</td>
        <td>Dex, acrobat</td>
        <td>Super speed</td>
        <td>Super speed</td>
    </tr>
</table>



```html
<table>
    <caption>Superheros and sidekicks</caption>
    <tr>
        <th>name</th>
        <td>Batman</td>
        <td>Robin</td>
        <td>The Flash</td>
        <td>Kid Flash</td>
    </tr>
    <tr>
        <th>Skill</th>
        <td>Smarts</td>
        <td>Dex, acrobat</td>
        <td>Super speed</td>
        <td>Super speed</td>
    </tr>
</table>
```

### 表格列组元素

HTML 中的 表格列组（Column Group） `<colgroup>`标签用来定义表中的一组列表。一下例子中用`span=2`来将Batman和Robin以及The Flash和Kid Flash两列两列的分为一个组。

<table>
    <caption>Superheros and sidekicks</caption>
    <colgroup >
        <col span="2" style="background-color: #d7d9f2;">
        <col span="2" style="background-color: #ffe8d4;">
    </colgroup>
    <tr style="background: none;">
        <td>Batman</td>
        <td>Robin</td>
        <td>The Flash</td>
        <td>Kid Flash</td>
    </tr>
    <tr style="background: none;">
        <td>Smarts</td>
        <td>Dex, acrobat</td>
        <td>Super speed</td>
        <td>Super speed</td>
    </tr>
</table>



对于行来说，每行相同位置的组列样式相同。

```html
<table>
    <caption>Superheros and sidekicks</caption>
    <colgroup>
        <col span="2" style="background-color: #d7d9f2;">
        <col span="2" style="background-color: #ffe8d4;">
    </colgroup>
    <tr>
        <th>Batman</th>
        <th>Robin</th>
        <th>The Flash</th>
        <th>Kid Flash</th>
    </tr>
    <tr>
        <td>Smarts</td>
        <td>Dex, acrobat</td>
        <td>Super speed</td>
        <td>Super speed</td>
    </tr>
</table>
```

更多`colgroup`用法见：[colgroup标签的使用方法及作用](https://liudaima.com/a/253.html)

### 常用CSS属性

#### border-collapse

`border-collapse` [CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) 属性是用来决定表格的边框是分开的还是合并的。在分隔模式下，相邻的单元格都拥有独立的边框。在合并模式下，相邻单元格共享边框。具体用法和例子详见 MDN：[border-collapse](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-collapse) 。

#### table-layout

`table-layout` CSS属性定义了用于布局表格*单元格*，*行*和*列*的高宽算法。默认值为`auto`，表示自动计算单元格宽高，可以通过`table-layout: fixd`来通过css指定第一行内列的宽度，设置后，某一列的宽度仅由该列首行的单元格决定。在当前列中，该单元格所在行之后的行并不会影响整个列宽。具体用法和例子详见 MDN：[table-layout](https://developer.mozilla.org/zh-CN/docs/Web/CSS/table-layout) 。