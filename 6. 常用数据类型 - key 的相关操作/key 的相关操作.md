## key 键的相关操作

### 1. dbsize

> 查看当前数据库的 key 的数量

```bash
dbsize

127.0.0.1:6379> dbsize
(integer) 0
```



### 2. keys

> 查看当前库所有 key

```bash
# 查看当前库所有 key
keys *

127.0.0.1:6379> keys *
(empty array)


# 查看指定库的所有 key
keys *数据库索引

127.0.0.1:6379> keys *1
(empty array)
```



### 3. exists

> 判断指定 key 是否存在

```bash
exists <key>

# 0 表示指定 key 不存在，1 表示指定 key 存在
127.0.0.1:6379> exists k1
(integer) 0
```



### 4. type

> 查看指定 key 的类型

```bash
type <key>

127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> type k1
string
```



### 5. del

> 删除指定 key

```bash
del <key>

127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> del k1
(integer) 1
```



### 6. unlink

> 非阻塞删除指定 key

```bash
# 仅将 key 从 keyspace 元数据中删除，真正的删除会在后续异步执行
unlink <key>

127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> unlink k1
(integer) 1
```



### 7. expire

> 给指定的 key 设置过期时间

```bash
expire <key> 秒数

127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> expire k1 5
(integer) 1
```



### 8. ttl

> 查看指定 key 还有多少秒过期

```bash
# -1 表示永不过期，-2 表示已过期
ttl key

127.0.0.1:6379> ttl k1
(integer) -2
```



### 9. flushdb

> 清空当前库

```bash
# 谨慎使用 ！！！
flushdb
```



### 10. flushall

> 通杀全部库

```bash
# 谨慎使用 ！！！！！！
flushall
```



















