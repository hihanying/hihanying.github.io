---
layout:     post                   
title:      JavaWeb17-Redis
subtitle:   1．NoSQL&Redis入门2．Redis优势3．Redis安装&启动4．Redis五大数据类型和基本操作命令5．Redis总体配置Redis.conf6．Redis持久化（RDB和AOF）7．Jedis
date:       2019-11-07
author:     HY
header-img:img/post-bg-2015.jpg
catalog:true
tags:
    - JavaWeb
    - Redis
    - Jedis
---

# Redis

## 1. Redis 简介

**用来解决大规模数据集合多重数据种类高并发带来的挑战**

Redis 是完全开源免费的，遵守BSD协议，是一个高性能的key-value数据库。

Redis 是一款高性能的NOSQL系列的非关系型数据库

### 什么是NOSQL

NoSQL(NoSQL = Not Only SQL)，意即“不仅仅是SQL”，是一项全新的数据库理念，泛指非关系型的数据库。

* 优势：性能非常高、可扩展性
* 作用：对关系型数据库的不足进行弥补。

### Redis 优势

* 性能极高 – Redis能读的**速度**是110000次/s,写的速度是81000次/s 。
* 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets **数据类型**操作。
* 原子 – Redis的所有操作都是**原子性**的，同时Redis还支持对几个操作全并后的原子性执行。
* 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等**特性**。

## 2. Redis 安装

**安装：**

1. 下载地址：https://github.com/dmajkic/redis/downloads。
2. 将64bit的内容拷贝到自定义盘符安装目录取名redis，如 C:\redis。
   * 可以将安装路径加入环境变量，方便使用
3. 双击`redis-server.exe`，即可打开redis 服务器端。
4. 双击`redis-cli.exe`，即可对 Redis 进行操作。

**配置：**

* 修改 `redis.windows.conf` 文件
* 使用 `CONFIG set` 命令来修改配置

## 3. Redis 数据类型

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

### String（字符串）

* string是redis最基本的类型，一个key对应一个value。
* string类型是二进制安全的。意思是redis的string可以包含任何数据，比如jpg图片或者序列化的对象 。
* string类型是Redis最基本的数据类型，一个键最大能存储512MB。

**相关命令**

| 命令                   | 描述                        |
| :--------------------- | :-------------------------- |
| SET key value          | 设置指定 key 的值           |
| GET key                | 获取指定 key 的值。         |
| GETRANGE key start end | 返回 key 中字符串值的子字符 |
| DEL key                | 删除指定 key                |

> 获取所有元素可以使用：`GETRANGE key 0 -1`

### Hash（哈希）

* Redis hash 是一个键值对集合。
* Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。
* 每个 hash 可以存储 $2^{32 - 1}$ 键值对（40多亿）。

**相关命令**

| 命令                     | 描述                                       |
| :----------------------- | :----------------------------------------- |
| HGET key field           | 获取存储在哈希表中指定 key 中字段的值      |
| HGET key                 | 获取存储在哈希表中的 key                   |
| HGETALL key              | 获取在哈希表中指定 key 的所有字段和值      |
| HSET key field value     | 将哈希表 key 中的字段 field 的值设为 value |
| HDEL key field1 [field2] | 删除一个或多个哈希表字段                   |

### List（列表）

* Redis 列表是简单的字符串列表，按照插入**顺序**排序。
* 可以添加一个元素到列表的头部（左边：）或者尾部（右边：）。
* 列表最多可存储  $2^{32 - 1}$ 元素 (4294967295, 每个列表可存储40多亿)。

**相关命令**

| 命令                      | 描述                         |
| :------------------------ | :--------------------------- |
| LPUSH key value1 [value2] | 将一个或多个值插入到列表头部 |
| RPUSH key value1 [value2] | 在列表尾部添加一个或多个值   |
| LPOP key                  | 移出并获取列表的第一个元素   |
| RPOP key                  | 移除并获取列表最后一个元素   |
| LRANGE key start stop     | 获取列表指定范围内的元素     |

### Set（集合）

* Redis的Set是string类型的**无序**集合。
* 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。
* 根据集合内元素的唯一性，第二次插入的元素将被忽略。
* 集合中最大的成员数为 $2^{32 - 1}$ (4294967295, 每个集合可存储40多亿个成员)。

**相关命令**

| 命令                       | 描述                     |
| :------------------------- | :----------------------- |
| SADD key member1 [member2] | 向集合添加一个或多个成员 |
| SMEMBERS key               | 返回集合中的所有成员     |
| SREM key member1 [member2] | 移除集合中一个或多个成员 |

### Sorted Set(有序集合)

