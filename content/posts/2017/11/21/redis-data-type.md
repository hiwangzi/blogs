---
title: 【Redis 笔记】数据类型
date: 2017-11-21T10:41:00+08:00
draft: false
summary: ""
tags: ["Redis"]
---

REmote DIctionary Server(Redis)，一个 key-value 存储系统。

## 数据类型

Redis 支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。 

* string
    * string 类型是二进制安全的。意思是 redis 的 string 可以包含任何数据。比如 jpg 图片或者序列化的对象。
    * string 类型是 Redis 最基本的数据类型，一个键最大能存储 512MB。 
    * 相关命令：```set```, ```get```
        ```bash
        127.0.0.1:6379> set str1 hello
        OK
        127.0.0.1:6379> get str1
        "hello"
        ```

* hash
    * hash 是键值对的集合。
    * 每个 hash 可以存储 2^32 - 1 个（超过 40 亿个）键值对。
    * 相关命令：```hmset```, ```hmget```, ```hgetall```
        ```bash
        127.0.0.1:6379> hmset hashmap1 name hiwangzi blog hiwangzi.com
        OK
        127.0.0.1:6379> hmget hashmap1 name
        1) "hiwangzi"
        127.0.0.1:6379> hgetall hashmap1
        1) "name"
        2) "hiwangzi"
        3) "blog"
        4) "hiwangzi.com"
        ```

* list
    * list 是字符串列表，按**插入顺序排序**。
    * 列表的最大长度为 2^32 - 1 个元素。
    * 相关命令：```lpush```, ```lrange```
        ```bash
        127.0.0.1:6379> lpush list1 a b
        (integer) 2
        127.0.0.1:6379> lrange list1 0 100
        1) "b"
        2) "a"
        127.0.0.1:6379> lpush list1 c
        (integer) 3
        127.0.0.1:6379> lrange list1 0 2
        1) "c"
        2) "b"
        3) "a"
        ```
* set
    * set 是字符串的**无序**集合。
    * 添加，删除和验证成员是否存在的时间O(1)复杂性。
    * set 的最大成员数量为 2^32 - 1 个元素。
    * 相关命令：```sadd```, ```smembers```
        ```bash
        127.0.0.1:6379> sadd set1 a b
        (integer) 2
        127.0.0.1:6379> smembers set1
        1) "b"
        2) "a"
        127.0.0.1:6379> sadd set1 c
        (integer) 1
        127.0.0.1:6379> smembers set1
        1) "b"
        2) "c"
        3) "a"
        127.0.0.1:6379> sadd set1 a
        (integer) 0
        127.0.0.1:6379> smembers set1
        1) "b"
        2) "c"
        3) "a"

        # a 被添加了两次，但集合有唯一属性，所以只会存储一个。
        ```

* zset
    * zset = sorted set
    * Redis可排序集合类似于Redis集合，是不重复的字符集合。 不同之处在于，排序集合的每个成员都与分数相关联，这个分数用于按最小分数到最大分数来排序的排序集合。虽然成员是唯一的，但分数值可以重复。
    * 相关命令：```zadd```, ```zrange```, ```zrangebyscore```
        ```bash
        127.0.0.1:6379> zadd zset_test 0 redis
        (integer) 1
        127.0.0.1:6379> zadd zset_test 0 mongodb
        (integer) 1
        127.0.0.1:6379> zadd zset_test 1 sqllite
        (integer) 1
        127.0.0.1:6379> zadd zset_test 1 sqllite
        (integer) 0
        127.0.0.1:6379> zrange zset_test 0 1000
        1) "mongodb"
        2) "redis"
        3) "sqllite"
        127.0.0.1:6379> zrangebyscore zset_test 0 1000
        1) "mongodb"
        2) "redis"
        3) "sqllite"
        ```

## 参考
* [Redis数据类型 - Redis教程](http://www.yiibai.com/redis/redis_data_types.html)