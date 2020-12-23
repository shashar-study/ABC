# NoSql

#### 概念

NoSql = not only sql

泛指非关系型数据库

#### 特点

解耦

1. 方便拓展（数据之间没有关系）
2. 大数据量高性能（Redis一秒写8万次，读取11万，NoSql的缓存记录级，是一种细粒度的缓存，性能高）
3. 数据类型多样，不需要实现设计数据库，可随去随用

#### 与传统RDBMS区别

传统的RDBMS

--结构化组织

--SQL

--数据和关系都存在单独的表中

--数据操作、数据定义语言

--严格的一致性

--基础的事务



NoSQL

--没有固定的查询语言

--简直对存储、列存储、文档存储、图形数据库  

--最终一致性

--CAP定理和BASE

--高性能、高可用、高可拓

# Redis

#### 作用

1. 内存存储、持久化，内存是断电即失没持久化很重要
2. 效率高，可以用于高速缓存
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计数器（浏览量）
6. ……

#### 特性

1. 多样的数据类型
2. 持久化
3. 集群
4. 事务

#### 安装和使用

##### Windows下：

下载对应版本的Redis.zip压缩包，解压即可，解压之后的目录如下：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201130100538877.png" alt="image-20201130100538877" style="zoom:50%;" />

redis-server.exe是启动服务，双击打开，即可使用Redis，开启之后的页面如下：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201130100808687.png" alt="image-20201130100808687" style="zoom:33%;" />

这里可以看见Redis版本号、默认端口号、PID等等信息

redis-cli.exe是客户端

打开之后，就连上了服务器，

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201130101318998.png" alt="image-20201130101318998" style="zoom:33%;" />

输入ping命令，可以查看是否连接成功、可以使用set key value 实行存储数据、使用get key获取数据，del key进行删除，执行过程和结果如下：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201130101824520.png" alt="image-20201130101824520" style="zoom:33%;" />

redis-check-aof可以测试持久化文件  是否正常

redis-benchmark.exe  测试性能



##### Linux下：

下载对应版本的tar.gz压缩文件，例如，redis-6.0.0.tar.gz

一般，安装程序会放在 /opt下，因此，可通过命令

```bash
将Redis文件移动到opt目录下
mv redis-6.0.0.tar.gz /opt
进入该目录
cd /opt
解压该文件
tar -zxvf redis-6.0.0.tar.gz
```

等解压完毕，即可

我的Redis是通过宝塔面板安装的，位置在www/server下

进入之后，目录结构如下：

![image-20201130142530289](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201130142530289.png)

其中，redis.conf是Redis的配置文件

因为Redis是使用C写的，需要安装c语言环境，我的服务器上有，输入命令

```bash
gcc -v
```

![image-20201130142947098](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201130142947098.png)

出现版本信息，如果没有安装，可以通过如下命令安装

```bash
yum install gcc -c++
然后通过make命令配置
make
make install
```

由于我是通过宝塔命令安装的Redis，所以Redis-server之类的文件在www/server/rdis/src之下

![image-20201130155254690](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201130155254690.png)

使用：

找到redis.conf文件，使用vim编辑器

将下图中的daemonize 的状态改为yes

![image-20201130161445887](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201130161445887.png)

退出保存,开启服务然后连接服务器

```bash
cd /usr/bin
redis-server /www/server/redis/redis.conf
redis-cli -p 6379
```

然后，就可以使用客户端了，使用方式同Windows

![image-20201130170252107](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201130170252107.png)



#### 客户端的使用

打开方式

```
Windows直接找到redis-cli.exe，双击打开即可
Linux输入redis-cli -p 6379,其中6379是默认的端口号
```

常用的命令(http://doc.redisfans.com/index.html)

```Bash
ping  #使用客户端向 Redis 服务器发送一个 PING ，如果服务器运作正常的话，会返回一个 PONG 
set key value #将（key,value）键值对存入Redis
get key #获取key对应的value
del key #删除（key，value），删除多个是del key1 key2 key3
keys * #匹配Redis中所有key
shutdown #关闭
```

#### 测试性能

redis-benchmark.exe是官方自带的压力测试工具

```bash
#测试：100个并发连接  100000请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

会对set\get\INCR \LPUSH\ RPUSH\RPOP\SADD\ HSET\SPOP……进行测试

![image-20201203104406137](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201203104406137.png)

#### 基础知识

==数据库==

Redis的数据库默认有16个，这个数字可以再Redis.conf文件中找到

![image-20201203105357064](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201203105357064.png)

默认使用第0个，可以使用select 数字 进行切换

```bash
127.0.0.1:6379> select 3
OK
127.0.0.1:6379[3]> DBSIZE
(integer) 0
127.0.0.1:6379[3]> select 0
OK
# 测试删除
127.0.0.1:6379[3]> DBSIZE
(integer) 3
127.0.0.1:6379[3]> keys *
1) "key"
2) "name"
3) "swit"
127.0.0.1:6379[3]> flushdb #清空当前数据库，flushall是清空所有数据库
OK
127.0.0.1:6379[3]> DBSIZE
(integer) 0

