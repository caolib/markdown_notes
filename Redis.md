---
title: Redis
date: 2023-08-31 20:59:08
categories: 数据库
tags: 
  - redis
  - 数据库
  - NOSQL
---

# [Redis](https://redis.io/)

## 什么是NOsql

> `NOSQL` -> `not only sql`,不仅仅是sql(不是没有sql)

Nosql特点:

1. 方便拓展（数据之间没有关系，很好拓展！）
2. 大数据量高性能（redis一秒写8万次，读取11万次，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高！）
3. 数据类型是多样的！（不需要事先设计数据库！随取随用！如果是数据量非常大的表，关系型数据库就很难设计了！）
4. 传统==RDBMS==(关系型数据库管理系统)和==NoSQL==

RDBMS的特点：

- 基于关系模型，使用表格的存储方式，数据按照行和列进行组织。
- 使用SQL语言进行数据的查询和操作，SQL语言是一种通用的、标准化的、结构化的语言，可以进行复杂的查询和分析。
- 强调ACID规则（原子性、一致性、隔离性、持久性），可以保证数据的完整性和一致性，适合处理高要求的事务操作。
- 通常只能进行纵向扩展，即增加单个服务器的硬件资源来提高性能，这种方式成本高昂且有上限。
- 适合处理结构化或半结构化的数据，需要进行复杂查询或分析的场景，例如金融、电商、教育等领域。



NoSQL的特点：

- 不基于关系模型，使用键值对、文档、图形等多种存储方式，数据的结构和格式可以灵活变化，不需要预先定义。
- 使用非结构化查询语言（UnQL）或者特定的API进行数据的访问和操作，UnQL没有统一的标准，每种NoSQL数据库都有自己的语法和规则。
- 通常只提供弱一致性或最终一致性的保证，不能支持复杂的事务操作，但可以提高数据的可用性和并发性。
- 通常可以进行横向扩展，即增加多个服务器来分担数据和负载，这种方式成本低廉且可以无限扩展。
- 适合处理非结构化或多变的数据，需要高并发或海量数据存储的场景，例如社交网络、游戏、物联网等领域。

大数据时代的3V:主要是描述问题的

1. 海量Volume
2. 多样Variety
3. 实时Velocity

大数据时代的3高:主要是对程序的要求

1. 高并发
2. 高可扩
3. 高性能

实际开发项目一般都是 NoSQL+RDBMS 搭配使用

---

## redis入门

### 概述

> redis是什么？

1. Redis（==Re==mote ==Di==ctionary ==S==erver )，即远程字典服务
2. 是一个开源的使用ANSI ==C语言==编写、支持网络、可基于内存亦可持久化的日志型、==Key-Value==数据库，并提供多种语言的API
3. redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave==(主从)同步==
4. 免费和开源，是当下最热门的NoSQL技术之一

> redis能做什么？

1. 内存存储、==持久化==，内存是断电即失的，所以说持久化很重要！
2. 效率高，可以用于高速缓存
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计数器（浏览量）

> 特性

1. 多样的数据类型
2. 持久化
3. 事务
4. 集群

### 基本知识

> 登录redis客户端redis-cli:computer:

```bash
# 直接登录 -h ip地址 -p 端口号 -a 密码(不用密码也能登陆，但是没有权限)
redis-cli -h localhost -p 6379 -a 123456

# 推荐登录后再用密码进行认证
# 1.登录
redis-cli (-h localhost -p 6379 如果是本地登陆可以省略)

# 2.身份认证 OK >>> 成功
> auth 123456

# 查看命令的帮助信息
help [command]
```

<img src="https://github.com/TankingCao/picx-images-hosting/raw/master/CopyQ.PGpyFa.13e3fgmo8pb4.webp" alt="CopyQ" />

> **redis有==16==个数据库(0-15)，默认使用的是==0==，可以使用select切换数据库**

切换数据库 `select [index]`

```bash
# 切换数据库为1号
select 1
```

查看当前数据库大小`dbsize`

```bash
dbsize
> (integer) 1
```

设置键值对`set [key] [value]`

```bash
set name clb
> OK
```

根据键获取值`get [key]`

