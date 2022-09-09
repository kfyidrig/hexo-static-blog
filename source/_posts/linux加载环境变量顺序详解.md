---
title: linux加载环境变量顺序详解
date: 2022-05-27 20:28:16
tags: [Linux学习]
---

`linux`设置环境变量的方式挺多的，我了解到的有以下方式（注意环境变量指的是$PATH变量）。

### export命令

类似esm命名导出，可以为**shell**变量或函数设置导出属性。例如`export foo = 1`，通过`export -p`可以看到刚刚声明的`foo = 1`，具体用法见[export 命令](https://wangchujiang.com/linux-command/c/export.html)。

<!-- more -->

```bash
export foo = 1

# 例如为nodejs设置环境变量（假定当前shell位于bin目录）
export PATH = $(pwd):$PATH
```

### 修改文件

#### ~/.bashrc

通过修改用户目录下的`.bashrc`文件进行配置.

```bash
echo "export foo = 2" >> ~/.bashrc

# 查看是否设置成功
export -p
```

#### ~/.profile

和修改~/.bashrc文件类似，也是要在文件最后加上新的路径即可。

```bash
echo "export foo = 3" >> ~/.profile
```

#### /etc/bash.bashrc

修改系统配置，需要对该文件有写入权限。

```bash
echo "export foo = 4" >> /etc/bash.bashrc
```

#### /etc/profile

操作方式同上，也需要对该文件有写入权限。

#### /etc/environment

直接编辑`/etc/environment`,也需要对该文件有写入权限。

```bash
sudo nano /etc/environment

PATH="/usr/local/sbin:加入路径即可"
```

### 方法对比

| 方法                 | 生效时间   | 生效期限    | 生效范围     |
| ------------------ | ------ | ------- | -------- |
| 直接export声明         | 立即生效   | 窗口关闭后无效 | 当前终端有效   |
| 修改~/.bashrc        | 立即生效   | 永久有效    | 仅对当前用户有效 |
| 修改~/.profile       | 立即生效   | 永久有效    | 仅对当前用户有效 |
| 修改/etc/bash.bashrc | 新开终端生效 | 永久有效    | 对所有用户有效  |
| 修改/etc/profile     | 新开终端生效 | 永久有效    | 对所有用户有效  |
| 修改/etc/environment | 新开终端生效 | 永久有效    | 对所有用户有效  |

### 总结

其实所谓的优先级仅仅是先加载的被后面的覆盖了而已，通过在不同文件设置同一个变量和查询资料，可以知道linux加载环境变量的顺序为：

1. /etc/environment

2. /etc/profile

3. /etc/bash.bashrc

4. ~/.profile

5. ~/.bashrc

再回来看看上述的5种方法，不难发现其实本质都是修改PATH值。
