---
title: 代码与数据
date: 2017-09-03T13:20:00+08:00
draft: false
summary: ""
tags: [""]
---

半年之前，第一次接触到这种将函数作为参数传递的做法，当时实在觉得难以理解。

{{< rawhtml >}}
<blockquote class="twitter-tweet"><p lang="zh" dir="ltr">PHP 的变量真的是啥都能装，不管函数还是类，这个真的是灵活到飘逸🙈。另外，“魔术方法”哈哈哈哈哈哈哈🌞好酷哦... <a href="https://t.co/nxGwbBKAEl">pic.twitter.com/nxGwbBKAEl</a></p>&mdash; Zi WANG (@hiwangzi) <a href="https://twitter.com/hiwangzi/status/846697844496986112?ref_src=twsrc%5Etfw">March 28, 2017</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
{{< /rawhtml >}}

直到最近再次接触到 Python，查询了一些资料，才开始对其有了一些初步的认识。

> 我们暂停一下，看看计算机中比较诡异的地方，也就是代码(code)和数据(data)的统一。这是一个槛，如果不跨过这槛，很多概念就不清楚。我们常常说计算机程序分成 code 和 data 两部分。很多人会理解成，code 是会运行的，是动态的，data 是给 code 使用，是静态的，这是两种完全不同的东西。
>
> 回调函数（callback）是什么？ - 回答作者: 黄兢成 https://zhihu.com/question/19801131/answer/17156023 

最开始我的认知也就如上面这个问题的答主描述的一般，但如果转变一下思维，将 code 与 data 统一视为视为信息，对于 PHP 中可以将函数作为参数传递的做法，也就可以理解了。

> 其实 code 只是对行为的一种描述，比如有个机器人可以开灯，关灯，扫地。如果跟机器人约定好，0 表示开灯，1 表示关灯，2 表示扫地。
> 
> 我发出指令串，0 1 2，就可以控制机器人开灯，关灯，扫地。再约定用二进制表示，两位一个指令，就有一个数字串，000111，这个时候 000111 这串数字就描述了机器人的一系列动作，这个就是从一方面理解是 code，它可以控制机器人的行为。
> 
> 但另一方面，它可以传递，可以记录，可以修改，也就是数据。只要大家都协商好，code 就可以编码成 data, 将 data 解释运行的时候，也变成了 code。

关于 code 与 data，上面的答主如是说：
> * 有些语言不区分，它的 function(表示code)跟 int, double 的地位是一样的。这种语言称函数是第一类值。
> * 有些语言是不能存储函数，不能动态创建函数，不能动态销毁函数。只能存储一个指向函数的指针，这种语言称函数是第二类值。

在 Python 中，将一切都视为对象，传递的即是函数的指针。这种做法即属于上述描述中的第二类。

最后，发自内心的说，这个[回答的内容](https://zhihu.com/question/19801131/answer/17156023 )真得值得一读。

我要走的路还有很长，路漫漫其修远兮……