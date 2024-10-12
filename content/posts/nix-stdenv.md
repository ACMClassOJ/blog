---
title: 使用 Nix 管理评测环境
date: 2024-10-12T11:00:00+08:00
author:
  name: Alan Liang
  avatar: https://acm.sjtu.edu.cn/OnlineJudge-pic/20211124-010333-292122.png
  description: ACMOJ 开发者、前运维
tags:
  - 特性
  - 评测
---

长期以来，评测机使用操作系统（Ubuntu Server）提供的编译器和运行时来评测用户程序。受制于 Ubuntu 较为保守的版本策略，我们无法及时支持最新的编译器版本。同时，评测机环境全部通过 apt 手动安装，并没有一个标准化的评测环境。近期，我们使用 [Nix][nix] 描述了标准化的评测环境，评测时将使用 Nixpkgs 提供的编译器、运行时等，而不再使用 Ubuntu 自带的环境。

[nix]: https://nixos.org/

<!--more-->

## 为什么使用 Nix？

与一般的发行版相比，nix 可以用很少量的代码描述一个完整操作系统的运行环境。我们可以从配置文件生成一个文件树，直接 chroot 进去作为任务的运行环境。

Docker（或其他 OCI 容器技术）也可以实现类似的功能，但是 docker 容器的启动是一个比较复杂的过程，会引入不必要的额外开销。

Nix 利用符号链接实现软件包安装。这样，我们就可以创建多个运行环境，而它们共用的软件包只会占用一份磁盘空间。

[洛谷][luogu-env] 和 [Hydro][hydro-env] 也使用 Nix 管理评测环境。

[luogu-env]: https://github.com/luogu-dev/judge-env
[hydro-env]: https://github.com/hydro-dev/nix-channel/blob/6c2bc29efd08ab5982e3f40a4e294c0cd7971b67/judge.nix

## 有什么变化？

编译和运行的环境不再基于 Ubuntu Server，而是基于 NixOS。正常的题目、提交代码应当不受影响。如果发现评测出现了问题，请联系 OJ 运维组。

我们同时升级了 GCC、Python 和 Icarus Verilog 的版本，这可能对代码产生一定的影响。

## 更多细节

请参见 [OJ 源代码][src]。

[src]: https://github.com/ACMClassOJ/TesutoHime/tree/master/judger2/sandbox/stdenv