```

==Redis是单线程的==，CPU不是Redis的性能瓶颈，机器的内存和网络带宽才是

Redis是将所有的数据放在内存中，多线程会出现上下文的切换，耗时较长，对于内存系统来说，如果没有上下文切换，效率就是最高的



#### Redis五大数据类型

Redis是一个开源的内存数据结构存储系统，它可以用作==数据库==、==缓存==和==消息中间件==，它支持多种类型的数据结构

##### Redis-key

```bash
#清除当前数据库
flushdb
#清除全部数据库
flushall
#获取当前数据库所有key值
keys *
#存储（key,value）键值对
set key value
#获取key对应的value
get key
#判断key是否存在，存在返回1，不存在返回0
exists key
#移除（key,value)
move key 1#这里的1代表当前数据库
#设置过期时间为n秒，若没有key会返回0，有key会返回1 表示设置成功，到设置的时间这个（key,value)会自动删除
expire key n
#查看key的剩余存活时间,若不存在key会返回-2，若存在key但是key没有设置过期策略会返回-1
ttl key
#查看当前key对应的value的类型
type key
```

##### String

```bash
#将value拼接在key原本对应的值之后
append key value
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201204103506375.png" alt="image-20201204103506375" style="zoom:33%;" />

```bash
#获取value对应的长度
strlen key
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201204103645377.png" alt="image-20201204103645377" style="zoom:33%;" />

```bash
#实现i++操作,key值加1
incr key
incrby key increment
#对应的i--
decr key
decrby key decrement
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201204104120212.png" alt="image-20201204104120212" style="zoom:33%;" />![image-20201204104239368](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201204104239368.png)

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201204104120212.png" alt="image-20201204104120212" style="zoom:33%;" />![image-20201204104239368](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201204104239368.png)

```bash
#获取字符串某一范围内的值,截取字符串
getrange key start end
#范围修改字符串
setrange key offset value
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201204112342290.png" alt="image-20201204112342290" style="zoom:33%;" />

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201204112834569.png" alt="image-20201204112834569" style="zoom:33%;" />

​				

```bash
#如果key值 存在 ，则创建失败
setnx key value
#同时设置多个值
mset key1 value1 key2 value 2 key3 value3
#同时获取多个值
mget key1 key2 key3
#同时创建多个 值，若 已有key，则创建时失败，是一个原子性的操作，要么一起成功，要么全部失败
msetnx key1 value1 key2 value2
#getset的用法，先get在set，若无key,返回nil并创建，若有 key，返回value并覆盖value

```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201207155822046.png" alt="image-20201207155822046" style="zoom:33%;" /> <img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201207165844596.png" alt="image-20201207165844596" style="zoom:50%;" />

###### 存储和获取对象 

```bash
#创建一个 user对象,值为json，第一种
 mset user:1 {name:zhangsan,age:3}
 #第二种
 mset user:2:name lisi user:2:age 15
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201207164513675.png" alt="image-20201207164513675" style="zoom:33%;" />

#### List

在Redis里，通过不同的设置，可以利用list实现队列、堆栈、阻塞队列等 

所有的List命令都是以L开头

```bash
#创建链表key,并插入数据element到列表头部，左插入
LPUSH key element
#创建链表key,并插入数据element到列表尾部，右插入
RPUSH key element
#查看列表
LRANGE key start end

```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201209151617158.png" alt="image-20201209151617158" style="zoom:50%;" />

```bash
#移除左边第一个元素,返回被移除的元素
LPOP key
#移除右边第一个元素,返回被移除的元素
RPOP key
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201209152036017.png" alt="image-20201209152036017" style="zoom:50%;" />

```bash
#获取key第num为元素
lindex key num
#获取链表长度
llen key
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201209152355267.png" alt="image-20201209152355267" style="zoom:50%;" />

```bash
#移除key中num个值为value的数据，从左边开始移除，返回移除的元素个数，例如有一个value，num>=1 也会返回1，如果没有value，返回0
lrem key num value

#截取list,仅保留start end 之间的list元素
ltrim key start end

#将原链表的最后一个值移除并复制到新列表的开头
rpoplpush oldlist newlist

#判断是否存在某个链表
EXISTS key

#将某个链表index 位置的值改为value ，若是不存在这个链表或者下标超出返回都会报错
lset key index value

#在指定元素element之前或之后插入value
LINSERT key before/after element value

```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201209164811029.png" alt="image-20201209164811029" style="zoom:50%;" />

> #### 小结

1. list 是一个链表，可以实现左右插入删除和before、after插入
2. 如果key不存在，创建显得链表
3. 如果key存在，新增内容
4. 空链表也代表不存在
5. 两边插入更新 效率比中间高

#### Set

set里面的值不可以重复，set命令都以s开头,以下是一些基本操作

```bash
#向set中添加元素
sadd key value
sadd key value1 value2 value3
#获取set中所有元素
SMEMBERS key
#判断element是不是key中的元素，返回1 =存在，返回0不存在
sismember key element
#获取set大小
scard key
#删除元素
srem key element
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201210111619913.png" alt="image-20201210111619913" style="zoom:50%;" />

```bash
#获取随机数,可以指定个数，默认为1
srandmember key
srandmember key count
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201210111744310.png" alt="image-20201210111744310" style="zoom:50%;" />

```bash
#随机移除元素
spop key
spop key cout
#将oldset中元素element移除，并复制到newset中
smove oldset newset element
```



集合运算

```bash
#求A和B的差集，返回A中有而B中没有的元素
SDIFF A B
#求A和B的交集
SINTER A B
```



#### Hash(哈希)

可以认为存储形式是key--map<key,value>，所有命令以H开头

哈希表更适合对象的存储

