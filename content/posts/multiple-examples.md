---
title: 现已支持多组测试样例
date: 2024-09-27T15:00:00+08:00
author:
  name: Alan Liang
  avatar: https://acm.sjtu.edu.cn/OnlineJudge-pic/20211124-010333-292122.png
  description: ACMOJ 开发者、前运维
tags:
  - 特性
  - 题目
  - 数据
---

很多题目的题面中会提供不止一组的测试样例，现已支持在题面中添加多组测试样例。

<!--more-->

[自 OJ 上线以来][v0]，题面中一直存在着两个字段：「样例输入」和「样例输出」。如果想要展示多组样例，只能以「输入 1、输入 2、输出 1、输出 2」的逻辑展示，而且也没有一个统一的格式。前些天，OJ 的助教群中再次有助教指出了这一问题。（插播一则广告：如果您是使用 ACMOJ 的老师或助教，欢迎您加入 ACMOJ 助教 QQ 群，群号显示在 OJ 首页。）

[v0]: https://github.com/ACMClassOJ/TesutoHime/commit/a71cd47272afce3eecb49a0d7bb2e7b1af38ce6d#diff-5781b4ca165abc96fa544b38d2f062529cf48a46b303d813430b02bd58ad0bb3R13

我们在题面中加入了一个新的部分，在管理界面中显示为「样例（新版）」；原有样例数据默认折叠，在管理界面中显示为「样例（旧版）」。建议您在新上题时，使用新版样例，后续我们会逐步废弃旧版样例。在展示题面时，会优先展示新版样例；若新版样例不存在，则会展示旧版样例。

对于既有题目，我们使用[模式匹配][pattern]的方法将大部分题目迁移到了新版样例，但由于各个助教上题时使用的 Markdown 格式不同，仍有部分题目没有新版样例数据。我们会逐步给剩余的题目手动添加新版样例信息。如果您发现迁移过程出现了错误（样例丢失、被修改、错位等），请联系 OJ 运维组。

[pattern]: https://github.com/ACMClassOJ/TesutoHime/blob/6750700f1a7b0e7c4e72af21d1d74b82965548c3/scripts/migrations/migrate_examples.py
