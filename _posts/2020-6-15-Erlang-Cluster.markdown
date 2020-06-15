---
layout: post
title: "How build Erlang cluster"
date: 2020-6-15 12:28:39 +0800
categories: Erlang
---

## -name -sname 的区别

Erlang 的 Node 名称 = （name @ host）
-name 可以指定 name 或者 name@host

```
erl -name a
Erlang/OTP 22 [erts-10.7.1] [source] [64-bit] [smp:8:8] [ds:8:8:10] [async-threads:1] [hipe] [dtrace]

Eshell V10.7.1  (abort with ^G)
(a@a01.test.com)1>

erl -name a@jk.com
Erlang/OTP 22 [erts-10.7.1] [source] [64-bit] [smp:8:8] [ds:8:8:10] [async-threads:1] [hipe] [dtrace]

Eshell V10.7.1  (abort with ^G)
(a@jk.com)1>
```

-sname 只能用来指定 name 的值,并且 host 没有 DNS 后缀，所以-sname 一般用在 localhost，或者相同的 DNS domain

```
erl -sname a
Erlang/OTP 22 [erts-10.7.1] [source] [64-bit] [smp:8:8] [ds:8:8:10] [async-threads:1] [hipe] [dtrace]

Eshell V10.7.1  (abort with ^G)
(a@a01)1>

erl -sname a@jk.com
2020-06-15 10:56:43.443709
    args: []
    format: "Can't set short node name!\nPlease check your configuration\n"
    label: {error_logger,info_msg}
```

使用 name 的时候，默认是用 FQDN 作为 host name，如果没有 DNS 的支持，node 的链接或有问题，可以用 IP 来代替。

node 启动的时候不会默认连接，之后发送请求的时候才会连接。
