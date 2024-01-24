---
title: 现已支持 Python 评测
date: 2024-01-24T11:08:02+08:00
author:
  name: Alan Liang
  avatar: https://acm.sjtu.edu.cn/OnlineJudge-pic/20211124-010333-292122.png
  description: ACMOJ 开发者、运维
tags:
  - 特性
  - 语言
---

ACMOJ 现已支持提交 Python 代码。使用 Python 提交时，时间和内存限制与 C++ 相同，但 Python 代码经常比 C++ 慢，在部分题目上可能会 TLE，敬请注意。

<!--more-->

我们现在使用的 Python 环境为 CPython 3.10.12[^1]。如果有需要，我们将来可能会更换为 [PyPy][pypy]。需要提交 C++ 头文件的题目不支持提交 Python，在提交页面上，语言选择栏也将不显示 Python。（如果一道题目只能使用一种语言提交，则不会显示语言选择栏。）这个配置是 OJ 由评测数据包自动生成的，无需管理员手动操作。

[pypy]: https://www.pypy.org/
[environment]: https://acm.sjtu.edu.cn/OnlineJudge/about#environment
[^1]: 您可以在 [关于 » 编译器参数][environment] 中查看全部评测环境。

作为 OJ 中使用 Python 的一个参考，[这是一份可以通过 A+B Problem 的代码][code]：

```python
a, b = (int(x) for x in input().strip().split())
print(a + b)
```

[code]: https://acm.sjtu.edu.cn/OnlineJudge/code/380213/

在使用 Python 输入数据的过程中需要注意行末可能存在的空格。使用 `strip()` 方法可以去除行首和行末的空格。

如果您在使用的过程中有任何问题或建议，欢迎联系 OJ 开发组。