```bash
get name
> "clb"
```

查找所有适配的key `keys [pattern] `

```bash
# 查询所有
keys *
> 1) "name"
> 2) "age"
```

清空当前数据库`flushdb`

```bash
flushdb
```

清空所有数据库`flushall`

```bash
flushall
```

---

**==redis是单线程的！==**

官方表示，redis是基于内存操作，CPU不是redis的性能瓶颈，redis的性能瓶颈是机器的内存大小和网络带宽，既然单线程更容易实现，那就顺理成章的使用单线程了

==**redis为什么单线程还这么快？**==

误区：

- 高性能的服务器一定是多线程的？
- 多线程（CPU上下文会切换）一定比单线程效率高？

==核心==：redis是将所有数据全部放在内存中的，所以说使用单线程去操作效率就是最高的，多线程（CPU上下文会切换：耗时的操作！！！），对于内存系统来说，如果没有上下文切换效率就是最高的！多次读写操作都是在一个CPU上的，在内存情况下，这个就是最佳的方案！

---

### Redis-Key

查看一个或者多个key是否存在，返回个数`exists [key...]`

<img src="https://s2.loli.net/2024/09/27/R2JGgMxC94EhyTk.webp" alt="CopyQ" />

设置过期时间(多长时间后过期自动从数据库删除)`expire [key] [time] 单位默认是秒`

> ```bash
> expire name 3	# 3秒后过期，过期了数据就没了
> ```
>

查看key的剩余时间`ttl [key]`

```bash
ttl name
> 2
```

移动key到其他数据库`move [key] [db]`

```bash
move name 15
```

删除一个或多个key`del [key...]`

```bash
del name
del name age
```

查看key的类型`type [key]`,不存在返回none

```bash
type name 
> string
```

---

## 五大数据类型

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作==数据库==、==缓存==和==消息中间件==，它支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）

---

### String

==字符串==

set、get、keys、exists同上

追加字符串`append [key] [value]` 如果name不存在，就等同于set方法

```bash
append name nb
> (integer) 5
```

查询字符串长度`strlen [key]`

```bash
strlen name
> (integer) 5
```

---

增加和减少操作，前提是key对应的值可以转化成integer，否则报错

自增(++) `incr [key]`,自减(–)`decr [key]`

```bash
get views
> "3"

incr views
> (integer) 4

decr views
> (integer) 3
```

增加(+=)`incrby [key] [步长]`，减少(-=)`decrby [key] [步长]`

```bash
get views
> "10"

INCRBY views 5
> (integer) 15

DECRBY views 3
> (integer) 12
```

>  获取字符串的子串`getrange [key] [start] [end]`

注意：==闭区间==（开始索引和结束索引都包含），==逆序索引从-1开始(倒数第1个索引为-1，倒数第2索引为-2…)==

```bash
get k
> "hello world!"

getrange k 0 4	#[0,4]
> "hello"

getrange k 0 -1	#获取全部，0表示头部，-1表示尾部
"hello world!"
```

> 替换字符串`setrange [key] [start] [value]`,从start索引开始，用value直接覆盖后面的值，如果原来字符串长度不够，那么补0增加长度后再进行覆盖

```bash
127.0.0.1:6379> get str
"12345"

127.0.0.1:6379> setrange str 1 abcdefg
(integer) 8

127.0.0.1:6379> get str
"1abcdefg"
```

设置key-value并指定过期时间`setex [key] [seconds] [value]`

```bash
127.0.0.1:6379> setex name 10 clb
OK
127.0.0.1:6379> ttl name
(integer) 7
```

> ==设置key-value==`setnx [key] [value]`

和set的区别：==如果key已经存在，set方法直接覆盖原来的值，而setnx不会==

```bash
127.0.0.1:6379> setnx name cxk
(integer) 1
127.0.0.1:6379> setnx name ikun
(integer) 0	#key已经存在，不覆盖
127.0.0.1:6379> get name
"cxk"
```

---

批量设置`mset [key value ...]`

```bash
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3
OK
127.0.0.1:6379> keys *
1) "k3"
2) "k1"
3) "k2"
```

批量获取`mget [key ...]`

