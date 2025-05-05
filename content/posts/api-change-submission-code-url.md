---
title: API 变更：获取提交记录的代码将需要 Authorization Header
date: 2025-05-05T12:00:02+08:00
weight: 1
author:
  name: Alan Liang
  avatar: https://acm.sjtu.edu.cn/OnlineJudge-pic/20211124-010333-292122.png
  description: ACMOJ 开发者、前运维
tags:
  - 特性
  - 变更
  - API
---

自 2025 年 6 月 1 日起，访问 `/submission/{submission id}` 接口返回的 `code_url` 将需要传入 Authorization header 进行鉴权，与其他 API 接口一致。

<!--more-->

我们在 2023 年 2 月对评测后端进行了升级。旧评测机制下，提交的代码存储在数据库中，而新评测机制下，提交的代码存储在 s3 对象存储中。6 月 1 日前，`code_url` 返回的 URL 是 s3 对象存储中内嵌鉴权机制的 URL，无需使用 access token 鉴权；但这种方法无法访问旧评测机制下提交的代码。

为修复这个问题及保持 API 鉴权机制的一致性，自 6 月 1 日起，`code_url` 将变更，需要通过 Authorization header 传入 access token 进行鉴权，否则无法通过权限检查。如果您的应用中用到了这个 API，您将需要更改您的代码以传入此 header。

**注意**：此前，由于 OJ 后端服务器的错误配置，访问 `code_url` **不能**携带 Authorization header。这是一个 bug，现已修复；即日起，访问此 URL 时可以正常携带 Authorization。
