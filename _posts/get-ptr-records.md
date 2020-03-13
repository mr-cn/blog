---
title: 使用nslookup获得IP的PTR记录
date: 2017-09-30 15:09:44
tags:
---
PTR记录并不常用，所以资料比较少，找到方法后特此记录。

终端输入`nslookup`进入程序，输入`set q=ptr`进入PTR查询模式。

输入IP，回车，可显示查询得到的PTR记录。

```
➜  ~ nslookup
> set q=ptr
> 45.76.1.1
Server:         10.0.0.1
Address:        10.0.0.1#53

Non-authoritative answer:
1.1.76.45.in-addr.arpa      name = my.ptr.record.

Authoritative answers can be found from:
```

可以看到，IP的PTR记录显示在了`name = `的后面。