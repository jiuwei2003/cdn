---
title: Runtime类
tags:
  - 其它
categories:
  - 其它
comments: true
abbrlink: 37493
date: 2023-03-19 17:32:41
---

## Runtime

<!--more-->

exec方法操作dos命令

1.创建通过Runtime类getRuntime()方法创建Runtime对象

```
Runtime r = Runtime.getRuntime();
```

2.引用Runtime对象的exec方法实现操作dos命令

以下实现关机功能：

```
r.exec("shutdown -s -t,1000");
```