```bash
#向哈希表key中插入<field,value>
hset key field value [field value ...]
#从哈希表key中获取field对应的value
hget key field
hget key field1 field2 field3
#获取key中所有点的<field,value>
hgetall key
#删除key中的<field,value>
hdel key field [field ...]
#计算哈希表的长度（有几个键值对）
hlen key
#判断key中是否有field
hexists key field

#获取key中所有的field
hkeys key
#获取key中所有的value
vals key
```



#### Zset（有序集合）

相比较set，增加了score，可以利用score进行排序

```bash
#插入值member，排序依据为score
zadd key [NX|XX] [CH] [INCR] score member [score member ...]

#从小到大排序，min和max是范围，[WITHSCORES]加上可返回scores,不加仅返回member，一般，可以把min、max换成-inf、+inf,代表正负无穷，这样可以在不知道key的范围进行排序
zrangebyscore key min max [WITHSCORES] [LIMIT offset count]
#从大到小
zrevrangebyscore key max min [WITHSCORES] [LIMIT offset count]
```



#### 三种特殊数据类型

##### 地理位置geospatial

作用：推算距离、地理位置信息，有六个命令

```bash
#添加地理位置（纬度、经度、名称）
geoadd key longitude latitude member [longitude latitude member ...]
#获取地理位置
geopos key member [member ...]
#计算两者之间的距离，不加默认单位m，[m|km|ft|mi]分别代表 米、千米、英尺、英里
geodist key member1 member2 [m|km|ft|mi]

#以给定的经纬度为中心，给定长度为半径，查找附近的人， [WITHDIST]显示距离 、[WITHCOORD]显示他人的经纬度、
georadius key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
#以给定的元素为中心，给定长度为半径，查找附近的人
georadiusbymember key member radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
#显示元素的哈希值
 geohash key member [member ...]
```

##### 基数Hyperloglog

基数指的是一个集合中不重复的元素，基数统计即统计一个数据集中不重复元素的个数，

优点，内存小，是固定的，有一定的错误率

所以，允许容错就用Hyperloglog，不允许就使用set

```bash
#添加元素
PFadd key element [element ...]
#计算不重复的元素
 PFcount key [key ...]
#将sourcekey（可以是多个的并集）中不重复的元素拿出来放到 destkey 中
PFmerge destkey sourcekey [sourcekey ...]
```

##### Bitmap

```bash
#添加元素
setbit key offset value
#获取元素
getbit key offset
#统计元素
 bitcount key [start end]
```



#### 事务

redis事务本质：一组命令的集合，一个事务中所有的指令都被序列化，在事务执行的过程中，按照顺序执行

性质：一次性、顺序性、排他性

==Redis事务没有隔离级别的概念！==

所有的命令在事务中，滨海没有直接被执行，只有发起执行任务时才执行

==这意味着，Redis单条命令有原子性，但是事务 不保证原子性==

##### 步骤：

1. 开启事务（multi）
2. 命令入队（……一系列命令，入队）
3. 执行事务（exec）/放弃事务（discard）

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201214103556920.png" alt="image-20201214103556920" style="zoom: 33%;" />![image-20201214103958680](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201214103958680.png)

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201214104047291.png" alt="image-20201214104047291" style="zoom:33%;" />

##### 异常：

在事务步骤2、3中出现异常的 情况

> 编译期异常，代码出错，事务中所有命令都不会被执行

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201214111455580.png" alt="image-20201214111455580" style="zoom:33%;" />

> 运行期异常，单条命令出现语法错误，该命令抛出异常，其他命令可以正常执行

命令没有报错，正常入队，但是语法错误，报错，对其他命令不影响

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201214111828071.png" alt="image-20201214111828071" style="zoom:33%;" />

##### 锁

乐观锁 和悲观锁

悲观锁：认为事务一定会出错，每一步都加锁

乐观锁：认为事务一定不出错，不加锁，获取version，更新的时候比较 version，看期间该数据是否被修改

 使用检测器模拟乐观锁

```bash
#例如，Redis里存有两个变量money、out,money代表账户余额，out代表花销
#正常逻辑是money减少num,out增加num

#第一种情况，事务正常运行
#获取money的version
watch money
#开启事务
multi
#花钱，money减少
decrby money 10
#out增加
incrby out 10
#执行事务 
exec
#解除监视
unwatch
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201215160750612.png" alt="image-20201215160750612" style="zoom:50%;" />

```bash
#第二种情况，在事务执行之前有别的线程修改了money的值
#一开始的操作如上
#获取money的version
watch money
#开启事务
multi
#花钱，money减少
decrby money 10
#out增加
incrby out 10
```

接下来在执行exec之前，重新打开一个终端，并在新的终端里修改money的值

```bash
incrby money 100
```

再次切换到原客户端，执行事务，就会发现事务执行失败了

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201215161151934.png" alt="image-20201215161151934" style="zoom:50%;" />

这时候，可以重新获取money版本，重新开启执行事务 即可

# Java使用Redis

## jedis

jedis是官方推荐的Java调用Redis的包

起一个maven项目，测试jedis 的使用

### 使用

1. 导包

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.3.0</version>
</dependency>

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.75</version>
</dependency>
```

2. 连接测试

