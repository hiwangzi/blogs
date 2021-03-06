---
title: 【Redis 笔记】常用命令
date: 2017-11-21T11:20:00+08:00
draft: false
summary: ""
tags: ["Redis"]
---

|编号|命令|描述|
|----|---|----|
|1|DEL key|此命令删除一个指定键(如果存在)。|
|2|DUMP key|此命令返回存储在指定键的值的序列化版本。|
|3|EXISTS key|此命令检查键是否存在。|
|4|EXPIRE key seconds|设置键在指定时间秒数之后到期/过期。|
|5|EXPIREAT key timestamp|设置在指定时间戳之后键到期/过期。这里的时间是Unix时间戳格式。|
|6|PEXPIRE key milliseconds|设置键的到期时间(以毫秒为单位)。|
|7|PEXPIREAT key milliseconds-timestamp|以Unix时间戳形式来设置键的到期时间(以毫秒为单位)。|
|8|KEYS pattern|查找与指定模式匹配的所有键。|
|9|MOVE key db|将键移动到另一个数据库。|
|10|PERSIST key|删除指定键的过期时间，得永生。|
|11|PTTL key|获取键的剩余到期时间(以毫秒为单位)。|
|12|RANDOMKEY|从Redis返回一个随机的键。|
|13|RENAME key newkey|更改键的名称。|
|14|RENAMENX key newkey|如果新键不存在，重命名键。|
|15|TYPE key|返回存储在键中的值的数据类型。|