> 批量设置`msetnx [key value ...]`msetnx是一个==**原子性**==操作，要么全部成功，要么全部失败,只要有一个key存在，整个操作全部失败

```bash
127.0.0.1:6379> mget k1 k2 k3
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 666 k4 999
(integer) 0		# k1存在，整个操作失败，k4也未赋值
127.0.0.1:6379> mget k1 k2 k3 k4
1) "v1"
2) "v2"
3) "v3"
4) (nil)
```

---

设置一个对象user:1 值用一个json字符串来表示，但是这样不能直接获取到name属性，所以要单独赋值

```sql
set user:1 {name:zhangsan,age:19}
```

> 这里的key的设计：`user:{id}:{filed}`非常巧妙(这样就能直接获取一个user对象中的各种属性，类似一种层次结构)

```sql
127.0.0.1:6379> mset user:1:name zhangsan user:1:age 19
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "19"
```

---

> getset方法`getset [key] [value]`获取key原来对应的value，设置一个新的值，返回原来被替换的值，如果本来就不存在，返回nil

```sql
127.0.0.1:6379> getset name ikun
(nil)
127.0.0.1:6379> getset name kunkun
"ikun"
127.0.0.1:6379> get name
"kunkun"
```

---

### List

==列表==

插入值

1. 从左边插入（头插法）`lpush [key] [value ...]`
2. 从右边插入（尾插法）`rpush [key] [value ...]`
3. 从左边开始遍历`lrange [start] [end]`

```sql
127.0.0.1:6379> lpush list 1 2 3 
(integer) 3
127.0.0.1:6379> lrange list 0 -1 # -1代表尾部，所以是遍历全部
1) "3"
2) "2"
3) "1"
127.0.0.1:6379> rpush list 4 5 6
(integer) 6
127.0.0.1:6379> lrange list 0 -1
1) "3"
2) "2"
3) "1"
4) "4"
5) "5"
6) "6"
```

移除元素

1. 从左边开始移除`lpop [key] [count个数] `
2. 从右边开始移除`rpop [key] [count个数] `

```sql
127.0.0.1:6379> lrange list 0 -1
1) "3"
2) "2"
3) "1"
4) "4"
5) "5"
6) "6"
127.0.0.1:6379> lpop list 2
1) "3"
2) "2"
127.0.0.1:6379> rpop list 3
1) "6"
2) "5"
3) "4"
127.0.0.1:6379> lrange list 0 -1
1) "1"
```

获取列表中指定索引的值`lindex [key] [index]`

```sql
127.0.0.1:6379> lrange lst 0 -1
1) "5"
2) "4"
3) "3"
4) "2"
5) "1"
127.0.0.1:6379> lindex lst 1
"4"
```

获取列表长度`llen [key]`

```sql
127.0.0.1:6379> lrange lst 0 -1
1) "5"
2) "4"
3) "3"
4) "2"
5) "1"
127.0.0.1:6379> llen lst
(integer) 5
```

> 删除列表中n个指定元素(精确匹配)`lrem [key] 3 2` >>> 删除list列表中从左往右数的前3个2，返回值为成功删除的个数

```sql
127.0.0.1:6379> lrange list 0 -1
1) "2" 
2) "10"
3) "9"
4) "6"
5) "2"
6) "2"
7) "2"
8) "1"
9) "2"
127.0.0.1:6379> lrem list 3 2
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "10"
2) "9"
3) "6"
4) "2"
5) "1"
6) "2"
```

截取列表指定索引之间的元素`ltrim [key] [start] [stop]` 截取[start,stop],==这是在原数组上进行修改!==

```sql
127.0.0.1:6379> lrange list 0 -1
1) "a"
2) "b"
3) "c"
4) "d"
5) "e"
6) "ok"
127.0.0.1:6379> ltrim list 0 2
OK
127.0.0.1:6379> lrange list 0 -1
1) "a"
2) "b"
3) "c"
```

> 移除arr1中最右边的元素并从左边插入arr2`rpoplpush [source] [destination]`