* Redis Sorted Set （zset）是string类型元素的集合，且不允许重复的成员。不同的是每个元素都会关联一个double类型的分数。
* redis通过分数来为集合中的成员进行从小到大的排序。
* zset的成员是唯一的，但分数(score)却可以重复。

**相关命令**

| 命令                                     | 描述                                                   |
| :--------------------------------------- | :----------------------------------------------------- |
| ZADD key score1 member1 [score2 member2] | 向有序集合添加一个或多个成员，或者更新已存在成员的分数 |
| ZRANGE key start stop [WITHSCORES]       | 通过索引区间返回有序集合成指定区间内的成员             |
| ZREM key member [member ...]             | 移除有序集合中的一个或多个成员                         |

### 通用 keys 命令

| 命令         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| DEL key      | 该命令用于在 key 存在时删除 key。                            |
| TYPE key     | 返回 key 所储存的值的类型。                                  |
| KEYS pattern | 查找所有符合给定模式( pattern)的 key。例如：`KEYS *` 查询所有的 key |

[更多命令](https://www.w3cschool.cn/redis/redis-keys.html)

## 4. Redis 持久化

redis是一个内存数据库，当redis服务器重启，获取电脑重启，数据会丢失，我们可以将redis内存中的数据持久化保存到硬盘的文件中。

### Redis 持久化机制

1. RDB：默认方式，在一定的间隔时间中，检测key的变化情况，然后持久化数据
	1. 编辑redis.windwos.conf文件
		```python
		#   after 900 sec (15 min) if at least 1 key changed
		save 900 1
		#   after 300 sec (5 min) if at least 10 keys changed
		save 300 10
		#   after 60 sec if at least 10000 keys changed
		save 60 10000
		```
		
	2. 重新启动redis服务器，并指定配置文件名称
		
		```
		redis-server.exe redis.windows.conf	
		```
	
2. AOF：日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据
	1. 编辑redis.windwos.conf文件
		
		```python
		# yes开启aof；no关闭aof
		appendonly yes
		# appendfsync always ： 每一次操作都进行持久化
		appendfsync everysec ： 每隔一秒进行一次持久化
		# appendfsync no	 ： 不进行持久化
		```

### Redis 数据备份与恢复

**数据备份**

* Redis **SAVE** 命令用于创建当前数据库的备份。
* 该命令将在 redis 安装目录中创建dump.rdb文件。


**恢复数据**

如果需要恢复数据，只需将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可。获取 redis 目录可以使用 `CONFIG GET dir`命令。

## 5. Redis 在 Java 中使用

### 基本使用

* 首先需要下载驱动包 [**下载 jedis.jar**](https://mvnrepository.com/artifact/redis.clients/jedis)，确保下载最新驱动包。

* 在 classpath 中包含该 jar 包。

* 基本使用：

  ```java
  //1. 获取连接
  Jedis jedis = new Jedis("localhost",6379);
  //2. 操作
  jedis.set("username","zhangsan");
  //3. 关闭连接
  jedis.close();
  ```

### 连接池使用

Jedis连接池是基于apache-commons pool2实现的。使用前需要[**下载commons-pool2.jar**](https://mvnrepository.com/artifact/org.apache.commons/commons-pool2)。

```java
//0.创建一个配置对象
JedisPoolConfig config = new JedisPoolConfig();
config.setMaxTotal(50);
config.setMaxIdle(10);
//1.创建Jedis连接池对象
JedisPool jedisPool = new JedisPool(config,"localhost",6379);
//2.获取连接
Jedis jedis = jedisPool.getResource();
//3. 使用
jedis.set("name","java");
//4. 关闭 归还到连接池中
jedis.close();
jedisPool.close();
```

### 连接池工具类

```java
package util;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

/**
 * JedisPool工具类
 *  加载配置文件，配置连接池的参数
 *  提供获取连接的方法
 */
public class JedisPoolUtils {

    private static JedisPool jedisPool;

    static{
        // 1. 读取配置文件
        InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
        Properties pro = new Properties();
        try {
            pro.load(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
        // 2. 获取数据，设置到 JedisPoolConfig 中
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));
        // 3. 获取数据，初始化 JedisPool
        jedisPool = new JedisPool(config,pro.getProperty("host"),Integer.parseInt(pro.getProperty("port")));
    }

    /**
     * 获取 Jedis
     * @return Jedis
     */
    public static Jedis getJedis(){
        return jedisPool.getResource();
    }
}
```


jedis.properties配置文件内容：

```properties
host=127.0.0.1
port=6379
maxTotal=50
maxIdle=10
```

使用

```java
// 获取 jedis
Jedis jedis = JedisPoolUtils.getJedis();
```




