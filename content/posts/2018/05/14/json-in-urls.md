---
title: "译｜在 URLs 中使用 JSON"
date: 2018-05-14T00:00:00+08:00
draft: false
summary: "除了在 URL 中使用路径作为参数或使用查询参数之外，是否还有更好的选择呢？"
tags: ["Web"]
---

> 原文地址：[JSON in URLs | Dropbox Developer Blog](https://blogs.dropbox.com/developers/2015/03/json-in-urls/)

基于 HTTP 的 API 经常将参数编码为 URL 路径或查询参数。例如，调用丢丢盒（Dropbox）API 搜索文件名时的路径可能如下所示：

```
/1/search/auto/My+Documents?query=draft+2013
```

虽然对于简单的例子而言，使用 URL 编码似乎已经足够完美，但使用 JSON 可能也有一些优点。

## 混乱的 URL 路径

在上面的例子中，因为第一个 `+` 就在 URL 之中，所以其就是字面意义上的加号。而第二个 `+` 表示一个空格，因为它位于 URL 查询部分。这两者很容易混淆，因为他们的编码规则在大多数情况下是相同的，而且就像 `urlencode` 一样，有时一些库提供的函数的名字又非常模棱两可。我们 SDK 的一个早期版本就因此有过一个 bug。

另一个常见的错误就是误认为在路径部分 `/` 同其他普通字符一样。

* `/hello-world` 等价于 `/hello%2Dworld`
* `/hello/world` **不**等价于 `/hello%2Fworld`

符号 `/` 是保留的分界符。将其改为使用 `%` 编码的形式会改变 URL 的含义。大多数 URL 编码库并没有非常清晰地对两者作出区分。虽然大多数情况下这不重要，[但某些情况却非常关键](https://sakurity.com/blog/2015/03/15/authy_bypass.html)。

## URL 查询参数表现力不足

假设 API 参数需要一组值时，该怎么处理呢？一些 API 可能使用逗号或重复的名字来处理。

```
/docs/salary.csv?columns=1,2
/docs/salary.csv?column=1&column=2
```

对于嵌套的字段，一些 API 可能如下处理：

```
/emails?from[name]=Don&from[date]=1998-03-24&to[name]=Norm
```

这些变通都是合理的，但他们仍然只是变通之举。并没有普遍的标准。但 JSON 可以一致、简单的处理嵌套。

## 使用 URL 编码是否很糟糕呢？

不，它只是为不同的情况设计的：人类交互使用。

JSON 将一切字符串用引号包裹。这让许多事情变得更简单以及更健壮，但对于人们来说，读与写可能略显乏味单调。

相较于 JSON，URL 参数可以更快的开始使用。在通常情况下，这非常不错。但缺点就是更容易造成混乱，因此任何处理 URL 参数的代码都会变得很复杂。

这里要做一些合理的权衡。 我不会在我的浏览器地址栏使用 JSON，但对于基于 HTTP 的 API 来说，使用 JSON 来处理可能是一个更好的选择。

## 所以问题在哪儿呢？

当思考到这的时候，我们可以说对比于 URL 编码，使用 JSON 在某些方面更佳。对于结构化的数据，HTTP API 响应已经基本上都在使用 JSON 了。最近一次我处理 URL 编码的响应体是在 2007 年时使用 OAuth 1。目前 OAuth 2 已经使用 JSON 了。

API 请求体分裂成了 JSON 和 URL 编码两种形式。URL 编码的一个好处是[你可以很好的使用 `curl` 命令行工具做示例](https://stripe.com/docs/api/curl#create_charge)。但很多 API，包括丢丢盒新一些的 API，已经开始在请求体中使用 JSON。

所以为什么不在 URL 中同样使用 JSON 呢？就像下面这样：

* URL encoded: `/log?a=b&c=4`
* JSON in URL: `/log?%7B%22a%22:%22b%22,%22c%22:4%7D`

第一点，JSON 形式更长了。这可能会导致你的 URL 超出[长度限制](https://stackoverflow.com/questions/417142/what-is-the-maximum-length-of-a-url-in-different-browsers/417184#417184)。

同时，它看起来很丑，但这可以通过抽象来解决。例如，除非在 packet 层面上出现了错误，否则你也无需去处理原始网络 packet。同样的道理，除非你的 URL 出现错误，否则你无需去接触丑陋版本的 URL。一旦它通过了检测，你在错误消息或日志输出中看到的就只是解码过的字符串了。

创建一个整理抽象层需要花费额外的工作，尤其过去从未使用过类似方式。虽然在 URL 中使用 JSON 前期会有一些烦恼，但它带来的好处值得让人一试。