```java
   //申明Jedis 对象，有很多种构造方法，最常见的就是host+port
   //有很多种方法，方法同之前的指令
   Jedis jedis = new Jedis("localhost");
    System.out.println(jedis.ping());
    Map<String, String> formula = jedis.hgetAll("20552_2020-09-30_formula");
    Map<String, String> data = jedis.hgetAll("20552_2020-09-30_data");
    jedis.close();//关闭连接
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201215164755903.png" alt="image-20201215164755903" style="zoom:50%;" />



## SpringBoot 整合

新建SpringBoot项目，勾选以下内容

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201216110622080.png" alt="image-20201216110622080" style="zoom:50%;" />

Springboot2.X以后连接Redis不再使用jedis，而是lettuce

两者区别 ：

jedis：直连Redis，多线程不安全，如果想要避免，需要使用jedis pool，BIO

lettuce：采用netty，实例可以在多个线程共享，NIO

### 步骤基本使用：

导包：

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
```



配置

```properties
#配置Redis

spring.redis.host=localhost
spring.redis.port=6379
```

测试：

```java
@SpringBootTest
class RedisSpringbootApplicationTests {

  @Autowired
  private RedisTemplate redisTemplate;

  @Test
  void contextLoads() {
   /* String key = "zhangsan";
    int value = 20;
    redisTemplate.opsForValue().set(key,value);*/
    System.out.println(redisTemplate.opsForValue().get("zhangsan"));
    System.out.println(redisTemplate.opsForValue().get("one"));
  }
}
```

在这里，我们使用的默认的RedisTemplate，在这个默认的模板中，有很多默认的函数如下：

![image-20201217100546348](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201217100546348.png)

opsForValue（）是用来操作字符串的，里面有很多方法，

![image-20201217101007888](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201217101007888.png)

可以看到，基本和之前学习的命令没有太大差别，其他的也是如此。

### 自定义RedisTemplate

**Springboot项目整合Redis**有两种默认的template，分别是：

1. RedisTemplate：使用的是默认的序列化器jdk序列化方式
2. StringRedisTemplate：使用了String序列化方式

由于分别使用了不同的序列化器，所以在Redis中存储的形式也不相同。举例如下：

```java
 @Test
  void useRedisTemplate() {
    //redisTemplate.opsForValue().set("User","张三");
    System.out.println(redisTemplate.opsForValue().get("User"));
  }

  @Test
  void useStringTemplate(){
    stringRedisTemplate.opsForValue().set("User","张三");
  }
```

在Redis客户端里的存储情况如下：

![image-20201217150856864](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201217150856864.png)

3）是StringStringRedisTemplate类型的，4）是RedisTemplate

如果每次使用Redis进行存储的时候还需要考虑怎么选择序列化方式未免太过麻烦，实际上在项目中我们一般会自定义一个RedisTemplate，这样就不会产生由于序列化方式而引起的不必要的麻烦。

自定义的RedisTemplate，其实主要做了两件事

1. new 序列化方式
2. 配置key 、value的序列化

可以看到，在RedisConfig中，首先分别new Jackson2JsonRedisSerializer(Object.class)和StringRedisSerializer()，再set key 和value的序列化

```java
import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.net.UnknownHostException;

/**
 * Author：shasha<br>
 * Time：2020/12/17 <br>
 * Description：redis配置类 <br>
 */
@Configuration
public class RedisConfig {

  /**
   * 自定义RedisTemplate,自定义序列化器
   * */
  @Bean(name = "myRedisTemplate")
  public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory)
      throws UnknownHostException {
    //为了开发方便，一般使用<String,Object>
    RedisTemplate<String, Object> template = new RedisTemplate<>();
    template.setConnectionFactory(redisConnectionFactory);

    //序列化配置
    // Json序列化配置
    Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
    ObjectMapper om = new ObjectMapper();
    om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
    om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
    jackson2JsonRedisSerializer.setObjectMapper(om);
    // String 的序列化
    StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

    // key采用String的序列化方式
    template.setKeySerializer(stringRedisSerializer);
    // hash的key也采用String的序列化方式
    template.setHashKeySerializer(stringRedisSerializer);
    // value序列化方式采用jackson
    template.setValueSerializer(jackson2JsonRedisSerializer);
    // hash的value序列化方式采用jackson
    template.setHashValueSerializer(jackson2JsonRedisSerializer);
    template.afterPropertiesSet();
    return template;
  }

}
```

### 自定义RedisUtils

根据项目具体实现的不同，编写自己的工具类,将一些常用操作重写，以便直接使用，而不是每次都需要使用opsFor……来使用，简化操作，但实际上 底层还是 不变的。

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;
import org.springframework.util.CollectionUtils;

import java.util.Collection;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;

/**
 * Author：shasha<br>
 * Time：2020/12/21 <br>
 * Description： <br>
 */
@Component
public final class RedisUtils {

  @Autowired
  @Qualifier("myRedisTemplate")
  private RedisTemplate<String, Object> redisTemplate;

