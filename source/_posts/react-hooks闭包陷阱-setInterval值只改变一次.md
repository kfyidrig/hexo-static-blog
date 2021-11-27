---
title: react hooks闭包陷阱 setInterval值只改变一次
date: 2021-11-09 20:10:27
tags: ['前端知识']
---

下面的Closure组件是一个典型的闭包陷阱的例子，我们可能会理所当然的认为counter会因为setInterval每1000毫秒自加1，但是实际上counter会停止在2不再增加。[查看DEMO](https://codesandbox.io/s/react-closure-trap-7tss7?file=/src/App.js)

<!--more -->

```jsx
//例子一
import {useState,useEffect} from 'react'

export default function Closure() {

  const [counter, setCounter] = useState(1);

  function handleAdd() {
    setCounter(counter+1);
  }

  useEffect(function () {
    const taskId = setInterval(
        handleAdd,
        1000
    );
    return function () {
      clearInterval(taskId);
    };
    // 注意此处应填写依赖[handleAdd,counter]
    // 但是为了展示闭包陷阱我们不得不这么做，勿模仿
    // eslint-disable-next-line
  }, [])
  return <>The timer is {counter}.</>
}
```

先不要着急去找出为什么会这样，我们先从更简单的例子看起。

### 一个更典型的例子

初学let、var和const时，我们应该就见过以下例子，var和setTimeout的组合。

```js
//例子二
for(var i=0;i<5;i++){
	setTimeout(()=>{
		console.log(i)
	},0);
}
```

这里的结果是，控制台连续输出了5个5，而不是0到4，要理解这个问题，我们可以从下面这个例子入手。

```js
//例子三
for(let i=0;i<5;i++){
	setTimeout(()=>{
		console.log(i)
	},0);
}
```

这里的结果是，控制台连续输出0到4，这正是我们上一个例子（例子二）所想要的，而我们仅仅把var换成了let，为什么会这样呢？我们不妨对这两个例子（例子二和例子三）进行断点调试。我们先对例子三进行调试，运行至setTimeout时作用域链为`全局 -> for循环块级 -> null` ，**值得注意的是，for循环的每次循环都会生成新的块级作用域**，这里有5个块级作用域（因为循环了5次），每一次循环的i分别位于五个作用块级域，值为i=1至i=4。

![例子二调试](例子三调试.png)

再来看看例子二，作用域链也为`全局 -> for循环块级（内容为空，调试时未显示） -> null`，**这里的for循环也会产生5个块级作用域**，只是作用域内容为空，因为i位于顶级作用域，每一次的循环将会修改顶级作用域里i的值（在setTimeout执行前就已经被修改了）。

![例子二调试](例子二调试.png)

注意，我们并没有在意setTimeout和匿名函数的作用域，例子二和三的作用域示意图如下。首先是例子三（let）的作用域示意图。**注意，for循环的每次循环都会生成新的块级作用域。
![let块级作用域](let块级作用域.png)**

接下来是例子二的作用域示意图，注意，var会提升变量作用域至函数顶部。每一次对i进行加运算都是作用于同一个i。
![var作用域提升](var作用域提升.png)

### 再回来看看hooks闭包陷阱

简单来说，整个setInterval过程如下（可以先看看下方的示意图）：

1. Closure函数执行，组件开始挂载，执行函数Closure，形成作用域A（作用域B包含于作用域A）
2. 组件挂在完成一秒钟后，handleAdd函数（位于作用域A形成的作用域链）读取作用域链上方的counter，读取到counter为1，调用setCounter更新组件
3. Closure函数再次执行，创建了新的作用域C，新作用域C的counter=2，作用域A因为被setInterval内部匿名函数引用，不会被V8回收
4. 一秒钟后，handleAdd函数（位于作用域A形成的作用域链）读取作用域链上方的counter，读取到counter为1，调用setCounter尝试更新组件，但是此时组件更新被Rect阻止，因为新状态counter为2，目前状态counter已经为2
5. 一秒钟后，handleAdd函数（位于作用域A形成的作用域链）读取作用域链上方的counter，counter仍然是1……
6. 一秒钟后，handleAdd函数（位于作用域A形成的作用域链）读取作用域链上方的counter，counter仍然是1……

![closure函数作用域示意图](closure函数作用域示意图.png)

### 简单说下解决方法

第一个方案是在useEffet内部增加一个变量cnt，每次对cnt++再用cnt的值去更新组件的counter，但是这样做会导致第一次组件挂载时的作用域链一直被setInterval内部匿名函数引用，内存泄漏。

第二个方案是使用setTimeout在每次组件counter变化后再去更新组件，但是这样做时间误差可能较大。代码如下。

```jsx
import {useState,useEffect} from 'react'

export default function Closure() {

  const [counter, setCounter] = useState(1);

  useEffect(function () {
    setTimeout(()=>{
      setCounter(counter+1);
    },1000);
  }, [counter])
  return <>The timer is {counter}.</>
}
```

第三种就是记录起始时间，结合第二种方法，只是把counter的值设置为现在时间减去开始时间的差除以1000ms。

```jsx
import {useState,useEffect,useRef} from 'react'

export default function Closure() {
  const timeRef=useRef({last: Date.now()});

  const [counter, setCounter] = useState(1);

  useEffect(function () {
    setTimeout(()=>{
      const now=Date.now(),
          div=now-timeRef.current.last;
      timeRef.current.last=now;
      setCounter(Math.floor(div/1000 + counter));
    },1000);

  }, [counter])
  return <>The timer is {counter}.</>
}
```

