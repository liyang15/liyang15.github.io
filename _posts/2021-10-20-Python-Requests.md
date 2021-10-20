---
layout: post
title:  "Requests: 让 HTTP 服务人类"
categories: Python
tags: Network Engineer
date: 2021-10-20
---



# Requests: 让 HTTP 服务人类

Requests 唯一的一个**非转基因**的 Python HTTP 库，人类可以安全享用。

**警告**：非专业使用其他 HTTP 库会导致危险的副作用，包括：安全缺陷症、冗余代码症、重新发明轮子症、啃文档症、抑郁、头疼、甚至死亡。

看吧，这就是 Requests 的威力：

```
>>> r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
>>> r.status_code
200
>>> r.headers['content-type']
'application/json; charset=utf8'
>>> r.encoding
'utf-8'
>>> r.text
u'{"type":"User"...'
>>> r.json()
{u'private_gists': 419, u'total_private_repos': 77, ...}
```

参见 [未使用 Requests 的相似代码](https://gist.github.com/973705).

Requests 允许你发送**纯天然，植物饲养**的 HTTP/1.1 请求，无需手工劳动。你不需要手动为 URL 添加查询字串，也不需要对 POST 数据进行表单编码。Keep-alive 和 HTTP 连接池的功能是 100% 自动化的，一切动力都来自于根植在 Requests 内部的 [urllib3](https://github.com/shazow/urllib3)。

## 用户见证

Twitter、Spotify、Microsoft、Amazon、Lyft、BuzzFeed、Reddit、NSA、女王殿下的政府、Amazon、Google、Twilio、Mozilla、Heroku、PayPal、NPR、Obama for America、Transifex、Native Instruments、Washington Post、Twitter、SoundCloud、Kippt、Readability、以及若干不愿公开身份的联邦政府机构都在内部使用。

- **Armin Ronacher**

  Requests 是一个完美的例子，它证明了通过恰到好处的抽象，API 可以写得多么优美。

- **Matt DeBoard**

  我要想个办法，把 @kennethreitz 写的 Python requests 模块做成纹身。一字不漏。

- **Daniel Greenfeld**

  感谢 @kennethreitz 的 Requests 库，刚刚用 10 行代码炸掉了 1200 行意大利面代码。今天真是爽呆了！

- **Kenny Meyers**

  Python HTTP: 疑惑与否，都去用 Requests 吧。简单优美，而且符合 Python 风格。

## 功能特性

Requests 完全满足今日 web 的需求。

- Keep-Alive & 连接池
- 国际化域名和 URL
- 带持久 Cookie 的会话
- 浏览器式的 SSL 认证
- 自动内容解码
- 基本/摘要式的身份认证
- 优雅的 key/value Cookie
- 自动解压
- Unicode 响应体
- HTTP(S) 代理支持
- 文件分块上传
- 流下载
- 连接超时
- 分块请求
- 支持 `.netrc`

Requests 支持 Python 2.6—2.7以及3.3—3.7，而且能在 PyPy 下完美运行。

## 用户指南

这部分文档是以文字为主，从 Requests 的背景讲起，然后对 Requests 的重点功能做了逐一的介绍。

- 简介
  - [开发哲学](https://docs.python-requests.org/zh_CN/latest/user/intro.html#id2)
  - [Apache2 协议](https://docs.python-requests.org/zh_CN/latest/user/intro.html#apache2)
  - [Requests 协议](https://docs.python-requests.org/zh_CN/latest/user/intro.html#requests)
- 安装 Requests
  - [pip install requests](https://docs.python-requests.org/zh_CN/latest/user/install.html#pip-install-requests)
  - [获得源码](https://docs.python-requests.org/zh_CN/latest/user/install.html#id1)
- 快速上手
  - [发送请求](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id2)
  - [传递 URL 参数](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#url)
  - [响应内容](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id3)
  - [二进制响应内容](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id4)
  - [JSON 响应内容](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#json)
  - [原始响应内容](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id5)
  - [定制请求头](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id6)
  - [更加复杂的 POST 请求](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#post)
  - [POST一个多部分编码(Multipart-Encoded)的文件](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#post-multipart-encoded)
  - [响应状态码](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id7)
  - [响应头](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id8)
  - [Cookie](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#cookie)
  - [重定向与请求历史](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id9)
  - [超时](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id10)
  - [错误与异常](https://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id11)
- 高级用法
  - [会话对象](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#session-objects)
  - [请求与响应对象](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#request-and-response-objects)
  - [准备的请求 （Prepared Request）](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#prepared-request)
  - [SSL 证书验证](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#ssl)
  - [客户端证书](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#id4)
  - [CA 证书](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#ca)
  - [响应体内容工作流](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#body-content-workflow)
  - [保持活动状态（持久连接）](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#keep-alive)
  - [流式上传](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#streaming-uploads)
  - [块编码请求](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#chunk-encoding)
  - [POST 多个分块编码的文件](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#post)
  - [事件挂钩](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#event-hooks)
  - [自定义身份验证](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#custom-auth)
  - [流式请求](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#streaming-requests)
  - [代理](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#proxies)
  - [合规性](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#compliance)
  - [HTTP动词](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#http)
  - [定制动词](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#id17)
  - [响应头链接字段](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#link-headers)
  - [传输适配器](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#transport-adapters)
  - [阻塞和非阻塞](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#blocking-or-nonblocking)
  - [Header 排序](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#header)
  - [超时（timeout）](https://docs.python-requests.org/zh_CN/latest/user/advanced.html#timeout)
- 身份认证
  - [基本身份认证](https://docs.python-requests.org/zh_CN/latest/user/authentication.html#id2)
  - [摘要式身份认证](https://docs.python-requests.org/zh_CN/latest/user/authentication.html#id3)
  - [OAuth 1 认证](https://docs.python-requests.org/zh_CN/latest/user/authentication.html#oauth-1)
  - [OAuth 2 与 OpenID 连接认证](https://docs.python-requests.org/zh_CN/latest/user/authentication.html#oauth-2-openid)
  - [其他身份认证形式](https://docs.python-requests.org/zh_CN/latest/user/authentication.html#id4)
  - [新的身份认证形式](https://docs.python-requests.org/zh_CN/latest/user/authentication.html#id5)

## 社区指南

这部分文档也是文字为主，详细介绍了 Requests 的生态和社区。

- [常见问题](https://docs.python-requests.org/zh_CN/latest/community/faq.html)
- [推荐的库和扩展](https://docs.python-requests.org/zh_CN/latest/community/recommended.html)
- [Integrations](https://docs.python-requests.org/zh_CN/latest/community/out-there.html)
- [Articles & Talks](https://docs.python-requests.org/zh_CN/latest/community/out-there.html#articles-talks)
- [支持](https://docs.python-requests.org/zh_CN/latest/community/support.html)
- [Vulnerability Disclosure](https://docs.python-requests.org/zh_CN/latest/community/vulnerabilities.html)
- [更新](https://docs.python-requests.org/zh_CN/latest/community/updates.html)
- [版本历史](https://docs.python-requests.org/zh_CN/latest/community/updates.html#id3)
- [Release Process and Rules](https://docs.python-requests.org/zh_CN/latest/community/release-process.html)

## API 文档/指南

如果你要了解具体的函数、类、方法，这部分文档就是为你准备的。

- 开发接口
  - [主要接口](https://docs.python-requests.org/zh_CN/latest/api.html#id2)
  - [异常](https://docs.python-requests.org/zh_CN/latest/api.html#id3)
  - [请求会话](https://docs.python-requests.org/zh_CN/latest/api.html#id4)
  - [低级类](https://docs.python-requests.org/zh_CN/latest/api.html#id5)
  - [更低级的类](https://docs.python-requests.org/zh_CN/latest/api.html#id6)
  - [身份验证](https://docs.python-requests.org/zh_CN/latest/api.html#id7)
  - [编码](https://docs.python-requests.org/zh_CN/latest/api.html#id8)
  - [Cookie](https://docs.python-requests.org/zh_CN/latest/api.html#cookie)
  - [状态码查询](https://docs.python-requests.org/zh_CN/latest/api.html#id9)
  - [迁移到 1.x](https://docs.python-requests.org/zh_CN/latest/api.html#x)
  - [迁移到 2.x](https://docs.python-requests.org/zh_CN/latest/api.html#id12)

## 贡献指南

如果你要为项目做出贡献，请参考这部分文档。

- Contributor's Guide
  - [Be Cordial](https://docs.python-requests.org/zh_CN/latest/dev/contributing.html#be-cordial)
  - [Get Early Feedback](https://docs.python-requests.org/zh_CN/latest/dev/contributing.html#get-early-feedback)
  - [Contribution Suitability](https://docs.python-requests.org/zh_CN/latest/dev/contributing.html#contribution-suitability)
  - Code Contributions
    - [Steps for Submitting Code](https://docs.python-requests.org/zh_CN/latest/dev/contributing.html#steps-for-submitting-code)
    - [Code Review](https://docs.python-requests.org/zh_CN/latest/dev/contributing.html#code-review)
    - [New Contributors](https://docs.python-requests.org/zh_CN/latest/dev/contributing.html#new-contributors)
    - [Kenneth Reitz's Code Style™](https://docs.python-requests.org/zh_CN/latest/dev/contributing.html#kenneth-reitz-s-code-style)
  - [Documentation Contributions](https://docs.python-requests.org/zh_CN/latest/dev/contributing.html#documentation-contributions)
  - [Bug Reports](https://docs.python-requests.org/zh_CN/latest/dev/contributing.html#bug-reports)
  - [Feature Requests](https://docs.python-requests.org/zh_CN/latest/dev/contributing.html#feature-requests)
- Development Philosophy
  - [Management Style](https://docs.python-requests.org/zh_CN/latest/dev/philosophy.html#management-style)
  - [Values](https://docs.python-requests.org/zh_CN/latest/dev/philosophy.html#values)
  - [Semantic Versioning](https://docs.python-requests.org/zh_CN/latest/dev/philosophy.html#semantic-versioning)
  - [Standard Library?](https://docs.python-requests.org/zh_CN/latest/dev/philosophy.html#standard-library)
  - [Linux Distro Packages](https://docs.python-requests.org/zh_CN/latest/dev/philosophy.html#linux-distro-packages)
- How to Help
  - [Feature Freeze](https://docs.python-requests.org/zh_CN/latest/dev/todo.html#feature-freeze)
  - [Development Dependencies](https://docs.python-requests.org/zh_CN/latest/dev/todo.html#development-dependencies)
  - [Runtime Environments](https://docs.python-requests.org/zh_CN/latest/dev/todo.html#runtime-environments)
  - [Are you crazy?](https://docs.python-requests.org/zh_CN/latest/dev/todo.html#are-you-crazy)
  - [Downstream Repackaging](https://docs.python-requests.org/zh_CN/latest/dev/todo.html#downstream-repackaging)
- 贡献者
  - [Keepers of the Four Crystals](https://docs.python-requests.org/zh_CN/latest/dev/authors.html#keepers-of-the-four-crystals)
  - [Urllib3](https://docs.python-requests.org/zh_CN/latest/dev/authors.html#urllib3)
  - [Patches and Suggestions](https://docs.python-requests.org/zh_CN/latest/dev/authors.html#patches-and-suggestions)

没有别的指南了，你现在要靠自己了。

祝你好运。