```sql
127.0.0.1:6379> lrange arr1 0 -1
1) "1"
2) "2"
3) "3"
127.0.0.1:6379> lrange arr2 0 -1
1) "a"
2) "b"
3) "c"
127.0.0.1:6379> rpoplpush arr1 arr2
"3"
127.0.0.1:6379> lrange arr1 0 -1
1) "1"
2) "2"
127.0.0.1:6379> lrange arr2 0 -1
1) "3"
2) "a"
3) "b"
4) "c"
```

根据索引设置列表中的元素`lset [key] [index] [value]`，相当于更新操作

列表不存在或索引超出范围(可以使用负数表示逆序索引)都会报错

```sql
127.0.0.1:6379> lrange list 0 -1
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> lset list 1 666
OK
127.0.0.1:6379> lrange list 0 -1
1) "v1"
2) "666"
3) "v3"
```

在列表中从左往右数第1个pivot前面/后面插入指定元素

`linsert [key] before|after [pivot] [element]`

```sql
127.0.0.1:6379> lrange list 0 -1
1) "kun"
2) "cxk"
3) "clb"
4) "kun"
5) "nb"
127.0.0.1:6379> linsert list before kun 666 # 从左往右第一个kun前插入666
(integer) 6
127.0.0.1:6379> lrange list 0 -1
1) "666"
2) "kun"
3) "cxk"
4) "clb"
5) "kun"
6) "nb"
127.0.0.1:6379> linsert list after nb ctrl # 从左往右第一个nb后面插入ctrl
(integer) 7
127.0.0.1:6379> lrange list 0 -1
1) "666"
2) "kun"
3) "cxk"
4) "clb"
5) "kun"
6) "nb"
7) "ctrl"
```

> 小结：List实际上是一个==双向链表==结构，在两边crud快，中间元素crud相对较慢



---

### Set

==集合，不能有重复元素==

set集合添加元素`sadd [key] [member ...]`,添加重复元素无效

```sql
127.0.0.1:6379> sadd set hello ikun cxk ctrl hello
(integer) 4 # 因为set不允许有重复元素，所以最后一个hello未被添加
```

set查看所有元素`smembers [key]`

```sql
127.0.0.1:6379> smembers set
1) "ctrl"
2) "ikun"
3) "hello"
4) "cxk"
```

set是否包含元素`sismember [key] [member]`

```sql
127.0.0.1:6379> sismember set ikun
(integer) 1
```

获取set中元素个数`scarg set`

```sql
127.0.0.1:6379> scard set
(integer) 4
```

移除set中指定元素`srem [key] [member]`

```sql
127.0.0.1:6379> srem set ikun
(integer) 1
```

从set中随机移除几个元素`spop [key] [count:个数超出上限则全部移除]`

```sql
127.0.0.1:6379> spop set 1
1) "5"
127.0.0.1:6379> spop set 2
1) "4"
2) "1"
```

从set中随机取出几个元素`srandmember [key] [count:个数超出上限则取出全部]`

```sql
127.0.0.1:6379> srandmember set 1
1) "1"
127.0.0.1:6379> srandmember set 1
1) "4"
127.0.0.1:6379> srandmember set 2
1) "1"
2) "4"
127.0.0.1:6379> srandmember set 3
1) "2"
2) "1"
3) "5"
```

从一个集合中移动指定元素到另外一个集合`smove [source] [destination] member`

```sql
127.0.0.1:6379> sadd s1 ikun cxk
(integer) 2
127.0.0.1:6379> sadd s2 ctrl rap
(integer) 2
127.0.0.1:6379> smove s1 s2 ikun
(integer) 1
127.0.0.1:6379> SMEMBERS s2
1) "ctrl"
2) "ikun"
3) "rap"
```

差集、交集、并集`sdiff|sinter|sunion [set1] [set2...]`

```sql
127.0.0.1:6379> sadd s1 a b c d
(integer) 4
127.0.0.1:6379> sadd s2 c d e f
(integer) 4
127.0.0.1:6379> sdiff s1 s2 # 差集
1) "a"
2) "b"
127.0.0.1:6379> sdiff s2 s1 # 差集
1) "e"
2) "f"
127.0.0.1:6379> sinter s1 s2 # 交集
1) "d"
2) "c"
127.0.0.1:6379> sunion s1 s2 # 并集
1) "f"
2) "b"
3) "a"
4) "e"
5) "d"
6) "c"
```

