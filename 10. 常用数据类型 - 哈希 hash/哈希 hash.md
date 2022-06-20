## 哈希 hash

### 一、简介

hash 是一个 string 类型的 `field` 和 `value` 的映射表，即一个键值对集合，特别适合`存储对象`



### 二、常用命令

#### 2.1 hset

`hset <key> <field> <value>`

> 给 key 集合中的 field 赋值 value

```bash
127.0.0.1:6379> hset user:1001 id 1001 name zs age 20
(integer) 3
```



#### 2.2 hmset

`hmset <key> <field1> <value1> <field2> <value2> ...`

> 批量设置 hash 的值

```bash
127.0.0.1:6379> hmset user:1002 id 1002 name ls age 30
OK
127.0.0.1:6379> hget user:1002 id
"1002"
```



#### 2.3 hsetnx

`hsetnx <key> <field> <value>`

> 将 hash 中域 field 的值设置为 value，当且仅当 域 field 不存在

```bash
# 0 表示设置失败，1 表示设置成功
127.0.0.1:6379> hsetnx user:1001 id 1002
(integer) 0
127.0.0.1:6379> hsetnx user:1001 gender 1
(integer) 1
```



#### 2.4 hget

`hget <key> <field>`

> 从 key 对应的集合中取出 field 对应的 value

```bash
127.0.0.1:6379> hget user:1001 name
"zs"
```



#### 2.5 hexists

`hexists <key> <field>`

> 查看 key 所对应的 hash 中，是否存在 field

```bash
# 0 表示不存在，1 表示存在
127.0.0.1:6379> hexists user:1001 gerder
(integer) 0
```



#### 2.6 hkeys

`hkeys <key>`

> 列出 key 所对应的 hash 中的所有 field

```bash
127.0.0.1:6379> hkeys user:1001
1) "id"
2) "name"
3) "age"
```



#### 2.7 hvals

`hvals <key>`

> 列出 key 所对应的 hash 中的所有 value

```bash
127.0.0.1:6379> hvals user:1001
1) "1001"
2) "zs"
3) "20"
```



#### 2.8 hincrby

`hincrby <key> <field> <increment>`

> 为 hash 中域 field 的值加上增量 increment

```bash
127.0.0.1:6379> hincrby user:1001 age 5
(integer) 25
127.0.0.1:6379> hget user:1001 age
"25"
```



### 三、数据类型

hash 类型对应的数据结构是 `zipList（压缩列表）`和 `hashtable（哈希表）`

当 field-value 的长度较短且个数较少时，使用 zipList；否则使用 hashtable





















