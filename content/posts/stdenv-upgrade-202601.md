---
title: 评测环境已更新至 GCC 15 / Python 3.14
date: 2026-02-27T23:31:38+08:00
weight: 1
author:
  name: Alan Liang
  avatar: https://acm.sjtu.edu.cn/OnlineJudge-pic/20211124-010333-292122.png
  description: ACMOJ 开发者、前运维
tags:
  - 运维
  - 评测
  - 语言
---

我们近日升级了评测环境：

<!--more-->

- GCC: <del>13.3.0</del> → <ins>15.2.0</ins>
- Python: <del>3.13.0rc3</del> → <ins>3.14.0</ins>
- CMake: <del>3.29.6</del> → <ins>4.1.2</ins>

环境依赖的软件包亦有更新。如果您的题目更新后无法评测，请检查题目 SPJ、标程等代码与新版本评测环境的兼容性。

为了方便各位用户在本地获得与评测环境一致的软件版本，我们将评测环境进行了打包。您可以在 x86-64 架构的 Linux 系统（包括 WSL 2）中[下载打包好的环境](http://acm.sjtu.edu.cn/OnlineJudge/oj-images/acmoj-stdenv-20260119.tar.zst)。此功能还在测试中，我们期待您的意见或建议。您可以发送至助教 QQ 群或 <acmclassoj@googlegroups.com>，或者直接联系运维组成员。

感谢您使用 ACM Class OnlineJudge。