  // =============================common============================
  /**
   * 指定缓存失效时间
   * @param key  键
   * @param time 时间(秒)
   */
  public boolean expire(String key, long time) {
    try {
      if (time > 0) {
        redisTemplate.expire(key, time, TimeUnit.SECONDS);
      }
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }

  /**
   * 根据key 获取过期时间
   * @param key 键 不能为null
   * @return 时间(秒) 返回0代表为永久有效
   */
  public long getExpire(String key) {
    return redisTemplate.getExpire(key, TimeUnit.SECONDS);
  }


  /**
   * 判断key是否存在
   * @param key 键
   * @return true 存在 false不存在
   */
  public boolean hasKey(String key) {
    try {
      return redisTemplate.hasKey(key);
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }


  /**
   * 删除缓存
   * @param key 可以传一个值 或多个
   */
  @SuppressWarnings("unchecked")
  public void del(String... key) {
    if (key != null && key.length > 0) {
      if (key.length == 1) {
        redisTemplate.delete(key[0]);
      } else {
        redisTemplate.delete((Collection<String>) CollectionUtils.arrayToList(key));
      }
    }
  }


  // ============================String=============================

  /**
   * 普通缓存获取
   * @param key 键
   * @return 值
   */
  public Object get(String key) {
    return key == null ? null : redisTemplate.opsForValue().get(key);
  }

  /**
   * 普通缓存放入
   * @param key   键
   * @param value 值
   * @return true成功 false失败
   */

  public boolean set(String key, Object value) {
    try {
      redisTemplate.opsForValue().set(key, value);
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }


  /**
   * 普通缓存放入并设置时间
   * @param key   键
   * @param value 值
   * @param time  时间(秒) time要大于0 如果time小于等于0 将设置无限期
   * @return true成功 false 失败
   */

  public boolean set(String key, Object value, long time) {
    try {
      if (time > 0) {
        redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
      } else {
        set(key, value);
      }
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }


  /**
   * 递增
   * @param key   键
   * @param delta 要增加几(大于0)
   */
  public long incr(String key, long delta) {
    if (delta < 0) {
      throw new RuntimeException("递增因子必须大于0");
    }
    return redisTemplate.opsForValue().increment(key, delta);
  }


  /**
   * 递减
   * @param key   键
   * @param delta 要减少几(小于0)
   */
  public long decr(String key, long delta) {
    if (delta < 0) {
      throw new RuntimeException("递减因子必须大于0");
    }
    return redisTemplate.opsForValue().increment(key, -delta);
  }


  // ================================Map=================================

  /**
   * HashGet
   * @param key  键 不能为null
   * @param item 项 不能为null
   */
  public Object hget(String key, String item) {
    return redisTemplate.opsForHash().get(key, item);
  }

  /**
   * 获取hashKey对应的所有键值
   * @param key 键
   * @return 对应的多个键值
   */
  public Map<Object, Object> hmget(String key) {
    return redisTemplate.opsForHash().entries(key);
  }

  /**
   * HashSet
   * @param key 键
   * @param map 对应多个键值
   */
  public boolean hmset(String key, Map<String, Object> map) {
    try {
      redisTemplate.opsForHash().putAll(key, map);
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }


  /**
   * HashSet 并设置时间
   * @param key  键
   * @param map  对应多个键值
   * @param time 时间(秒)
   * @return true成功 false失败
   */
  public boolean hmset(String key, Map<String, Object> map, long time) {
    try {
      redisTemplate.opsForHash().putAll(key, map);
      if (time > 0) {
        expire(key, time);
      }
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }


  /**
   * 向一张hash表中放入数据,如果不存在将创建
   *
   * @param key   键
   * @param item  项
   * @param value 值
   * @return true 成功 false失败
   */
  public boolean hset(String key, String item, Object value) {
    try {
      redisTemplate.opsForHash().put(key, item, value);
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }

  /**
   * 向一张hash表中放入数据,如果不存在将创建
   *
   * @param key   键
   * @param item  项
   * @param value 值
   * @param time  时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
   * @return true 成功 false失败
   */
  public boolean hset(String key, String item, Object value, long time) {
    try {
      redisTemplate.opsForHash().put(key, item, value);
      if (time > 0) {
        expire(key, time);
      }
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }


  /**
   * 删除hash表中的值
   *
   * @param key  键 不能为null
   * @param item 项 可以使多个 不能为null
   */
  public void hdel(String key, Object... item) {
    redisTemplate.opsForHash().delete(key, item);
  }


  /**
   * 判断hash表中是否有该项的值
   *
   * @param key  键 不能为null
   * @param item 项 不能为null
   * @return true 存在 false不存在
   */
  public boolean hHasKey(String key, String item) {
    return redisTemplate.opsForHash().hasKey(key, item);
  }


  /**
   * hash递增 如果不存在,就会创建一个 并把新增后的值返回
   *
   * @param key  键
   * @param item 项
   * @param by   要增加几(大于0)
   */
  public double hincr(String key, String item, double by) {
    return redisTemplate.opsForHash().increment(key, item, by);
  }


  /**
   * hash递减
   *
   * @param key  键
   * @param item 项
   * @param by   要减少记(小于0)
   */
  public double hdecr(String key, String item, double by) {
    return redisTemplate.opsForHash().increment(key, item, -by);
  }


  // ============================set=============================

  /**
   * 根据key获取Set中的所有值
   * @param key 键
   */
  public Set<Object> sGet(String key) {
    try {
      return redisTemplate.opsForSet().members(key);
    } catch (Exception e) {
      e.printStackTrace();
      return null;
    }
  }


  /**
   * 根据value从一个set中查询,是否存在
   *
   * @param key   键
   * @param value 值
   * @return true 存在 false不存在
   */
  public boolean sHasKey(String key, Object value) {
    try {
      return redisTemplate.opsForSet().isMember(key, value);
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }


  /**
   * 将数据放入set缓存
   *
   * @param key    键
   * @param values 值 可以是多个
   * @return 成功个数
   */
  public long sSet(String key, Object... values) {
    try {
      return redisTemplate.opsForSet().add(key, values);
    } catch (Exception e) {
      e.printStackTrace();
      return 0;
    }
  }


  /**
   * 将set数据放入缓存
   *
   * @param key    键
   * @param time   时间(秒)
   * @param values 值 可以是多个
   * @return 成功个数
   */
  public long sSetAndTime(String key, long time, Object... values) {
    try {
      Long count = redisTemplate.opsForSet().add(key, values);
      if (time > 0)
        expire(key, time);
      return count;
    } catch (Exception e) {
      e.printStackTrace();
      return 0;
    }
  }


  /**
   * 获取set缓存的长度
   *
   * @param key 键
   */
  public long sGetSetSize(String key) {
    try {
      return redisTemplate.opsForSet().size(key);
    } catch (Exception e) {
      e.printStackTrace();
      return 0;
    }
  }


  /**
   * 移除值为value的
   *
   * @param key    键
   * @param values 值 可以是多个
   * @return 移除的个数
   */

  public long setRemove(String key, Object... values) {
    try {
      Long count = redisTemplate.opsForSet().remove(key, values);
      return count;
    } catch (Exception e) {
      e.printStackTrace();
      return 0;
    }
  }

  // ===============================list=================================

  /**
   * 获取list缓存的内容
   *
   * @param key   键
   * @param start 开始
   * @param end   结束 0 到 -1代表所有值
   */
  public List<Object> lGet(String key, long start, long end) {
    try {
      return redisTemplate.opsForList().range(key, start, end);
    } catch (Exception e) {
      e.printStackTrace();
      return null;
    }
  }


  /**
   * 获取list缓存的长度
   *
   * @param key 键
   */
  public long lGetListSize(String key) {
    try {
      return redisTemplate.opsForList().size(key);
    } catch (Exception e) {
      e.printStackTrace();
      return 0;
    }
  }


  /**
   * 通过索引 获取list中的值
   *
   * @param key   键
   * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
   */
  public Object lGetIndex(String key, long index) {
    try {
      return redisTemplate.opsForList().index(key, index);
    } catch (Exception e) {
      e.printStackTrace();
      return null;
    }
  }


  /**
   * 将list放入缓存
   *
   * @param key   键
   * @param value 值
   */
  public boolean lSet(String key, Object value) {
    try {
      redisTemplate.opsForList().rightPush(key, value);
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }


  /**
   * 将list放入缓存
   * @param key   键
   * @param value 值
   * @param time  时间(秒)
   */
  public boolean lSet(String key, Object value, long time) {
    try {
      redisTemplate.opsForList().rightPush(key, value);
      if (time > 0)
        expire(key, time);
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }

  }


  /**
   * 将list放入缓存
   *
   * @param key   键
   * @param value 值
   * @return
   */
  public boolean lSet(String key, List<Object> value) {
    try {
      redisTemplate.opsForList().rightPushAll(key, value);
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }

  }


  /**
   * 将list放入缓存
   *
   * @param key   键
   * @param value 值
   * @param time  时间(秒)
   * @return
   */
  public boolean lSet(String key, List<Object> value, long time) {
    try {
      redisTemplate.opsForList().rightPushAll(key, value);
      if (time > 0)
        expire(key, time);
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }


  /**
   * 根据索引修改list中的某条数据
   *
   * @param key   键
   * @param index 索引
   * @param value 值
   * @return
   */

  public boolean lUpdateIndex(String key, long index, Object value) {
    try {
      redisTemplate.opsForList().set(key, index, value);
      return true;
    } catch (Exception e) {
      e.printStackTrace();
      return false;
    }
  }


  /**
   * 移除N个值为value
   *
   * @param key   键
   * @param count 移除多少个
   * @param value 值
   * @return 移除的个数
   */

  public long lRemove(String key, long count, Object value) {
    try {
      Long remove = redisTemplate.opsForList().remove(key, count, value);
      return remove;
    } catch (Exception e) {
      e.printStackTrace();
      return 0;
    }

  }

}
```



## redis.conf详解

打开阿里云上的Redis.conf，从头开始看

> 单位

介绍单位的换算，最后一行说明Redis对单位的大小写不敏感，都可以识别

![image-20201221160127649](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201221160127649.png)

> 包含

Redis可以整合其他的配置文件

![image-20201221160325066](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201221160325066.png)

> 加载一些so文件

![image-20201221160443501](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201221160443501.png)

> 网络

```bash
bind 127.0.0.1  #绑定的IP，只允许该IP连接
protected-mode yes #开启保护模式
port 6379  #端口号
```

> 通用

```bash
daemonize yes  #默认是no，yes表示以守护进程运行，即后台运行

pidfile /www/server/redis/redis.pid  #如果daemonize yes，需要指定pid

#日志（可选的类型）
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)  生产环境
# warning (only very important / critical messages are logged)
loglevel notice

logfile "/www/server/redis/redis.log"  #日志生成文件名

databases 16  #默认的数据库数量

always-show-logo yes #是否总是显示logo

```

> 快照SNAPSHOTTING

持久化，在规定的时间内，进行了；多少次操作，则会持久化到.rdb    .aof

Redis是内存数据库，断电即失

```bash
save 900 1  #900S 内，至少一个key修改，进行持久化
save 300 10 #300S 内，至少10个key修改，进行持久化
save 60 10000 #60S 内，至少10000个key修改，进行持久化，高并发状态下

stop-writes-on-bgsave-error yes #持久化出现错误后，是否要继续操作
rdbcompression yes  #是否压缩rdb文件，需要消耗CPU资源
rdbchecksum yes  #保存rdb文件时，是否进行校验
dir /www/server/redis/  #rdb文件保存目录

```

> 复制REPLICATION 

见主从复制

> 安全SECURITY

可以设置密码

```bash
#获取Redis 密码
config get requirepass
#设置密码
config set requirepass
#登录
auth 密码
```



> 限制

```bash
maxclients 10000  #最大客户端连接数
maxmemory <bytes> #Redis最大内存容量

#内存达到上限的处理策略
MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select among five behaviors:
#
# volatile-lru -> Evict using approximated LRU among the keys with an expire set.
# allkeys-lru -> Evict any key using approximated LRU.
# volatile-lfu -> Evict using approximated LFU among the keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU.
# volatile-random -> Remove a random key among the ones with an expire set.
# allkeys-random -> Remove a random key, any key.
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
# noeviction -> Don't evict anything, just return an error on write operations.
```

> APPEND ONLY MODE  aof配置

```bash
appendonly no  #默认不开启，一般rdb就足够了
appendfilename "appendonly.aof"

#同步机制
# appendfsync always  #每次更新都同步，耗费资源
appendfsync everysec  #默认每秒一次，可能丢失1s内数据
# appendfsync no  #不同步

```

## Redis持久化

Redis是内存数据库，断电即失，需要持久化功能

### RDB

RDB 是 Redis 默认的持久化方案。在指定的时间间隔内，执行指定次数的写操作，则会将内存中的数据写入到磁盘中。即在指定目录下生成一个dump.rdb文件。Redis 重启会通过加载dump.rdb文件恢复数据。

> 触发机制

1. 在指定的时间内，执行指定次数的写操作（触发快照SNAPSHOTTING配置的规则）
2. 执行save（阻塞， 只管保存快照，其他的等待） 或者是bgsave （异步）命令
3. 执行flushall 命令，清空数据库所有数据，意义不大。
4. 执行shutdown 命令，保证服务器正常关闭且不丢失任何数据，意义...也不大。



> 恢复数据

将dump.rdb 文件拷贝到redis的安装目录的bin目录下，重启redis服务即可。在实际开发中，一般会考虑到物理机硬盘损坏情况，选择备份dump.rdb 。

可以通过在Redis客户端中通过命令

```bash
config get dir
```

获取安装文件位置，将rdb文件放在安装目录的bin目录下即可

![image-20201222161937158](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201222161937158.png)

> 优缺点

优点：
1 适合大规模的数据恢复。
2 如果业务对数据完整性和一致性要求不高，RDB是很好的选择。

缺点：
1 数据的完整性和一致性不高，因为RDB可能在最后一次备份时宕机了。
2 备份时占用内存，因为Redis 在备份时会独立创建一个子进程，将数据写入到一个临时文件（此时内存中的数据是原来的两倍哦），最后再将临时文件替换之前的备份文件。
所以Redis 的持久化和数据的恢复要选择在夜深人静的时候执行是比较合理的。

### AOF

Redis 默认不开启。它的出现是为了弥补RDB的不足（数据的不一致性），所以它采用日志的形式来记录每个**写操作**，并**追加**到文件中。Redis 重启的会根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

> 使用步骤

打开redis.conf文件，修改配置

```bash
#找到 APPEND ONLY MODE 对应内容,默认关闭，开启需要手动把no改为yes
appendonly yes

#指定本地数据库文件名，默认值为 appendonly.aof
appendfilename "appendonly.aof"

#指定更新日志条件
# appendfsync always
appendfsync everysec
# appendfsync no

#配置重写触发机制
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
#解说：当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发。一般都设置为3G，64M太小了。
```

> 恢复数据

正常情况下，将appendonly.aof 文件拷贝到redis的安装目录的bin目录下，重启redis服务即可。但在实际开发中，可能因为某些原因导致appendonly.aof 文件格式异常，从而导致数据还原失败，数据还原失败的redis无法正常启动，可以在文件目录下    通过命令

```bash
redis-check-aof --fix appendonly.aof
```

对aof文件进行修复



> AOF的重写机制

前面也说到了，AOF的工作原理是将写操作追加到文件中，文件的冗余内容会越来越多。所以聪明的 Redis 新增了重写机制。当AOF文件的大小超过所设定的阈值时，Redis就会对AOF文件的内容压缩。

重写的原理：Redis 会fork出一条新进程，读取内存中的数据，并重新写到一个临时文件中。并没有读取旧文件，最后替换旧的aof文件。

触发机制：当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发。这里的“一倍”和“64M” 可以通过配置文件修改。

> AOF 的优缺点

优点：数据的完整性和一致性更高
       缺点：因为AOF记录的内容多，文件会越来越大，数据恢复也会越来越慢。

### 持久化总结

1. Redis 默认开启RDB持久化方式，在指定的时间间隔内，执行指定次数的写操作，则将内存中的数据写入到磁盘中。
2. RDB 持久化适合大规模的数据恢复但它的数据一致性和完整性较差。
3. Redis 需要手动开启AOF持久化方式，默认是每秒将写操作日志追加到AOF文件中。
4. AOF 的数据完整性比RDB高，但记录内容多了，会影响数据恢复的效率。
5. Redis 针对 AOF文件大的问题，提供重写的瘦身机制。
6. 若只打算用Redis 做缓存，可以关闭持久化。
7. 若打算使用Redis 的持久化。建议RDB和AOF都开启。其实RDB更适合做数据的备份，留一后手。AOF出问题了，还有RDB。



## redis发布订阅

Redis 发布订阅 (pub/sub) 是一种消息通信模式：发送者 (pub) 发送消息，订阅者 (sub) 接收消息。

Redis 客户端可以订阅任意数量的频道。

![image-20201223092244959](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201223092244959.png)

> 常用命令

```bash
#订阅一个或多个符合给定模式的频道。
PSUBSCRIBE pattern [pattern ...]

#查看订阅与发布系统状态。
PUBSUB subcommand [argument [argument ...]]

#将信息发送到指定的频道。
PUBLISH channel message

#退订所有给定模式的频道。
PUNSUBSCRIBE [pattern [pattern ...]]

#订阅给定的一个或多个频道的信息。
SUBSCRIBE channel [channel ...]

#指退订给定的频道。
UNSUBSCRIBE [channel [channel ...]]
```



## redis 主从复制

![img](http://file.elecfans.com/web1/M00/88/2A/o4YBAFyHXvKADq9RAAAMnMao0og549.png)

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master)，后者称为从节点(slave)，数据的复制是单向的，只能由主节点到从节点。

默认情况下，每台Redis服务器都是主节点；且一个主节点可以有多个从节点(或没有从节点)，但一个从节点只能有一个主节点。



> 主从复制的作用

1、数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。

2、故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。

3、负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。

4、读写分离：可以用于实现读写分离，主库写、从库读，读写分离不仅可以提高服务器的负载能力，同时可根据需求的变化，改变从库的数量。

5、高可用（集群）基石：除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。



> 主从复制启用

从节点开启主从复制，有3种方式：

1、配置文件：在从服务器的配置文件中加入 slaveof。

2、启动命令：redis-server启动命令后加入 --slaveof。

3、客户端命令：Redis服务器启动后，直接通过客户端执行命令 slaveof，则该Redis实例成为从节点。



> 步骤

==在没有经过配置前，每一台redis服务器都是主节点==

我们需要做的就是，给redis服务器配置一个主节点，让其成为子节点

比如第三种方法，在进入redis客户端之后，输入如下命令：

```bash
SLAVEOF 主节点地址 主节点端口号
#SLAVEOF 127.0.0.1 6379
```

这种方式是暂时的，实际开发中，应该去redis.conf中配置

> 复制REPLICATION  

将该配置语句取消注释，并填入主节点的host和port

```bash
replicaof <masterip> <masterport>
```

如果主节点有密码，还需要配置

```bash
 masterauth <master-password>
```

在客户端中，可以通过命令info replication查看当前客户端的的信息，包括，是否为主节点，如果是，有几个子节点，子节点的host和port等；如果不是，主节点的host和port，如下

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201223145729052.png" alt="image-20201223145729052" style="zoom:33%;" />





> 细节

1. 读写分离，主机写，从机只能读
2. 主机中所有数据，都会被从机保留
3. 没有额外配置哨兵时，主机断了，从机任然是从机，没有写操作，主机恢复后依然可以获取主机内容
4. 若是使用命令行配置成为从机，若是从机重启，会自动取消从机的身份





> 原理

<img src="https://images2018.cnblogs.com/blog/907596/201807/907596-20180710175627988-299575978.png" alt="img" style="zoom:50%;" />

**全量同步**
==Redis全量复制一般发生在Slave初始化阶段==，这时Slave需要将Master上的所有数据都复制一份。具体步骤如下： 
\- 从服务器连接主服务器，发送SYNC命令； 
\- 主服务器接收到SYNC命名后，开始执行BGSAVE命令生成RDB文件并使用缓冲区记录此后执行的所有写命令； 
\- 主服务器BGSAVE执行完后，向所有从服务器发送快照文件，并在发送期间继续记录被执行的写命令； 
\- 从服务器收到快照文件后丢弃所有旧数据，载入收到的快照； 
\- 主服务器快照发送完毕后开始向从服务器发送缓冲区中的写命令； 
\- 从服务器完成对快照的载入，开始接收命令请求，并执行来自主服务器缓冲区的写命令；

<img src="https://images2018.cnblogs.com/blog/907596/201807/907596-20180710175736785-1172070242.png" alt="img" style="zoom:50%;" />

完成上面几个步骤后就完成了从服务器数据初始化的所有操作，从服务器此时可以接收来自用户的读请求。

**增量同步**
Redis增量复制是指Slave初始化后开始正常工作时主服务器发生的写操作同步到从服务器的过程。 
增量复制的过程主要是主服务器每执行一个写命令就会向从服务器发送相同的写命令，从服务器接收并执行收到的写命令。

**Redis主从同步策略**
主从刚刚连接的时候，进行全量同步；全同步结束后，进行增量同步。当然，如果有需要，slave 在任何时候都可以发起全量同步。redis 策略是，无论如何，首先会尝试进行增量同步，如不成功，要求从机进行全量同步。

**注意点**
如果多个Slave断线了，需要重启的时候，因为只要Slave启动，就会发送sync请求和主机全量同步，当多个同时出现的时候，可能会导致Master IO剧增宕机。

如果主节点宕机，子节点可以通过命令==SLAVEOF no one==让自己变成主节点，其他节点可以通过==手动==配置成为该节点的子节点，原有的主节点恢复正常之后成了光杆司令。

## 哨兵模式（Sentinel）

主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑哨兵模式。

哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，哨兵是一个独立的进程，作为进程，它会独立运行。其原理是**哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。**

<img src="https://upload-images.jianshu.io/upload_images/11320039-57a77ca2757d0924.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp" alt="img" style="zoom:50%;" />