---

### Hash

==键值对，相当于map==

设置`hset|hmset [hash] [key value ...]`hset从redis4开始支持批量设置

获取

1. 根据key获取一个 值`hget [hash] [key]`
2. 根据多个key获取多个值`hmget [hash] [key ...]`
3. 获取所有键和值`hgetall [hash]`

```sql
127.0.0.1:6379> hset hash k1 v1 k2 v2 k3 v3
(integer) 3
127.0.0.1:6379> hget hash k1
"v1"
127.0.0.1:6379> hgetall hash
1) "k1"
2) "v1"
3) "k2"
4) "v2"
5) "k3"
6) "v3"
```

根据键删除k-v`hdel [hash] [key ...]`

```sql
127.0.0.1:6379> hdel hash k1
(integer) 1
```

获取键值对个数`hlen [hash]`

```sql
127.0.0.1:6379> hlen hash
(integer) 2
```

获取所有的key`hkeys [hash]`

```sql
127.0.0.1:6379> hkeys hash
1) "k2"
2) "k3"
```

获取所有的value`hvals [hash]`

```sql
127.0.0.1:6379> hvals hash
1) "v2"
2) "v3"
```

`hsetnx` `hincrby`用法同string

> hash更多应用于存储用户信息！

---

### Zset

==有序集合==，在set基础上增加了一个score，默认根据score升序排序

```sql
127.0.0.1:6379> zadd set -6 ikun 66 kun 6 kunkun 0 ctrl
(integer) 4
127.0.0.1:6379> zrange set 0 -1
1) "ikun"
2) "ctrl"
3) "kunkun"
4) "kun"
```

获取score在-inf(负无穷,可以改为具体数值)到+inf之间元素并带有score(升序)

`zrangebyscore [set] [-inf] [+inf] (withscores可以不用)`

```sql
127.0.0.1:6379> zrangebyscore set -inf +inf withscores
1) "ikun"
2) "-6"
3) "ctrl"
4) "0"
5) "kunkun"
6) "6"
7) "kun"
8) "66"
```

根据score逆序获取，range前面加`rev`

```sql
127.0.0.1:6379> zrevrange set 0 -1
1) "kun"
2) "kunkun"
3) "ctrl"
4) "ikun"
127.0.0.1:6379> ZREVRANGEBYSCORE set +inf -inf withscores
1) "kun"
2) "66"
3) "kunkun"
4) "6"
5) "ctrl"
6) "0"
7) "ikun"
8) "-6"
```

> 获取有序集合元素个数`zcard [set]`

> 统计score在区间内的元素个数`zcount [set] [min] [max]`包含两端

---

## 三种特殊数据类型

### [Geospatial(地理位置)](http://www.redis.cn/commands/geoadd.html)

> ==**geoadd**== 添加一个或多个城市信息`geoadd [key] [经度 纬度 member ...]`
>
> :rocket: ==**这里的key实际上是zset(有序集合),zset方法可以操作这里的key**==

```bash
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.50 29.53 chongqing
(integer) 1
```

>  查询所有位置信息

```bash
127.0.0.1:6379> zrange china:city 0 -1
1) "chongqing"
2) "xian"
3) "shenzhen"
4) "hangzhou"
5) "shanghai"
6) "beijing"
```

> ==**geopos**== 查询一个或多个城市经度和纬度`geopos [key] [member ...]`

```bash
127.0.0.1:6379> geopos china:city beijing shanghai
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
2) 1) "121.47000163793563843"
   2) "31.22999903975783553"
```

> ==**geodist**==计算两地之间的距离`geodist [key] [member1] [member2] [unit:单位,默认为米]`

- **m** 表示单位为米(默认使用)
- **km** 表示单位为千米
- **mi** 表示单位为英里
- **ft** 表示单位为英尺

```bash
127.0.0.1:6379> geodist china:city beijing shanghai
"1067378.7564"
127.0.0.1:6379> geodist china:city beijing shanghai km
"1067.3788"
```

