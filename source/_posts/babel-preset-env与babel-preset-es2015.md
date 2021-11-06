---
title: babel-preset-env与babel-preset-es2015
date: 2021-01-27 18:00:24
tags: [前端知识]
---

最近开始入门react了，按照书上命令安装JSX转换工具链`babel-preset-es2015`时npm提示我现在有更新的babel-preset-env可以用咯（Deprecated），看了官方[babel-preset-es2015 -> babel-preset-env](https://babeljs.io/docs/en/env/)的说明，还是有点蒙，所以搜索了下babel-preset-env与babel-preset-es2015的关系。

## 如何使用

1. 卸载原来的`babel-preset-es2015`，安装`babel-preset-env`

```shell
npm uninstall babel-preset-es2015		#如果是局部安装
npm uninstall babel-preset-es2015 -g	#如果是全局安装
npm install babel-preset-env --save-dev
```

<!-- more-->

2. 接下来在 env工具链目录配置 .babelrc 文件，如果没有就创建一个，修改`presets`项为env

```json
{
	...
	"presets": [ "env" ],
	...
}
```

3. 到这里，babel命令已经在项目node_modules/.bin目录下了，env就已经可以用了！更多不妨看看知乎专栏[再见，babel-preset-2015](https://zhuanlan.zhihu.com/p/29506685) 。可以在这里测试一个示例，在项目目录新建一个index.js，内容如下

   ```javascript
   function quicksort(arr){
       if(!arr.length) return [];
       const [pivot,...rest]=arr;
       return [
           ...quicksort(rest.filter(x=>x < pivot)),
           pivot,
           ...quicksort(rest.filter(x=>x >= pivot)),
       ];
   }
   ```

   执行如下命令，便可得到ES5版本的JavaScript代码，便于低版本浏览器运行。

   ```javascript
   "use strict";
   
   function _toConsumableArray(arr) { if (Array.isArray(arr)) { for (var i = 0, arr2 = Array(arr.length); i < arr.length; i++) { arr2[i] = arr[i]; } return arr2; } else { return Array.from(arr); } }
   
   function _toArray(arr) { return Array.isArray(arr) ? arr : Array.from(arr); }
   
   function quicksort(arr) {
       if (!arr.length) return [];
   
       var _arr = _toArray(arr),
           pivot = _arr[0],
           rest = _arr.slice(1);
   
       return [].concat(_toConsumableArray(quicksort(rest.filter(function (x) {
           return x < pivot;
       }))), [pivot], _toConsumableArray(quicksort(rest.filter(function (x) {
           return x >= pivot;
       }))));
   }
   ```

Babel编译器不仅可以编译得到较低的ES代码，还可以解析JSX。

