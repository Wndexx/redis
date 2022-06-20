## 集合 set

### 一、简介

set 与 list 类似，区别在于 set 可以`自动排重`

set 提供了判断某个成员是否在一个 set 集合内的重要接口，这个也是 list 所没有的

set 是 string 类型的`无序`集合

set 底层是一个 `value 为 null 的 hash 表`，所以添加、删除、查找的复杂度都是 O(1)



### 二、常用命令

#### 2.1 sadd

`sadd <key> <value1> <value2> ...`

> 将一个或多个 member 元素加入到集合 key 中，已经存在的 member 元素将被忽略

```bash
127.0.0.1:6379> sadd k1 v1 v2 v1 v3
(integer) 3
```



#### 2.2 smembers

`smembers <key>`

> 取出该集合的所有值

```bash
127.0.0.1:6379> smembers k1
1) "v3"
2) "v2"
3) "v1"
```



#### 2.3 sismember

`sismember <key> <value>`

> 判断 key 所对应的集合中是否含有该 value 值，1 表示有，0 表示没有

```bash
127.0.0.1:6379> sismember k1 v3
(integer) 1
127.0.0.1:6379> sismember k1 v4
(integer) 0
```



#### 2.4 scard

`scard <key>`

> 返回 key 对应的集合的元素个数

```bash
127.0.0.1:6379> scard k1
(integer) 3
```



#### 2.5 srem

`srem <key> <value1> <value2> ...`

> 删除集合中的某些元素

```bash
127.0.0.1:6379> srem k1 v1 v2
(integer) 2
127.0.0.1:6379> smembers  k1
1) "v3"
```



#### 2.6 spop

`spop <key>`

> 随机从集合中吐出一个值，会从集合中删除，当值为空时，键消失

```bash
127.0.0.1:6379> spop k1
"v3"
127.0.0.1:6379> keys *
(empty array)
```



#### 2.7 srandmember

`srandmember <key> <n>`

> 随机从该集合中取出 n 个值，不会从集合中删除

```bash
127.0.0.1:6379> sadd k1 v1 v2 v3
(integer) 3
127.0.0.1:6379> srandmember k1
"v2"
127.0.0.1:6379> smembers k1
1) "v3"
2) "v2"
3) "v1"
```



#### 2.8 smove 

`smove <src> <dest> <value>`

> 把集合中的一个值移动到另一个集合

```bash
127.0.0.1:6379> sadd k1 v11 v12 v13
(integer) 3
127.0.0.1:6379> sadd k2 v21 v22 v23
(integer) 3
127.0.0.1:6379> smove k1 k2 v13
(integer) 1
127.0.0.1:6379> smembers k1
1) "v11"
2) "v12"
127.0.0.1:6379> smembers k2
1) "v23"
2) "v13"
3) "v21"
4) "v22"
```



#### 2.9 sinter

`sinter <key1> <key2>`

> 返回两个集合的交集元素

```bash
127.0.0.1:6379> sadd k1 v1 v2 v3 v5
(integer) 4
127.0.0.1:6379> sadd k2 v2 v4 v6
(integer) 3
127.0.0.1:6379> sinter k1 k2
1) "v2"
```



#### 2.10 sunion

`sunion <key1> <key2>`

> 返回两个集合的并集部分

```bash
127.0.0.1:6379> sunion k1 k2
1) "v5"
2) "v3"
3) "v4"
4) "v2"
5) "v1"
6) "v6"
```



#### 2.11 sdiff

`sdiff <key1> <key2>`

> 返回两个集合的差集部分（key1 有，key2 没有）

```bash
127.0.0.1:6379> sdiff k1 k2
1) "v5"
2) "v1"
3) "v3"
```



### 三、数据结构

set 数据结构是 `dict 字典`，字典使用`哈希表`实现的

内部使用 hash 结构，所有的 value 都指向同一个内部值



