> ==**georadius**== 以指定点为半径，查找位置在方圆半径内的元素(元素来源于key)，==返回结果按照距离升序排序==
>
> `georadius [key] [经度 纬度 距离 单位] [withcoord:输出经纬度] [withdist:输出距离] [count 个数:指定查找的最多个数] `

```bash
# 查找以经纬度坐标(110,30)为中心，方圆500km内的城市信息，只找最近的两个(只找出在china:key中录入的城市)
127.0.0.1:6379> georadius china:city 110 30 500 km withcoord withdist count 2
1) 1) "chongqing"
   2) "341.9374"
   3) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) "483.8340"
   3) 1) "108.96000176668167114"
      2) "34.25999964418929977"
```

> **==georadiusmember==** 根据已存在点作为中心查找范围内的元素,用法同上
>
> `georadiusbymember [key] [member] 200 km`

```bash
127.0.0.1:6379> georadiusbymember china:city shanghai 200 km
1) "hangzhou"
2) "shanghai"
```

> geohash返回一个位置的geohash字符串(11位字符) `geohash [key] [mamber ...]`

```bash
127.0.0.1:6379> geohash china:city beijing shanghai
1) "wx4fbxxfke0"
2) "wtw3sj5zbj0"
```

---

### Hyperloglog

==元素不重复，类似于set集合，但是占用内存更小，不过数据量大的时候有一定误差==

> 添加元素  `pfadd [key] [value ...]`
>
> 统计不重复元素个数 `pfcount [key]`
>
> 合并多个集合 `pfmege [destination] [source ...]`

```bash
127.0.0.1:6379> pfadd set1 1 2 3 4 5
(integer) 1
127.0.0.1:6379> pfadd set2 4 5 6 7 8
(integer) 1
127.0.0.1:6379> pfmerge set3 set1 set2
OK
127.0.0.1:6379> pfcount set3
(integer) 8
```

---

### Bitmap(位图)

设置一周打卡信息(1 打卡，0 未打卡)

> `setbit [key] [offset:int数字，此处表示周几] [status 0/1]`

```bash
127.0.0.1:6379> setbit sign 1 1
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 1
(integer) 0
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 1
(integer) 0
127.0.0.1:6379> setbit sign 7 0
(integer) 0
```

> 获取某天状态 `getbit [key] [offset]`

```bash
127.0.0.1:6379> getbit sign 4
(integer) 1
```

> 统计状态为1的个数 `bitcount [key]`

```bash
127.0.0.1:6379> bitcount sign
(integer) 4
```

---

## 事务

redis事务本质：一组命令的集合，有一致性、顺序性、排他性，

==redis事务没有隔离级别的概念==，所有命令在事务中并没有直接被执行，只有发起执行命令的时候才会执行

**==redis单条命令保证原子性，但是事务不保证原子性==**

> redis的事务

- 开始事务（`multi` 开启事务之后输入的命令不会直接执行，而是进入命令队列，直到事务被执行，所有的命令才按顺序执行）
- 命令入队（`...`）
- 执行事务（`exec`） 

```bash
127.0.0.1:6379> multi		# 开启事务
OK
# 命令入队
127.0.0.1:6379(TX)> mset k1 v1 k2 v2 k3 v3
QUEUED
127.0.0.1:6379(TX)> mget k1 k2
QUEUED
127.0.0.1:6379(TX)> exec		# 执行事务
1) OK
2) 1) "v1"
   2) "v2"
```

> 放弃事务（`discard`命令入队过程中使用可以放弃事务）

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 1231
QUEUED
127.0.0.1:6379(TX)> discard
OK
```

> ==编译型异常==（命令有问题，过不了编译），事务中所有命令不执行！

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> setttttt k2 v2		# 不存在的命令导致编译异常
(error) ERR unknown command 'setttttt', with args beginning with: 'k2' 'v2' 
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> exec
(error) EXECABORT Transaction discarded because of previous errors.# 之前的编译异常导致整个事务被放弃
```



