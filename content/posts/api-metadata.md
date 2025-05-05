---
title: API 现已支持获取 OJ 配置及状态
date: 2025-05-05T12:00:00+08:00
author:
  name: Alan Liang
  avatar: https://acm.sjtu.edu.cn/OnlineJudge-pic/20211124-010333-292122.png
  description: ACMOJ 开发者、前运维
tags:
  - 特性
  - API
---

API 新增关于 OJ 配置及状态相关的接口，开发者无需在代码中内嵌此部分数据。

<!--more-->

新增接口一览：

- `/meta/info/judge-status`：评测状态代码（例如 `wrong_answer`）对应的名称、短名称及 Bootstrap 颜色名；
- `/meta/info/language`：编程语言代码（例如 `cpp`）对应的编程语言名称及扩展名；
- `/meta/runner-status`：获取评测机状态；
- `/submission/{submission id}`：
  - `should_auto_reload`：是否应该轮询评测状态（为 true 表示评测尚未结束，应该在一段时间后重新加载评测状态）；
  - `should_show_score`：是否应该显示分数（如评测未完成、中止、分数无效等情况不应显示分数）。

您可以在 [API 详细文档](https://acm.sjtu.edu.cn/OnlineJudge/static/api/index.html) 中查看、体验这些接口。
