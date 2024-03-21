---
title: 使用更高效的输出比对工具
date: 2024-03-21T22:00:00+08:00
draft: false
author:
  name: Lau YeeYu
  avatar: https://acm.sjtu.edu.cn/OnlineJudge/oj-images/3d435e1d-274a-491b-9f36-f1433c3ccade
  description: ACMOJ 开发者、运维
tags:
  - 特性
  - checker
---

我们已经将输出比对工具更换为由我们自己编写的比对工具，相较于之前使用的通用比对工具 diff，我们的比对工具更为高效，尤其是在比对的文件很大且完全不同时。本文将会介绍使用自己编写的比对工具的原因，以及我们自己编写的输出比对工具的功能。

<!--more-->

注：比对程序在 TesutoHime 仓库的 [`judger2/checker`](https://github.com/ACMClassOJ/TesutoHime/tree/master/judger2/checker) 目录下。如要编译，可以在这个目录下执行 `scripts/build` 程序，这会在 `judger2/checker` 中生成比对程序 `checker`。

## 问题由来

在一次机考时，我们发现等待评测的任务数非常多，而且这个情况在机考没过多久时就发生了。经调查，造成等待队列过长的原因是比对文件过程。有些提交中，diff 程序运行了 10 秒仍然没有完成比对。

这里稍微介绍一下 OJ 的评测逻辑，我们的 OJ 会将一个评测任务分为三个阶段，分别是编译、执行、检查，每个阶段都会使用评测机。编译和执行的时限由题目数据本身决定，而检查阶段的时限由 OJ 预先设定，目前是 10 秒。对于一般的题目，我们使用 `diff -qZB` 来比较输出。

通常情况下，diff 的效率是很高效的，那么为什么这次会出现性能问题呢？根本原因在于 diff 是一个 patch 生成工具，而不是一个只输出是否一致的工具。因此，在我们的参数下，diff 会先尝试生成 patch，而对于两个很大而且几乎完全不同的文件，生成 patch 是一件非常困难的事情，diff 在生成 patch 上耗费太多时间。我们在自己的电脑上测试发现，对于 18 MiB 左右的两个几乎完全不同的文件，diff 将消耗约两分钟的时间。

你可能会问，那么为什么 diff 不针对 `-q`（q 表示 quiet，即不输出不同内容）的情况进行优化？实际上，diff 的确针对 `-q` 参数进行了优化，但是优化只限于仅有 `-q` 参数的情况，也就是说，我们的 `-qZB` 是不会进行优化的。

既然 diff 的性能会在特定情况下存在问题，因此我们打算自己编写专用比对程序，解决性能问题。程序的原理也非常简单，读取一行，然后比对即可（在特殊情况下可以优化，具体见后文）。

## 比对工具的功能

比对工具使用方法如下：（具体用法请看后面的介绍）

```plain
checker [OPTION]... [--] <FILE1> <FILE2>
```

或者

```plain
checker [OPTION]... - [--] <FILE>
```

选项有：

- `-Z` 或 `-ignore-tailing-space`：忽略行末空格；
- `-B` 或 `-ignore-blank-lines`：忽略空行。

你也可以使用 `-h` 选项查看帮助。

### 发现问题时会立刻退出

由于我们的比对工具不需要显示不同，所以程序会在发现不同后就会立刻退出。相较于 diff 这类需要完整比对所有错误的程序，我们的程序会在文件不同时更早地退出。

### 支持自定义参数

除了目前使用的参数外，我们的比对程序还提供了很多其他功能，这些功能主要是为了后续的 OJ 升级预留的。目前我们使用的参数是 `-ZB`，也就是忽略行末空格及空行。程序支持自由设置忽略行末空格以及忽略空行（如单独 `-Z` 就是忽略行末空格）。

### 针对文件直接比较的优化

在没有设置忽略行末空格及空行参数的时候，比对程序会每次尝试读取 4096 Bytes，然后尝试比对，在有 SIMD[^1] 指令集支持的电脑上，性能会非常好。

[^1]: SIMD 是指 simple instruction multiple data，这类指令会一次操作很多的内存数据。相较于传统的每次操作字长大小的数据，这类指令因为硬件优化以及更少的指令而更为高效。

### 支持使用输入流作为比较来源

此外，我们还支持从标准输入流读取。只要加上 `-` 参数，那么比对工具就会比较输入流和指定文件，当发现有不同时，程序会立刻退出。由于程序支持在发现文件不同时立刻退出，因此利用这个特性，我们可以让评测过程变得更短（即在比对程序发现问题时立刻终止评测），这对于一些需要评测很久的程序来说非常重要，可以节约大量的评测时间。

不过，由于目前 OJ 评测是分阶段的，所以这个功能不是特别容易完成，需要对代码进行重构。目前 OJ 并**没有**这个功能。而且，在发现问题时就终止程序会导致运行时间反映的并不是程序的速度，而是程序没有输出错误的时间，因此就算将这个功能实现，这个功能也还是不能是默认开启，必须让助教自行决定何时开启。