> ==运行时异常==（除0错误，索引越界…）错误命令不执行，其他命令正常执行，==不能保证原子性==

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> incr k1		# v1不能转换为integer类型，运行时异常，编译没问题
QUEUED
127.0.0.1:6379(TX)> get k1
QUEUED
127.0.0.1:6379(TX)> exec
1) OK
2) (error) ERR value is not an integer or out of range
3) "v1"
```

> ==**悲观锁**==：很悲观，认为什么时候都会出问题，无论做什么都会加锁

> ==**乐观锁**==：
>
> - 很乐观，认为什么时候都不会出现问题，所以不会上锁，更新数据的时候去判断一下，在此期间是否有人修改过这个数据
> - 获取version
> - 更新的时候比较version

使用watch当做redis的乐观锁操作 `watch [key]`

```bash
# 多线程并发修改
# --------------线程1--------------
127.0.0.1:6379> watch money # 监视，想当于加乐观锁
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> incrby money 666
QUEUED
# exec还未执行时，线程2开始执行
127.0.0.1:6379(TX)> exec
(nil)	# 执行失败

# --------------线程2--------------
127.0.0.1:6379> set money -6	# 修改了线程1中加了乐观锁的money
OK
```

执行失败后使用`unwatch`解锁后再重新监视并执行

---

## Jedis

==官方推荐==的java连接开发工具，使用java操作redis的中间件，如果要使用java操作redis，那么一定要对jedis十分的熟悉！

1. 导入jedis依赖

```xml
<!--导入jedis依赖-->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>4.4.3</version>
</dependency>
```

2. 创建jedis对象

```java
//创建一个Jedis对象并认证
jedis = new Jedis("192.168.66.6", 6379);
jedis.auth("123456");
```

3. 使用jedis对象调用API,jedis的API基本都是对应redis中的命令

```java
jedis.flushDB();
System.out.println(jedis.set("name", "clb"));
System.out.println(jedis.set("password", "123456"));
System.out.println(jedis.exists("name"));
```

> 事务操作

```java
#事务
@Test
public void testTX() {
    JSONObject json = new JSONObject();
    json.put("name", "cxk");
    json.put("age", "34");
    String result = json.toJSONString();
    jedis.flushDB();

    jedis.watch(result);
    //开启事务
    Transaction multi = jedis.multi();
    try {
        multi.set("user1", result);
        int i = 1 / 0; // 制造一个运行时异常
        multi.set("user2", result);
        multi.exec(); //执行事务
    } catch (Exception e) {
        multi.discard();//放弃事务
        throw new RuntimeException(e);
    } finally {
        System.out.println(jedis.get("user1"));
        System.out.println(jedis.get("user2"));
        jedis.close(); //关闭连接
    }
}
```

---

## SpringBoot整合

SpringBoot2.x之后，原来使用的jedis被替换成了lettuce

==jedis==：采用直连，多个线程池操作的话，是不安全的，如果想要避免，就要使用`jedis pool`连接池，更像`BIO`（阻塞IO）模式

==**lettuce**==：采用`netty`，实例可以在多个线程中进行共享，不存在线程不安全的情况！可以减少线程数据，更像`NIO`（非阻塞IO）模式

1. 导入redis依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

2. 配置文件配置redis

```yaml
spring:
  data:
    redis:
      host: 192.168.66.6
      port: 6379
      password: 123456
```

3. 测试

```java
@SpringBootTest
class Redis02SpringbootApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;
    @Test
    void contextLoads() {
        //方法和redis命令一一对应
        //opsForValue()   操作String
        //opsForList()    操作List
        //......
        redisTemplate.opsForValue().set("name", "坤坤");
      System.out.println(redisTemplate.opsForValue().get("name"));
    }
}
```

redis中各种数据类型对应的操作方法

<img src="https://s2.loli.net/2024/09/27/Iy2QGasZbTcLlYW.jpg" alt="1693648470356" />

4. ==序列化== ：
   - 在Spring Boot中，序列化是指==将对象转换为可以存储或传输的字节序列的过程==。这可以通过将对象转换为==JSON==或==XML==格式来实现。序列化允许在存储或传输对象时==减少内存使用==，并确保在在不同系统之间交换数据时能够正确表示对象
   - 实体类一般都要序列化，`implements Serializable`实现序列化接口就可以了，默认使用的是JDK序列化,也可以使用其他json工具实现，因为默认的jdk序列化存储中文到redis会出现乱码

---
