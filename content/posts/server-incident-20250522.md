---
title: 2025-05-22 服务中断
date: 2025-05-23T01:00:00+08:00
draft: false
author:
  name: Alan Liang
  avatar: https://acm.sjtu.edu.cn/OnlineJudge-pic/20211124-010333-292122.png
  description: ACM 班服务器运维
tags:
  - 运维
  - 事故
---

2025 年 5 月 22 日深夜至 23 日凌晨，OJ 评测服务发生中断，提交的代码无法及时评测。受影响的提交为 660005 号（2025-05-22 22:47:27）至 660081 号（2025-05-23 00:48:24）。这部分提交在 2025-05-23 00:51 统一进行了评测。

如果您是受此事件影响的学生，我们向您表示诚挚的歉意。如果您是老师或助教，且您的作业于 22 日晚截止，我们希望您能给受此事件影响的学生一定的宽限期，感谢您的理解。

<!--more-->

## 发生了什么？

[ACM 班服务器即将搬迁](https://acm.sjtu.edu.cn/OnlineJudge/blog/server-relocation-202505/)。新机房的网络环境与原有机房不同，因此，我们需要重新配置服务器的网络环境。为尽量减小迁移期间的下线时间，我提前对服务器内部的网络架构做了一些调整，调整期间意外破坏了 OJ Web 服务器与评测机间的网络连接，且没有及时发现。

目前，OJ 仅有针对 Web 服务的监控报警，没有对评测机的可用性进行监控，导致服务中断 2 小时后手动检查时才发现问题。未来，我们会建立自动化的评测机检测报警机制，以避免此类故障的再次发生。
