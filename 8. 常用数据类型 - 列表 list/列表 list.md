## 列表 list

### 一、简介

特点：`单键多值`

redis 列表是简单的字符串列表，按照插入顺序排序，可以添加一个元素到列表的头部（左边）或者尾部（右边）

redis 列表底层是`双向链表`，对两端的操作性能很高，对通过索引下标操作中间节点的性能较差



### 二、常用命令

#### 2.1 lpush/rpush

`lpush/rpush <key> <value1> <value2> <value3> ...`

> 从左边/右边插入一个或多个值

```bash
127.0.0.1:6379> lpush k1 v1 v2 v3
(integer) 3
127.0.0.1:6379> lrange k1 0 -1
1) "v3"
2) "v2"
3) "v1"

127.0.0.1:6379> rpush k2 v1 v2 v3
(integer) 3
127.0.0.1:6379> lrange k2 0 -1
1) "v1"
2) "v2"
3) "v3"
```



#### 2.2 lpop/rpop

`lpop/rpop <key>`

> 从左边/右边吐出一个值。值在键在，值光键亡

```bash
127.0.0.1:6379> lpop k1
"v3"
127.0.0.1:6379> rpop k1
"v1"
```



#### 2.3 rpoplpush 

`rpoplpush <key1> <key2>`

> 从 <key1> 列表右边吐出一个值，插到 <key2> 列表的左边

```bash
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> lpush k1 v1 v2 v3
(integer) 3
127.0.0.1:6379> rpush k2 v1 v2 v3
(integer) 3
127.0.0.1:6379> rpoplpush k1 k2
"v1"
127.0.0.1:6379> lrange k1 0 -1
1) "v3"
2) "v2"
127.0.0.1:6379> lrange k2 0 -1
1) "v1"
2) "v1"
3) "v2"
4) "v3"
```



#### 2.4 lrange

`lrange <key> <start> <stop>`

> 按照索引下标获得元素（从左到右）

```bash
127.0.0.1:6379> lpush k1 v1 v2 v3
(integer) 3
127.0.0.1:6379> lrange k1 0 1
1) "v3"
2) "v2"
```



`lrange <key> 0 -1`

> 从左到右列出 key 对应值的所有元素

```bash
127.0.0.1:6379> lrange k1 0 -1
1) "v3"
2) "v2"
3) "v1"
```



#### 2.5 lindex

`lindex <key> <index>`

> 按照索引下标获得元素（从左到右）

```bash
127.0.0.1:6379> lindex k1 1
"v2"
```



#### 2.6 llen

`llen <key>`

> 获得列表长度

```bash
127.0.0.1:6379> llen k1
(integer) 3
```



#### 2.7 linsert

`linsert <key> before/after <value> <newvalue>`

> 在 <value> 的前面/后面插入 <newvalue>

```bash
127.0.0.1:6379> linsert k1 before v2 v11
(integer) 4
127.0.0.1:6379> lrange k1 0 -1
1) "v3"
2) "v11"
3) "v2"
4) "v1"

127.0.0.1:6379> linsert k1 after v2 v22
(integer) 5
127.0.0.1:6379> lrange 0 -1
1) "v3"
2) "v11"
3) "v2"
4) "v22"
5) "v1"
```



#### 2.8 lrem

`lrem <key> <n> <value>`

> 从左边删除 n 个 value（从左到右）

```bash
127.0.0.1:6379> lpush k1 v1 v0 v2 v0 v3 v0
(integer) 6
127.0.0.1:6379> lrange k1 0 -1
1) "v0"
2) "v3"
3) "v0"
4) "v2"
5) "v0"
6) "v1"
127.0.0.1:6379> lrem k1 2 v0
(integer) 2
127.0.0.1:6379> lrange k1 0 -1
1) "v3"
2) "v2"
3) "v0"
4) "v1"
```



#### 2.9 lset

`lset <key> <index> <value>`

> 将 key 对应的列表下标为 index 的值替换成 value

```bash
127.0.0.1:6379> lset k1 2 v11
OK
127.0.0.1:6379> lrange k1 0 -1
1) "v3"
2) "v2"
3) "v11"
4) "v1"
```



### 三、数据结构

list 的数据结构为`快速列表 quickList`

如果列表元素较少，会使用`压缩列表 zipList`，即分配一块连续的内存，将所有的元素紧挨着存储

如果列表元素较多，会使用 `快速列表 quicklist`



普通的链表（linkedList）需要的附加指针所占空间太大

redis 将 linkedList 和 zipList 结合起来组成了 quickList，也就是将多个 zipList 使用双向指针串起来使用

这样既满足了快速的插入删除要求，又不会出现太大的空间冗余

![1655693736718](列表 list.assets/1655693736718.png)





















































