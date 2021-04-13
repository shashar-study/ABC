## 微服务架构

> 微服务需要解决的问题

1. 多个服务 应该如何访问？
2. 服务间如何通信？
3. 服务如何治理？
4. 服务挂了如何解决？

> 解决方案

SpringCloud  一套生态

1. SpringCloud Netflix  一站式解决方案

| API网关        | zuul组件                          |
| -------------- | --------------------------------- |
| 通信方式       | Feign  基于HTTP的通信，同步、阻塞 |
| 服务注册与发现 | Eureka                            |
| 熔断机制       | Hystrix                           |

2. Apache Dubbo Zookeeper  半自动，需要整合其他的组件

| API            | 无              |
| -------------- | --------------- |
| 通信方式       | Dubbo           |
| 服务注册与发现 | Zookeeper       |
| 熔断机制       | 无，借助Hystrix |

3. SpringCloud Alibaba

| API            | Dubbo Proxy   |
| -------------- | ------------- |
| 通信方式       | **Dubbo RPC** |
| 服务注册与发现 | Alibaba Nacos |
| 熔断机制       | Sentinel      |

新概念：服务网格~ Server Mesh istio  

万变不离其宗4个问题： 1. API网关 2. HTTP,RPC通信 3. 注册和发现 4. 熔断机制

## 微服务概念

通常而言，微服务架构是一种架构模式，或者说是一种架构风格，它体长将单一的应用程序划分成一组小的服务，每个服务运行在其独立的自己的进程内，服务之间互相协调，互相配置，为用户提供最终价值，服务之间采用轻量级的通信机制(HTTP)互相沟通，每个服务都围绕着具体的业务进行构建，并且能狗被独立的部署到生产环境中，另外，应尽量避免统一的，集中式的服务管理机制，对具体的一个服务而言，应该根据业务上下文，选择合适的语言，工具(Maven)对其进行构建，可以有一个非常轻量级的集中式管理来协调这些服务，可以使用不同的语言来编写服务，也可以使用不同的数据存储。
再来从技术维度角度理解下：

微服务化的核心就是将传统的一站式应用，根据业务拆分成一个一个的服务，彻底地去耦合，每一个微服务提供单个业务功能的服务，一个服务做一件事情，从技术角度看就是一种小而独立的处理过程，类似进程的概念，能够自行单独启动或销毁，拥有自己独立的数据库。

>    微服务与微服务架构

微服务

强调的是服务的大小，它关注的是某一个点，是具体解决某一个问题/提供落地对应服务的一个服务应用，狭义的看，可以看作是IDEA中的一个个微服务工程，或者Moudel。

IDEA 工具里面使用Maven开发的一个个独立的小Moudel，它具体是使用SpringBoot开发的一个小模块，专业的事情交给专业的模块来做，一个模块就做着一件事情。
强调的是一个个的个体，每个个体完成一个具体的任务或者功能。

微服务架构

一种新的架构形式，Martin Fowler 于2014年提出。

微服务架构是一种架构模式，它体长将单一应用程序划分成一组小的服务，服务之间相互协调，互相配合，为用户提供最终价值。每个服务运行在其独立的进程中，服务与服务之间采用轻量级的通信机制(如HTTP)互相协作，每个服务都围绕着具体的业务进行构建，并且能够被独立的部署到生产环境中，另外，应尽量避免统一的，集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言、工具(如Maven)对其进行构建。

> 微服务优缺点
>

优点

单一职责原则；
每个服务足够内聚，足够小，代码容易理解，这样能聚焦一个指定的业务功能或业务需求；
开发简单，开发效率高，一个服务可能就是专一的只干一件事；
微服务能够被小团队单独开发，这个团队只需2-5个开发人员组成；
微服务是松耦合的，是有功能意义的服务，无论是在开发阶段或部署阶段都是独立的；
微服务能使用不同的语言开发；
易于和第三方集成，微服务允许容易且灵活的方式集成自动部署，通过持续集成工具，如jenkins，Hudson，bamboo；
微服务易于被一个开发人员理解，修改和维护，这样小团队能够更关注自己的工作成果，无需通过合作才能体现价值；
微服务允许利用和融合最新技术；
微服务只是业务逻辑的代码，不会和HTML，CSS，或其他的界面混合;
每个微服务都有自己的存储能力，可以有自己的数据库，也可以有统一的数据库；
缺点

开发人员要处理分布式系统的复杂性；
多服务运维难度，随着服务的增加，运维的压力也在增大；
系统部署依赖问题；
服务间通信成本问题；
数据一致性问题；
系统集成测试问题；
性能和监控问题；

> 微服务技术栈有那些？

微服务技术条目 落地技术
服务开发 SpringBoot、Spring、SpringMVC等
服务配置与管理 Netfix公司的Archaius、阿里的Diamond等
服务注册与发现 Eureka、Consul、Zookeeper等
服务调用 Rest、PRC、gRPC
服务熔断器 Hystrix、Envoy等
负载均衡 Ribbon、Nginx等
服务接口调用(客户端调用服务的简化工具) Fegin等
消息队列 Kafka、RabbitMQ、ActiveMQ等
服务配置中心管理 SpringCloudConfig、Chef等
服务路由(API网关) Zuul等
服务监控 Zabbix、Nagios、Metrics、Specatator等
全链路追踪 Zipkin、Brave、Dapper等
数据流操作开发包 SpringCloud Stream(封装与Redis，Rabbit，Kafka等发送接收消息)
时间消息总栈 SpringCloud Bus
服务部署 Docker、OpenStack、Kubernetes等

为什么选择SpringCloud作为微服务架构

选型依据
整体解决方案和框架成熟度
社区热度
可维护性
学习曲线
当前各大IT公司用的微服务架构有那些？
阿里：dubbo+HFS
京东：JFS
新浪：Motan
当当网：DubboX
…

> 各微服务框架对比

功能点/服务框架 Netflix/SpringCloud Motan gRPC Thrift Dubbo/DubboX
功能定位 完整的微服务框架 RPC框架，但整合了ZK或Consul，实现集群环境的基本服务注册发现 RPC框架 RPC框架 服务框架
支持Rest 是，Ribbon支持多种可拔插的序列号选择 否 否 否 否
支持RPC 否 是(Hession2) 是 是 是
支持多语言 是(Rest形式) 否 是 是 否
负载均衡 是(服务端zuul+客户端Ribbon)，zuul-服务，动态路由，云端负载均衡Eureka（针对中间层服务器） 是(客户端) 否 否 是(客户端)
配置服务 Netfix Archaius，Spring Cloud Config Server 集中配置 是(Zookeeper提供) 否 否 否
服务调用链监控 是(zuul)，zuul提供边缘服务，API网关 否 否 否 否
高可用/容错 是(服务端Hystrix+客户端Ribbon) 是(客户端) 否 否 是(客户端)
典型应用案例 Netflix Sina Google Facebook
社区活跃程度 高 一般 高 一般 2017年后重新开始维护，之前中断了5年
学习难度 中等 低 高 高 低
文档丰富程度 高 一般 一般 一般 高
其他 Spring Cloud Bus为我们的应用程序带来了更多管理端点 支持降级 Netflix内部在开发集成gRPC IDL定义 实践的公司比较多

## Spring Cloud

> 学习文档
>

SpringCloud Netflix 中文文档：https://springcloud.cc/spring-cloud-netflix.html
       SpringCloud 中文API文档(官方文档翻译版)：https://springcloud.cc/spring-cloud-dalston.html
       SpringCloud中国社区：http://springcloud.cn/
       SpringCloud中文网：https://springcloud.cc

> 实际开发对应版本
>

实际开发中使用的spring boot和spring cloud版本

2.0.6.RELEASE 2018-10 Fomchiey-SR2 2018-10
        2.1.4.RELEASE 2019-04 Greenwich.SR1 2019-03



> Dubbo和SpringCloud

目前成熟的互联网架构，应用服务化拆分 + 消息中间件

对比结果：

|              | Dubbo         | SpringCloud                  |
| ------------ | ------------- | ---------------------------- |
| 服务注册中心 | Zookeeper     | Spring Cloud Netfilx Eureka  |
| 服务调用方式 | RPC           | REST API                     |
| 服务监控     | Dubbo-monitor | Spring Boot Admin            |
| 断路器       | 不完善        | Spring Cloud Netfilx Hystrix |
| 服务网关     | 无            | Spring Cloud Netfilx Zuul    |
| 分布式配置   | 无            | Spring Cloud Config          |
| 服务跟踪     | 无            | Spring Cloud Sleuth          |
| 消息总栈     | 无            | Spring Cloud Bus             |
| 数据流       | 无            | Spring Cloud Stream          |
| 批量任务     | 无            | Spring Cloud Task            |


最大区别：Spring Cloud 抛弃了Dubbo的RPC通信，采用的是基于HTTP的REST方式

严格来说，这两种方式各有优劣。虽然从一定程度上来说，后者牺牲了服务调用的性能，但也避免了上面提到的原生RPC带来的问题。而且REST相比RPC更为灵活，服务提供方和调用方的依赖只依靠一纸契约，不存在代码级别的强依赖，这个优点在当下强调快速演化的微服务环境下，显得更加合适。

品牌机和组装机的区别

社区支持与更新力度的区别

**总结：**二者解决的问题域不一样：Dubbo的定位是一款RPC框架，而SpringCloud的目标是微服务架构下的一站式解决方案。

> 学习流程
>

我们会使用一个Dept部门模块做一个微服务通用案例Consumer消费者(Client)通过REST调用Provider提供者(Server)提供的服务。

回顾Spring，SpringMVC，Mybatis等以往学习的知识。

Maven的分包分模块架构复习。

一个简单的Maven模块结构是这样的：

```
一个简单的Maven模块结构是这样的：

-- app-parent: 一个父项目(app-parent)聚合了很多子项目(app-util\app-dao\app-web...)
  |-- pom.xml
  |
  |-- app-core
  ||---- pom.xml
  |
  |-- app-web
  ||---- pom.xml
  ......

```



一个父工程带着多个Moudule子模块

MicroServiceCloud父工程(Project)下初次带着3个子模块(Module)

microservicecloud-api 【封装的整体entity/接口/公共配置等】
        microservicecloud-consumer-dept-80 【服务提供者】
        microservicecloud-provider-dept-8001 【服务消费者】

### 父工程

新建Maven项目，作为父项目，过程如下：

1. 

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210326105624831.png" alt="image-20210326105624831" style="zoom:50%;" />

由于是父项目，没有编码，可以把src删除

2. pom 文件配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.sha</groupId>
    <artifactId>KSpringCloud</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!--打包方式-->
    <packaging>pom</packaging>

    <!--版本号-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.16.18</lombok.version>
    </properties>
    <dependencyManagement>
        <dependencies>
            <!--SpringCloud-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Greenwich.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <!--SpringBoot-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.1.4.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <!--数据库连接-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.19</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>1.1.10</version>
            </dependency>
            <!--SpringBoot 启动器-->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>1.3.2</version>
            </dependency>

            <!--日志测试~-->
            <dependency>
                <groupId>ch.qos.logback</groupId>
                <artifactId>logback-core</artifactId>
                <version>1.2.3</version>
            </dependency>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
            </dependency>
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

### 第一个模块KSpringCloud-API

新建maven项目KSpringCloud-API，作为父工程的子module

项目总体包目录结构

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210401104803309.png" alt="image-20210401104803309" style="zoom:50%;" />

#### 数据库和表

新建数据库db01

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210331110809416.png" alt="image-20210331110809416" style="zoom:33%;" />

数据库表和预先插入的样例数据如下：

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : 鏈湴鏁版嵁搴�
 Source Server Type    : MySQL
 Source Server Version : 80019
 Source Host           : localhost:3306
 Source Schema         : db01

 Target Server Type    : MySQL
 Target Server Version : 80019
 File Encoding         : 65001

 Date: 01/04/2021 09:24:27
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for dept
-- ----------------------------
DROP TABLE IF EXISTS `dept`;
CREATE TABLE `dept`  (
  `dno` bigint(0) NOT NULL AUTO_INCREMENT,
  `dname` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `dsource` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`dno`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '閮ㄩ棬琛� ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of dept
-- ----------------------------
INSERT INTO `dept` VALUES (1, '开发部', 'db01');
INSERT INTO `dept` VALUES (2, '市场部', 'db01');
INSERT INTO `dept` VALUES (3, '运营部', 'db01');
INSERT INTO `dept` VALUES (4, '人事部', 'db01');
INSERT INTO `dept` VALUES (5, '财务部', 'db01');

SET FOREIGN_KEY_CHECKS = 1;
```

#### 导入依赖

```
 <dependencies>
     <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
     </dependency>
</dependencies>
```

#### 新建实体类Dept.java

```java
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.experimental.Accessors;
import java.io.Serializable;

@Data
@NoArgsConstructor
@Accessors(chain = true)
public class Dept implements Serializable {

  private Long dno;
  private String dname;
  private  String dsource;

  public Dept(String dname){
    this.dname=dname;
  }
}
```

### 第二个模块KSpringCloud-Provider

项目总体包结构

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210401104632569.png" alt="image-20210401104632569" style="zoom:50%;" />

#### 导入依赖

新建空的Maven项目，导入依赖,重点是第一条依赖，根据自己的包名进行修改

```
  <dependencies>
        <!--需要拿到api里面的实体类-->
        <dependency>
            <groupId>com.sha</groupId>
            <artifactId>KSpringCloud-API</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>

        <!--mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </dependency>

        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>

        <!--test-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-test</artifactId>
        </dependency>
        <!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--jetty 与tomcat类似-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
        </dependency>

        <!--热部署工具-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>
```



#### Dao层接口

```java
import com.sha.pojo.Dept;//对应自己设置的包名
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import java.util.List;

/**
 * Author：shasha<br>
 * Time：2021/3/31 <br>
 * Description： <br>
 */
@Mapper
@Repository
public interface DeptDao {

  public boolean addDept(Dept dept);

  public Dept queryById(Long id);

  public List<Dept> queryAll();
}
```

#### service层

```java
import java.util.List;

/**
 * Author：shasha<br>
 * Time：2021/3/31 <br>
 * Description： <br>
 */
public interface Dservice {

  public boolean addDept(Dept dept);

  public Dept queryById(Long id);

  public List<Dept> queryAll();
}
```

```java
import com.sha.dao.DeptDao;
import com.sha.pojo.Dept;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * Author：shasha<br>
 * Time：2021/3/31 <br>
 * Description： <br>
 */
@Service
public class DserviceImpl implements Dservice {

  @Autowired
  private DeptDao deptDao;

  @Override
  public boolean addDept(Dept dept) {
    return deptDao.addDept(dept);
  }

  @Override
  public Dept queryById(Long id) {
    return deptDao.queryById(id);
  }

  @Override
  public List<Dept> queryAll() {
    return deptDao.queryAll();
  }
}
```

#### Controller层

```java
import com.sha.pojo.Dept;
import com.sha.service.Dservice;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

/**
 * Author：shasha<br>
 * Time：2021/3/31 <br>
 * Description： <br>
 */
@RestController
public class Dcontroller {

  @Autowired
   private Dservice dservice;

  @PostMapping("/dept/add")
  public boolean addDept(Dept dept){
    return dservice.addDept(dept);
  }

  @GetMapping("/dept/get/{id}")
  public Dept getDept(@PathVariable("id") Long id){
    return dservice.queryById(id);
  }

  @GetMapping("/dept/list")
  public List<Dept> getAllDept(){
    return dservice.queryAll();
  }  
}
```



#### 配置文件

接下来配置数据库连接和mybatis

在resources下面新建application.yml，配置如下：(用户名和密码修改成自己对应的)

```yml
server:
  port: 8001

#mybatis
mybatis:
  type-aliases-package: com.sha.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml
  config-location: classpath:mybatis/mybatis-config.xml

#spring
spring:
  application:
    name: dept-provider
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db01?useUnicode=true&characterEncording=utf-8
    username: root
    password: 123456
```

按照下图创建文件mybatis-config.xml和DeptMapper.xml

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210401093950294.png" alt="image-20210401093950294" style="zoom:50%;" />



mybatis-config.xml具体 如下：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<settings>
    <!--开启二级缓存-->
    <setting name="cacheEnabled" value="true"/>
</settings>
</configuration>
```

DeptMapper.xml具体如下：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.sha.dao.DeptDao"> <!--写对应的包名-->
    <insert id="addDept" parameterType="Dept">
        insert into dept(dname, dsource)
        values(#{dname},DATABASE())
    </insert>

    <select id="queryById" parameterType="Long" resultType="Dept">
        select *
        from dept where dno=#{id}
    </select>
    <select id="queryAll" resultType="Dept" >
        select *
        from dept;
    </select>
</mapper>
```

#### 启动测试

编写启动类

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Provider_8001 {
  public static void main(String[] args) {
    SpringApplication.run(Provider_8001.class,args);
  }
}

```



结果如下：

![image-20210401094431368](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210401094431368.png)

![image-20210401094416749](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210401094416749.png)

### 第三个模块KSpringCloud-Customer

项目总体包结构

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210401104714431.png" alt="image-20210401104714431" style="zoom:50%;" />

#### 导入依赖

新建空的maven项目，导入依赖,同样的，实体类要根据自己的包名导入

```
    <dependencies>
        <!--实体类+web+热部署-->
        <dependency>
            <groupId>com.sha</groupId>
            <artifactId>KSpringCloud-API</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>
```



#### 注入RestTemplate

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ConfigBean {

   @Bean
  public RestTemplate getRestTemplate(){
     return new RestTemplate();
   }
}
```

#### Controller层

```java
import com.sha.pojo.Dept;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
public class DController {
  /*消费者没有service层
  * 通过restful方式调用接口
  * */

  @Autowired
  private RestTemplate restTemplate;

  private static final String REST_URL_PREFIX= "http://localhost:8001";

  @RequestMapping("/cus/dept/add")
  public boolean add(Dept dept){
    return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add",dept,Boolean.class);
  }

  @RequestMapping("/cus/dept/get/{id}")
  public Dept get(@PathVariable("id") Long id){
    return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id,Dept.class);
  }

  @RequestMapping("/cus/dept/list")
  public List<Dept> list(){
    return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class);
  }
}
```

#### 配置端口号application.yml

```yml
server:
  port: 80
```

#### 启动测试

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Customer_80 {
  public static void main(String[] args) {
    SpringApplication.run(Customer_80.class,args);
  }
}
```



## Eureka服务注册中心

### 概念

> 什么是Eureka

Netflix在涉及Eureka时，遵循的就是API原则.
Eureka是Netflix的有个子模块，也是核心模块之一。Eureka是基于REST的服务，用于定位服务，以实现云端中间件层服务发现和故障转移，服务注册与发现对于微服务来说是非常重要的，有了服务注册与发现，只需要使用服务的标识符，就可以访问到服务，而不需要修改服务调用的配置文件了，功能类似于Dubbo的注册中心，比如Zookeeper.

> 原理理解

Eureka基本的架构

Springcloud 封装了Netflix公司开发的Eureka模块来实现服务注册与发现 (对比Zookeeper).

Eureka采用了C-S的架构设计，EurekaServer作为服务注册功能的服务器，他是服务注册中心.

而系统中的其他微服务，使用Eureka的客户端连接到EurekaServer并维持心跳连接。这样系统的维护人员就可以通过EurekaServer来监控系统中各个微服务是否正常运行，Springcloud 的一些其他模块 (比如Zuul) 就可以通过EurekaServer来发现系统中的其他微服务，并执行相关的逻辑.

![img](https://img-blog.csdnimg.cn/20200521130157770.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70#pic_center)

和Dubbo架构对比.

<img src="https://img-blog.csdnimg.cn/20201120091517323.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70#pic_center" alt="img" style="zoom:50%;" />



Eureka 包含两个组件：Eureka Server 和 Eureka Client.

Eureka Server 提供服务注册，各个节点启动后，回在EurekaServer中进行注册，这样Eureka Server中的服务注册表中将会储存所有课用服务节点的信息，服务节点的信息可以在界面中直观的看到.

Eureka Client 是一个Java客户端，用于简化EurekaServer的交互，客户端同时也具备一个内置的，使用轮询负载算法的负载均衡器。在应用启动后，将会向EurekaServer发送心跳 (默认周期为30秒) 。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中把这个服务节点移除掉 (默认周期为90s).

三大角色

Eureka Server：提供服务的注册与发现
Service Provider：服务生产方，将自身服务注册到Eureka中，从而使服务消费方能狗找到
Service Consumer：服务消费方，从Eureka中获取注册服务列表，从而找到消费服务

> 自我保护机制 

一句话总结就是：**某时刻某一个微服务不可用，eureka不会立即清理，依旧会对该微服务的信息进行保存！**

- 默认情况下，当eureka server在一定时间内没有收到实例的心跳，便会把该实例从注册表中删除（**默认是90秒**），但是，如果短时间内丢失大量的实例心跳，便会触发eureka server的自我保护机制，比如在开发测试时，需要频繁地重启微服务实例，但是我们很少会把eureka server一起重启（因为在开发过程中不会修改eureka注册中心），**当一分钟内收到的心跳数大量减少时，会触发该保护机制**。可以在eureka管理界面看到Renews threshold和Renews(last min)，当后者（最后一分钟收到的心跳数）小于前者（心跳阈值）的时候，触发保护机制，会出现红色的警告：`EMERGENCY!EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT.RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEGING EXPIRED JUST TO BE SAFE.`从警告中可以看到，eureka认为虽然收不到实例的心跳，但它认为实例还是健康的，eureka会保护这些实例，不会把它们从注册表中删掉。
- 该保护机制的目的是避免网络连接故障，在发生网络故障时，微服务和注册中心之间无法正常通信，但服务本身是健康的，不应该注销该服务，如果eureka因网络故障而把微服务误删了，那即使网络恢复了，该微服务也不会重新注册到eureka server了，因为只有在微服务启动的时候才会发起注册请求，后面只会发送心跳和服务列表请求，这样的话，该实例虽然是运行着，但永远不会被其它服务所感知。所以，eureka server在短时间内丢失过多的客户端心跳时，会进入自我保护模式，该模式下，eureka会保护注册表中的信息，不在注销任何微服务，当网络故障恢复后，eureka会自动退出保护模式。自我保护模式可以让集群更加健壮。
- 但是我们在开发测试阶段，需要频繁地重启发布，如果触发了保护机制，则旧的服务实例没有被删除，这时请求有可能跑到旧的实例中，而该实例已经关闭了，这就导致请求错误，影响开发测试。所以，在开发测试阶段，我们可以把自我保护模式关闭，只需在eureka server配置文件中加上如下配置即可：`eureka.server.enable-self-preservation=false`【不推荐关闭自我保护机制】

### Eureka和Zookeeper对比

1. 回顾CAP原则
RDBMS (MySQL\Oracle\sqlServer) ===> ACID

NoSQL (Redis\MongoDB) ===> CAP

2. ACID是什么？
A (Atomicity) 原子性
C (Consistency) 一致性
I (Isolation) 隔离性
D (Durability) 持久性
3. CAP是什么?
C (Consistency) 强一致性
A (Availability) 可用性
P (Partition tolerance) 分区容错性
CAP的三进二：CA、AP、CP

4. CAP理论的核心
一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求
根据CAP原理，将NoSQL数据库分成了满足CA原则，满足CP原则和满足AP原则三大类
CA：单点集群，满足一致性，可用性的系统，通常可扩展性较差
CP：满足一致性，分区容错的系统，通常性能不是特别高
AP：满足可用性，分区容错的系统，通常可能对一致性要求低一些
5. 作为分布式服务注册中心，Eureka比Zookeeper好在哪里？
著名的CAP理论指出，一个分布式系统不可能同时满足C (一致性) 、A (可用性) 、P (容错性)，由于分区容错性P再分布式系统中是必须要保证的，因此我们只能再A和C之间进行权衡。

Zookeeper 保证的是 CP —> 满足一致性，分区容错的系统，通常性能不是特别高
Eureka 保证的是 AP —> 满足可用性，分区容错的系统，通常可能对一致性要求低一些
Zookeeper保证的是CP

 当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的注册信息，但不能接收服务直接down掉不可用。也就是说，服务注册功能对可用性的要求要高于一致性。但zookeeper会出现这样一种情况，当master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举。问题在于，选举leader的时间太长，30-120s，且选举期间整个zookeeper集群是不可用的，这就导致在选举期间注册服务瘫痪。在云部署的环境下，因为网络问题使得zookeeper集群失去master节点是较大概率发生的事件，虽然服务最终能够恢复，但是，漫长的选举时间导致注册长期不可用，是不可容忍的。

Eureka保证的是AP

 Eureka看明白了这一点，因此在设计时就优先保证可用性。Eureka各个节点都是平等的，几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。而Eureka的客户端在向某个Eureka注册时，如果发现连接失败，则会自动切换至其他节点，只要有一台Eureka还在，就能保住注册服务的可用性，只不过查到的信息可能不是最新的，除此之外，Eureka还有之中自我保护机制，如果在15分钟内超过85%的节点都没有正常的心跳，那么Eureka就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况：

Eureka不在从注册列表中移除因为长时间没收到心跳而应该过期的服务
Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其他节点上 (即保证当前节点依然可用)
当网络稳定时，当前实例新的注册信息会被同步到其他节点中
因此，Eureka可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像zookeeper那样使整个注册服务瘫痪

### 项目改造

#### 注册中心配置 

新建Maven项目 KSpringCloud-Eureka

导入依赖

```xml
 <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>
```

新建配置文件application.yml

```yml
server:
  port: 7001

#Eureka
eureka:
  instance:
    hostname: localhost #服务端实例名称
  client:
    register-with-eureka: false #是否向注册中心注册，由于自身为注册中心节点，不需要注册
    fetch-registry: false #false表示自己为注册中心
    service-url: #监控界面
       defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

新建启动类Eureka_7001.java

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class Eureka_7001 {
  public static void main(String[] args) {
    SpringApplication.run(Eureka_7001.class,args);
  }
}
```

#### KSpringCloud-Provider改造

新增pom依赖

```xml
  <!--Eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
```

新增application.yml配置

```yml
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:7001/eureka/
  instance:
    instance-id: KSpringCLoud-Provider8001
```

启动类加注解开启

```java 
@SpringBootApplication
@EnableEurekaClient
public class Provider_8001 {
  public static void main(String[] args) {
    SpringApplication.run(Provider_8001.class,args);
  }
}
```

先开启 KSpringCloud-Eureka，再开启KSpringCloud-Provider，访问localhost：7001可发现服务已经完成注册

关闭KSpringCloud-Provider，等待一会儿，可发现

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210401141453812.png" alt="image-20210401141453812" style="zoom:67%;" />

这行红字代表触发了保护机制 

> 自我保护机制

 EureKa自我保护机制：好死不如赖活着
一句话总结就是：某时刻某一个微服务不可用，eureka不会立即清理，依旧会对该微服务的信息进行保存！

默认情况下，当eureka server在一定时间内没有收到实例的心跳，便会把该实例从注册表中删除（默认是90秒），但是，如果短时间内丢失大量的实例心跳，便会触发eureka server的自我保护机制，比如在开发测试时，需要频繁地重启微服务实例，但是我们很少会把eureka server一起重启（因为在开发过程中不会修改eureka注册中心），当一分钟内收到的心跳数大量减少时，会触发该保护机制。可以在eureka管理界面看到Renews threshold和Renews(last min)，当后者（最后一分钟收到的心跳数）小于前者（心跳阈值）的时候，触发保护机制，会出现红色的警告：EMERGENCY!EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT.RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEGING EXPIRED JUST TO BE SAFE.从警告中可以看到，eureka认为虽然收不到实例的心跳，但它认为实例还是健康的，eureka会保护这些实例，不会把它们从注册表中删掉。

该保护机制的目的是避免网络连接故障，在发生网络故障时，微服务和注册中心之间无法正常通信，但服务本身是健康的，不应该注销该服务，如果eureka因网络故障而把微服务误删了，那即使网络恢复了，该微服务也不会重新注册到eureka server了，因为只有在微服务启动的时候才会发起注册请求，后面只会发送心跳和服务列表请求，这样的话，该实例虽然是运行着，但永远不会被其它服务所感知。所以，eureka server在短时间内丢失过多的客户端心跳时，会进入自我保护模式，该模式下，eureka会保护注册表中的信息，不在注销任何微服务，当网络故障恢复后，eureka会自动退出保护模式。自我保护模式可以让集群更加健壮。

但是我们在开发测试阶段，需要频繁地重启发布，如果触发了保护机制，则旧的服务实例没有被删除，这时请求有可能跑到旧的实例中，而该实例已经关闭了，这就导致请求错误，影响开发测试。所以，在开发测试阶段，我们可以把自我保护模式关闭，只需在eureka server配置文件中加上如下配置即可：eureka.server.enable-self-preservation=false【不推荐关闭自我保护机制】



#### 注册中心集群配置

前文实现了 KSpringCloud-Eureka的配置，现在来配置集群

新建 KSpringCloud-Eureka02和 KSpringCloud-Eureka03除了端口号为7002和7003外，同 KSpringCloud-Eureka没有区别 

因为现在是在一台电脑上模拟三个节点完成集群配置，因此，需要做额外的改善

> 修改主机映射

1. C:\Windows\System32\drivers\etc目录下打开文件hosts
2. 修改hosts，在末尾增加代码如下：

```
127.0.0.1 eureka7001.com
127.0.0.1 eureka7002.com
127.0.0.1 eureka7003.com
```

> 改造注册中心模块7001、7002、7003

以7001为例

修改后的application.yml如下：

```yml
server:
  port: 7001

#Eureka
eureka:
  instance:
    hostname: eureka7001.com #服务端实例名称
  client:
    register-with-eureka: false #是否向注册中心注册，由于自身为注册中心节点，不需要注册
    fetch-registry: false #false表示自己为注册中心
    service-url: #监控界面
       #集群配置7001关联7002和7003
       defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

7002和7003类似7001，需要修改 hostname和 defaultZone， defaultZone这里，7001关联7002、7003；7002关联7001、7002；7003关联7001、7002

> 修改KSpringCloud-Provider

修改配置文件application.yml

```yml
#Eureka
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
  instance:
    instance-id: KSpringCLoud-Provider8001
```

这里我做了一个测试，特意只向7001和 7002进行了注册，测试内容和结果如下：

> 测试

测试 一：启动7001、7002、7003、KSpringCloud-Provider

测试结果：集群配置成功，例如7002可看到7001、7003的集群

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210402101052594.png" alt="image-20210402101052594" style="zoom:50%;" />

测试二：关闭7001，模拟7001宕机 ，启动7002、7003、KSpringCloud-Provider

测试结果：访问7002和7003面板，可看到集群和服务，但是会触发保护机制，服务仍可调用

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210402102038413.png" alt="image-20210402102038413" style="zoom:50%;" />



测试三：关闭7001和7002，模拟宕机，启动7003、KSpringCloud-Provider

测试结果：触发保护机制，服务仍可调用

![image-20210402102712245](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210402102712245.png)



## Ribbon

### 概念

> 负载均衡以及Ribbon

Ribbon是什么？

Spring Cloud Ribbon 是基于Netflix Ribbon 实现的一套客户端负载均衡的工具。
简单的说，Ribbon 是 Netflix 发布的开源项目，主要功能是提供客户端的软件负载均衡算法，将 Netflix 的中间层服务连接在一起。Ribbon 的客户端组件提供一系列完整的配置项，如：连接超时、重试等。简单的说，就是在配置文件中列出 LoadBalancer (简称LB：负载均衡) 后面所有的及其，Ribbon 会自动的帮助你基于某种规则 (如简单轮询，随机连接等等) 去连接这些机器。我们也容易使用 Ribbon 实现自定义的负载均衡算法！

> Ribbon能干嘛？

<img src="https://img-blog.csdnimg.cn/20201121103107791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70#pic_center" alt="img" style="zoom:50%;" />

LB，即负载均衡 (LoadBalancer) ，在微服务或分布式集群中经常用的一种应用。
负载均衡简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA (高用)。
常见的负载均衡软件有 Nginx、Lvs 等等。
Dubbo、SpringCloud 中均给我们提供了负载均衡，SpringCloud 的负载均衡算法可以自定义。
负载均衡简单分类：
集中式LB
即在服务的提供方和消费方之间使用独立的LB设施，如Nginx(反向代理服务器)，由该设施负责把访问请求通过某种策略转发至服务的提供方！
进程式 LB
将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选出一个合适的服务器。
Ribbon 就属于进程内LB，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址！

### Ribbon集成

Ribbon是在消费者端集成的类库，在本文中，也就是需要在KSpringCloud-Customer中集成

1. 导入依赖

```xml
 <!--Ribbon-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-ribbon</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <!--Eureka: Ribbon需要从Eureka服务中心获取要拿什么-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
```

2. 配置application

```yml
#Eureka
eureka:
  client:
    register-with-eureka: false #消费者不需要注册服务
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

3. 主启动类加上@EnableEurekaClient注解，开启Eureka

```java
@SpringBootApplication
@EnableEurekaClient //开启Eureka 客户端
public class Customer_80 {
  public static void main(String[] args) {
    SpringApplication.run(Customer_80.class,args);
  }
}
```

4. 负载均衡配置

在restTemplate Bean上加注解@LoadBalance

```java
@Configuration
public class ConfigBean {

   @Bean
   @LoadBalanced //配置负载均衡实现RestTemplate
  public RestTemplate getRestTemplate(){
     return new RestTemplate();
   }
}

```

5. controller修改

controller 修改主要是服务调用地址修改，使用注册中心的服务不需要指导具体地址，只需要直到服务名即可

```java
//使用Ribbon之后，服务地址应该是服务名，变量，不能写死
  //private static final String REST_URL_PREFIX= "http://localhost:8001";
  private static final String REST_URL_PREFIX = "http://dept-provider";
```



### 项目改造

服务提供者仅有8001时，我们看不出负载均衡效果，因此，我们对项目进行改造，改造后的项目框架如下：



<img src="https://img-blog.csdnimg.cn/20200521131315626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70#pic_center" alt="img" style="zoom:50%;" />

> 增加数据库db02\db03

```sql
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for dept
-- ----------------------------
DROP TABLE IF EXISTS `dept`;
CREATE TABLE `dept`  (
  `dno` bigint(0) NOT NULL AUTO_INCREMENT,
  `dname` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `dsource` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`dno`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci;

insert into dept(dname, dsource) VALUES ('开发部',DATABASE());
insert into dept(dname, dsource) VALUES ('市场部',DATABASE());
insert into dept(dname, dsource) VALUES ('运营部',DATABASE());
insert into dept(dname, dsource) VALUES ('人事部',DATABASE());
insert into dept(dname, dsource) VALUES ('财务部',DATABASE());


SET FOREIGN_KEY_CHECKS = 1;
```

> 增加模块8002、8003

具体配置同8001

需要修改的部分：

```
1. yml文件中server port分别为8002 和 8003
2. yml文件中instance:instance-id，修改可以更好的区分
3. 启动类名字修改一下，也是为了更好的区分
```

## Feign

> Feign简介

Feign是声明式Web Service客户端，它让微服务之间的调用变得更简单，类似controller调用service。SpringCloud集成了Ribbon和Eureka，可以使用Feigin提供负载均衡的http客户端

只需要创建一个接口，然后添加注解即可~

Feign，主要是社区版，大家都习惯面向接口编程。这个是很多开发人员的规范。

> 调用微服务访问两种方法

微服务名字 【ribbon】
      接口和注解 【feign】

> Feign能干什么？

Feign旨在使编写Java Http客户端变得更容易
前面在使用Ribbon + RestTemplate时，利用RestTemplate对Http请求的封装处理，形成了一套模板化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一个客户端类来包装这些依赖服务的调用。所以，Feign在此基础上做了进一步的封装，由他来帮助我们定义和实现依赖服务接口的定义，在Feign的实现下，我们只需要创建一个接口并使用注解的方式来配置它 (类似以前Dao接口上标注Mapper注解，现在是一个微服务接口上面标注一个Feign注解)，即可完成对服务提供方的接口绑定，简化了使用Spring Cloud Ribbon 时，自动封装服务调用客户端的开发量。
Feign默认集成了Ribbon

利用Ribbon维护了MicroServiceCloud-Dept的服务列表信息，并且通过轮询实现了客户端的负载均衡，而与Ribbon不同的是，通过Feign只需要定义服务绑定接口且以声明式的方法，优雅而简单的实现了服务调用。

### 项目改造

1. 新建项目，用来展示Feign，与KSpringCloud-Customer对比

新建KSpringCloud-Customer-Feign 模块，项目内所有配置和代码同KSpringCloud-Customer

2. 导入依赖

本项目中，因为把pojo类放在API模块中，且 ，想把FeignClient放在API中，所以，KSpringCloud-API和KSpringCloud-Customer-Feign 都需要新增Feign依赖

```xml
 <!--Feign的依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-feign</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
```

3. FeignClient编写

在KSpringCloud-API中新建DeptClientService

```java
@FeignClient(value = "dept-provider")
public interface DeptClientService {

  @GetMapping("/dept/get/{id}")
  public Dept queryById(@PathVariable("id") Long id);

  @GetMapping("/dept/list")
  public List<Dept> queryAll();

  @GetMapping("/dept/add")
  public Boolean addDept(Dept dept);
}

```



4. 重写DController

重写之后的DController.java如下：

```java
@RestController
public class DController{

  @Autowired
  private DeptClientService deptClientService;

  @RequestMapping("/cus/dept/add")
  public boolean add(Dept dept){
    return deptClientService.addDept(dept);
  }

  @RequestMapping("/cus/dept/get/{id}")
  public Dept get(@PathVariable("id") Long id){
    return deptClientService.queryById(id);
  }

  @RequestMapping("/cus/dept/list")
  public List<Dept> list(){
    return deptClientService.queryAll();
  }
  
}
```



## Hystrix

### 服务熔断

> 分布式系统面临的问题

复杂分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免失败！

> 服务雪崩

 多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其他的微服务，这就是所谓的“扇出”，如果扇出的链路上**某个微服务的调用响应时间过长，或者不可用**，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201121144830148.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70#pic_center)

 对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几十秒内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障，**这些都表示需要对故障和延迟进行隔离和管理，以达到单个依赖关系的失败而不影响整个应用程序或系统运行**。

 我们需要，**弃车保帅**！

>  什么是Hystrix？

 **Hystrix**是一个应用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等，**Hystrix** 能够保证在一个依赖出问题的情况下，不会导致整个体系服务失败，避免级联故障，以提高分布式系统的弹性。

 “**断路器**”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控 (类似熔断保险丝) ，**向调用方返回一个服务预期的，可处理的备选响应 (FallBack) ，而不是长时间的等待或者抛出调用方法无法处理的异常，这样就可以保证了服务调用方的线程不会被长时间，不必要的占用**，从而避免了故障在分布式系统中的蔓延，乃至雪崩。

> Hystrix能干嘛？

- 服务降级
- 服务熔断
- 服务限流
- 接近实时的监控
- …

当一切正常时，请求流可以如下所示：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRodWIuY29tL05ldGZsaXgvSHlzdHJpeC93aWtpL2ltYWdlcy9zb2EtMS02NDAucG5n?x-oss-process=image/format,png)

当许多后端系统中有一个潜在阻塞服务时，它可以阻止整个用户请求：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRodWIuY29tL05ldGZsaXgvSHlzdHJpeC93aWtpL2ltYWdlcy9zb2EtMi02NDAucG5n?x-oss-process=image/format,png)

随着大容量通信量的增加，单个后端依赖项的潜在性会导致所有服务器上的所有资源在几秒钟内饱和。

应用程序中通过网络或客户端库可能导致网络请求的每个点都是潜在故障的来源。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，从而备份队列、线程和其他系统资源，从而导致更多跨系统的级联故障。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRodWIuY29tL05ldGZsaXgvSHlzdHJpeC93aWtpL2ltYWdlcy9zb2EtMy02NDAucG5n?x-oss-process=image/format,png)

当使用**Hystrix**包装每个基础依赖项时，上面的图表中所示的体系结构会发生类似于以下关系图的变化。**每个依赖项是相互隔离的**，限制在延迟发生时它可以填充的资源中，并包含在回退逻辑中，该逻辑决定在依赖项中发生任何类型的故障时要做出什么样的响应：



> 服务熔断

什么是服务熔断?

 **熔断机制是赌赢雪崩效应的一种微服务链路保护机制**。

 当扇出链路的某个微服务不可用或者响应时间太长时，会进行服务的降级，**进而熔断该节点微服务的调用，快速返回错误的响应信息**。检测到该节点微服务调用响应正常后恢复调用链路。在SpringCloud框架里熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阀值缺省是**5秒内20次调用失败，就会启动熔断机制**。熔断机制的注解是：`@HystrixCommand`。

服务熔断解决如下问题：

- 当所依赖的对象不稳定时，能够起到快速失败的目的；
- 快速失败后，能够根据一定的算法动态试探所依赖对象是否恢复。

#### 项目改造

1. 新建项目KSpringCloud-hystrix-8001，复制KSpringCloud-Provider下的resources、全部java代码、pom文件
2. 导入依赖

```xml
     <!--导入Hystrix依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
```

3. 修改配置文件application.yml

修改一下instance-id，以便于区分

```yml
#Eureka
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
  instance:
    instance-id: xy_mdel_serverhystrix8001
    prefer-ip-address: false  #显示 IP地址，而非localhost
```



4. 修改controller

```java
@RestController
public class Dcontroller {

  @Autowired
   private Dservice dservice;

  /**
   * 正常方案
   * */
  @GetMapping("/dept/get/{id}")
  @HystrixCommand(fallbackMethod = "getHystrixDept")
  public Dept getDept(@PathVariable("id") Long id){
    Dept dept = dservice.queryById(id);
    if (dept==null){
      throw new RuntimeException();
    }
    return dept;
  }
  /**
   * 熔断
   * 备选方案
   * @param id
   * */
  public Dept getHystrixDept(@PathVariable("id") Long id){
    return new Dept().setDno(id)
        .setDname("这个id=>"+id+",没有对应的信息,null---@Hystrix~")
        .setDsource("在MySQL中没有这个数据库");
  }
}
```

因此，**为了避免因某个微服务后台出现异常或错误而导致整个应用或网页报错，使用熔断是必要的**

### 服务降级

> 概念

 服务降级是指 当服务器压力剧增的情况下，根据实际业务情况及流量，对一些服务和页面有策略的不处理，或换种简单的方式处理，从而释放服务器资源以保证核心业务正常运作或高效运作。说白了，就是尽可能的把系统资源让给优先级高的服务。

资源有限，而请求是无限的。如果在并发高峰期，不做服务降级处理，一方面肯定会影响整体服务的性能，严重的话可能会导致宕机某些重要的服务不可用。所以，一般在高峰期，为了保证核心功能服务的可用性，都要对某些服务降级处理。比如当双11活动时，把交易无关的服务统统降级，如查看蚂蚁深林，查看历史订单等等。

服务降级主要用于什么场景呢？当整个微服务架构整体的负载超出了预设的上限阈值或即将到来的流量预计将会超过预设的阈值时，为了保证重要或基本的服务能正常运行，可以将一些 不重要 或 不紧急 的服务或任务进行服务的 延迟使用 或 暂停使用。

降级的方式可以根据业务来，可以延迟服务，比如延迟给用户增加积分，只是放到一个缓存中，等服务平稳之后再执行 ；或者在粒度范围内关闭服务，比如关闭相关文章的推荐。

![img](https://img-blog.csdnimg.cn/20200521132141732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70#pic_center)

由上图可得，当某一时间内服务A的访问量暴增，而B和C的访问量较少，为了缓解A服务的压力，这时候需要B和C暂时关闭一些服务功能，去承担A的部分服务，从而为A分担压力，叫做服务降级。

服务降级需要考虑的问题
1）那些服务是核心服务，哪些服务是非核心服务
2）那些服务可以支持降级，那些服务不能支持降级，降级策略是什么
3）除服务降级之外是否存在更复杂的业务放通场景，策略是什么？

> 自动降级分类

1）超时降级：主要配置好超时时间和超时重试次数和机制，并使用异步机制探测回复情况

2）失败次数降级：主要是一些不稳定的api，当失败调用次数达到一定阀值自动降级，同样要使用异步机制探测回复情况

3）故障降级：比如要调用的远程服务挂掉了（网络故障、DNS故障、http服务返回错误的状态码、rpc服务抛出异常），则可以直接降级。降级后的处理方案有：默认值（比如库存服务挂了，返回默认现货）、兜底数据（比如广告挂了，返回提前准备好的一些静态页面）、缓存（之前暂存的一些缓存数据）

4）限流降级：秒杀或者抢购一些限购商品时，此时可能会因为访问量太大而导致系统崩溃，此时会使用限流来进行限制访问量，当达到限流阀值，后续请求会被降级；降级后的处理方案可以是：排队页面（将用户导流到排队页面等一会重试）、无货（直接告知用户没货了）、错误页（如活动太火爆了，稍后重试）。

#### 项目改造

1. 修改KSpringCloud-API

新增DeptClientServiceFallBackFactory类，作为DeptClientService的异常回调

```java
@Component
public class DeptClientServiceFallBackFactory  implements FallbackFactory {
  /**
   * 为DeptClientService中所有方法提供服务降级操作
   * */
  @Override
  public DeptClientService create(Throwable throwable) {
    return new DeptClientService() {
      @Override
      public Dept queryById(Long id) {
        return new Dept().setDno(id)
            .setDname("ID："+id+" 不存在，客户端提供了服务降级信息")
            .setDsource("没有数据");
      }

      @Override
      public List<Dept> queryAll() {
        return null;
      }

      @Override
      public Boolean addDept(Dept dept) {
        return null;
      }
    };
  }
}
```

在DeptClientService中指定该类为服务降级操作类，具体来说，就是在FeignClient注解中指定该类为fallbackFactory

```java
@FeignClient(value = "xy-model-server",fallbackFactory = DeptClientServiceFallBackFactory.class)
public interface DeptClientService {

  @GetMapping("/dept/get/{id}")
  public Dept queryById(@PathVariable("id") Long id);

  @GetMapping("/dept/list")
  public List<Dept> queryAll();

  @GetMapping("/dept/add")
  public Boolean addDept(Dept dept);
}
```

2. 修改KSpringCloud-Customer-Feign

修改application.yml，开启Hystrix注解

```yml
#hystrix配置
feign:
    hystrix:
       enabled: true
```



### 服务熔断和降级的区别

作用位置不一样：

服务熔断—>服务端：某个服务超时或异常，引起熔断~，类似于保险丝(自我熔断)
       服务降级—>客户端：从整体网站请求负载考虑，当某个服务熔断或者关闭之后，服务将不再被调用，此时在客户端，我们可以准备一个 FallBackFactory ，返回一个默认的值(缺省值)。会导致整体的服务下降，但是好歹能用，比直接挂掉强。

触发原因不太一样：

服务熔断一般是某个服务（下游服务）故障引起，而服务降级一般是从整体负荷考虑；管理目标的层次不太一样，熔断其实是一个框架级的处理，每个微服务都需要（无层级之分），而降级一般需要对业务有层级之分（比如降级一般是从最外围服务开始）
实现方式不太一样，服务降级具有代码侵入性(由控制器完成/或自动降级)，熔断一般称为自我熔断。

熔断，降级，限流：

限流：限制并发的请求访问量，超过阈值则拒绝；

降级：服务分优先级，牺牲非核心服务（不可用），保证核心服务稳定；从整体负荷考虑；

熔断：依赖的下游服务故障触发熔断，避免引发本系统崩溃；系统自动执行和恢复


### Dashboard 流监控

#### 项目改造

1. 新建maven module   创建KSpringCloud-Customer-HystrixDashboard
2. 导入依赖,重点，导入Hystrix依赖

```xml
    <dependencies>
        <!--Hystrix依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <!--dashboard依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <!--Ribbon-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-ribbon</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <!--Eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <!--实体类+web-->
        <dependency>
            <groupId>com.sha</groupId>
            <artifactId>KSpringCloud-API</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>

    </dependencies>
```

3. 配置文件application.yml

```yml
server:
  port: 9001
```

4. 编写启动类

```java
@SpringBootApplication
@EnableHystrixDashboard
public class DeptConsumerDashboard_9001 {
  public static void main(String[] args) {
    SpringApplication.run(DeptConsumerDashboard_9001.class,args);
  }
}
```

5. 改造需要被监控的项目

被监控的项目是提供服务的Provider,若Provider想要被监控，需要导入两个依赖

```xml
       <!--actuator完善监控信息-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
    </dependencies>
   <!--导入Hystrix依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
```

然后在启动类中注册一个servlet

```java
 //增加一个 Servlet
  @Bean
  public ServletRegistrationBean hystrixMetricsStreamServlet(){
    ServletRegistrationBean registrationBean = new ServletRegistrationBean(new HystrixMetricsStreamServlet());
    //访问该页面就是监控页面
    registrationBean.addUrlMappings("/actuator/hystrix.stream");
    return registrationBean;
  }
```

例如这里监控项目KSpringCloud-hystrix-8001

导入依赖之后，启动类修改如下：

![image-20210409095220813](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210409095220813.png)

启动项目KSpringCloud-Customer-HystrixDashboard和 KSpringCloud-hystrix-8001以及Eureka7001

访问http://localhost:9001/hystrix，可看到

![image-20210409095350143](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210409095350143.png)

将需要监控的页面，也就是http://localhost:8001/actuator/hystrix.stream填入，进入之后，可发现

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210409095440070.png" alt="image-20210409095440070" style="zoom:50%;" />

打开 http://localhost:8001/actuator/hystrix.stream，可以看到监控情况

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210409095536620.png" alt="image-20210409095536620" style="zoom:33%;" />

## Zuul

### 概述

> 什么是zuul?

 Zull包含了对请求的路由(用来跳转的)和过滤两个最主要功能：

 其中路由功能负责将外部请求转发到具体的微服务实例上，是实现外部访问统一入口的基础，而过滤器功能则负责对请求的处理过程进行干预，是实现请求校验，服务聚合等功能的基础。Zuul和Eureka进行整合，将Zuul自身注册为Eureka服务治理下的应用，同时从Eureka中获得其他服务的消息，也即以后的访问微服务都是通过Zuul跳转后获得。

![img](https://img-blog.csdnimg.cn/20201122103018821.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70#pic_center)

注意：Zuul 服务最终还是会注册进 Eureka

提供：代理 + 路由 + 过滤 三大功能！

> Zuul 能干嘛？
>

路由
过滤
官方文档：https://github.com/Netflix/zuul/

### 项目改造--路由

1. 新建项目KSpringCloud-Zuul
2. 导入依赖

```xml
    <dependencies>
        <!--导入zuul依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zuul</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <!--Hystrix依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <!--dashboard依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <!--Ribbon-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-ribbon</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <!--Eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
        <!--实体类+web-->
        <dependency>
            <groupId>com.sha</groupId>
            <artifactId>KSpringCloud-API</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>
```

3. 新增配置文件application.yml

```yml
server:
  port: 9527
spring:
  application:
    name: xy_model_zuul

#Eureka
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
  instance:
    instance-id: xy_model_zuul9527
    prefer-ip-address: true #隐藏真实ID

info:
  app.name: KSZuul
  company.name: None
  version: 1.0

zuul:
  routes:
    xy-model-server: #服务名
       path: /server/**   #用以代替服务名
  prefix: /xy             #统一公共前缀
  ignored-services: "*"   # 不能再使用这个路径访问了，*： 忽略,隐藏全部的服务名称~
```

4. 新增启动类

```java
@SpringBootApplication
@EnableZuulProxy  //开启Zuul代理
public class ZuulApplication9527 {
  public static void main(String[] args) {
    SpringApplication.run(ZuulApplication9527.class,args);
  }
}
```

5. 启动Eureka7001、provider、Zuul

![image-20210409154638118](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210409154638118.png)

![image-20210409154658616](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210409154658616.png)



## SpringCloud-Config

### 概念

Dalston.RELEASE

Spring Cloud Config为分布式系统中的外部配置提供服务器和客户端支持。使用Config Server，您可以在所有环境中管理应用程序的外部属性。客户端和服务器上的概念映射与Spring Environment和PropertySource抽象相同，因此它们与Spring应用程序非常契合，但可以与任何以任何语言运行的应用程序一起使用。随着应用程序通过从开发人员到测试和生产的部署流程，您可以管理这些环境之间的配置，并确定应用程序具有迁移时需要运行的一切。服务器存储后端的默认实现使用git，因此它轻松支持标签版本的配置环境，以及可以访问用于管理内容的各种工具。很容易添加替代实现，并使用Spring配置将其插入。

> 概述

分布式系统面临的–配置文件问题

微服务意味着要将单体应用中的业务拆分成一个个子服务，每个服务的粒度相对较小，因此系统中会出现大量的服务，由于每个服务都需要必要的配置信息才能运行，所以一套集中式的，动态的配置管理设施是必不可少的。spring cloud提供了configServer来解决这个问题，我们每一个微服务自己带着一个application.yml，那上百个的配置文件修改起来，令人头疼！

什么是SpringCloud config分布式配置中心？

<img src="https://img-blog.csdnimg.cn/202005211322530.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70#pic_center" alt="img" style="zoom:67%;" />

 spring cloud config 为微服务架构中的微服务提供集中化的外部支持，配置服务器为各个不同微服务应用的所有环节提供了一个中心化的外部配置。

 spring cloud config 分为服务端和客户端两部分。

 服务端也称为 分布式配置中心，它是一个独立的微服务应用，用来连接配置服务器并为客户端提供获取配置信息，加密，解密信息等访问接口。

 客户端则是通过指定的配置中心来管理应用资源，以及与业务相关的配置内容，并在启动的时候从配置中心获取和加载配置信息。配置服务器默认采用git来存储配置信息，这样就有助于对环境配置进行版本管理。并且可用通过git客户端工具来方便的管理和访问配置内容。

spring cloud config 分布式配置中心能干嘛？

集中式管理配置文件
不同环境，不同配置，动态化的配置更新，分环境部署，比如 /dev /test /prod /beta /release
运行期间动态调整配置，不再需要在每个服务部署的机器上编写配置文件，服务会向配置中心统一拉取配置自己的信息
当配置发生变动时，服务不需要重启，即可感知到配置的变化，并应用新的配置
将配置信息以REST接口的形式暴露
spring cloud config 分布式配置中心与GitHub整合

 由于spring cloud config 默认使用git来存储配置文件 (也有其他方式，比如自持SVN 和本地文件)，但是最推荐的还是git ，而且使用的是 http / https 访问的形式。

### 项目改造

改造前提，设置一个远程仓库

这里使用码云，创建了一个远程仓库

仓库地址：https://gitee.com/shashar/kspring-cloud-config.git

创建文件application.yml

```yml
spring:
    profiles:
        active: dev
        
---
        
spring: 
    profiles: dev
    application:
        name: springcloud-config-dev
        
---
spring: 
    profiles: test
    application:
        name: springcloud-config-test
```



#### 第一个模块：KSpringCloud-Config-Server-3344

1. 导入依赖

```xml
    <dependencies>
        <!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--config-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
            <version>2.1.1.RELEASE</version>
        </dependency>
        <!--eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
    </dependencies>
```

2. 配置文件

```yml
server:
  port: 3344

spring:
  application:
    name: springcloud-config-server
  # 连接码云远程仓库
  cloud:
    config:
      server:
        git:
          # 注意是https的而不是ssh
          uri: https://gitee.com/shashar/kspring-cloud-config.git
          # 通过 config-server可以连接到git，访问其中的资源以及配置~

# 不加这个配置会报Cannot execute request on any known server 这个错：连接Eureka服务端地址不对
# 或者直接注释掉eureka依赖 这里暂时用不到eureka
eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```

3. 新建启动类

```java
@SpringBootApplication
@EnableConfigServer // 开启spring cloud config server服务
public class Config_Server_3344 {
  public static void main(String[] args) {
    SpringApplication.run(Config_Server_3344.class,args);
  }
}
```

启动，访问测试

地址一 ：http://localhost:3344/application-test.yml

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210412164151295.png" alt="image-20210412164151295" style="zoom:50%;" />

地址二：http://localhost:3344/application/test/master

![image-20210412164224608](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210412164224608.png)

……

#### 第二个模块KSPringCloud-Config-Client-3355

SpringCloud config是服务端连接远程仓，客户端连接服务端，进而获取远程仓中的配置

1. 在远程仓中创建配置文件config-client.yml

```yml
spring:
  profies:
    active: dev

---
server:
  port: 8201
spring:
  profiles: dev
  application:
    name: KSpringCloud-Config-Server

eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
  
---
server:
  port: 8202
spring:
  profiles: test
  application:
    name: KSpringCloud-Config-Server

eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
```

2. 本地项目中导入依赖

```xml
<dependencies>
    <!--config-->
    <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-start -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-config</artifactId>
        <version>2.1.1.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

</dependencies>
```

3. 编写配置类

编写系统级别的配置类bootstrap.yml，这里配置了需要读取的资源名称、分支和服务端的地址

```yml
# 系统级别的配置
spring:
  cloud:
    config:
      name: config-client # 需要从git上读取的资源名称，不要后缀
      profile: test #也可以改成dev，test端口号是8202，dev是8201
      label: master
      uri: http://localhost:3344
```

编写用户级别的配置类application.yml

```yml
# 用户级别的配置
spring:
  application:
    name: springcloud-config-client
```

4. 编写controller类ConfigClientController.java

```java
@RestController
public class  ConfigClientController {

  @Value("${spring.application.name}")
  private String applicationName; //获取微服务名称

  @Value("${eureka.client.service-url.defaultZone}")
  private String eurekaServer; //获取Eureka服务

  @Value("${server.port}")
  private String port; //获取服务端的端口号


  @RequestMapping("/config")
  public String getConfig(){
    return "applicationName:"+applicationName +
        "eurekaServer:"+eurekaServer +
        "port:"+port;
  }
}
```

@Value可从application.yml或application.properties中读取数据，这里是读取远程仓中的数据

5. 添加启动类

```java
@SpringBootApplication
public class ConfigClient_3355 {
  public static void main(String[] args) {
    SpringApplication.run(ConfigClient_3355.class,args);
  }
}
```

6. 启动和访问测试

启动3344和3355，首先测试一下3344，看看是否能正常连接远程仓

访问地址： http://localhost:3344/config-client-dev.yml

访问地址二：http://localhost:8202/config



#### 注册中心模块改造

1. 在远程仓中新建config-eureka.yml文件

```yml
spring:
  profiles:
    active: dev


---
spring:
  profiles: dev
  application:
    name: KSpringCLoud-Eureka
server:
  port: 7001

#Eureka
eureka:
  instance:
    hostname: eureka7001.com #服务端实例名称
  client:
    register-with-eureka: false #是否向注册中心注册，由于自身为注册中心节点，不需要注册
    fetch-registry: false #false表示自己为注册中心
    service-url: #监控界面
       #集群配置7001关联7002和7003
       defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/

---
spring:
  profiles: test
  application:
    name: KSpringCLoud-Eureka
server:
  port: 7001

#Eureka
eureka:
  instance:
    hostname: eureka7001.com #服务端实例名称
  client:
    register-with-eureka: false #是否向注册中心注册，由于自身为注册中心节点，不需要注册
    fetch-registry: false #false表示自己为注册中心
    service-url: #监控界面
       #集群配置7001关联7002和7003
       defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

2. 新建模块KSpringCloud-Config-Eureka，复制KSpringCloud-Eureka全部代码，pom文件
3. 导入pom依赖

```xml
<!--config-->
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-config -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
    <version>2.1.1.RELEASE</version>
</dependency>
```



4. 删除application.yml,新建bootstrap.yml，配置配置文件的信息

```yml
#系统级别配置，配置配置文件读取的位置、名称、远程分支和环境
spring:
  cloud:
    config:
      name: config-eureka  #远程仓中的名称
      label: master        #分支
      profile: dev         #开发环境
      uri: http://localhost:3344   #服务器地址
```

3. 修改启动类名称（也可以不改，用以区分）

```java
@SpringBootApplication
@EnableEurekaServer
public class ConfigEureka_7001 {
  public static void main(String[] args) {
    SpringApplication.run(ConfigEureka_7001.class,args);
  }
}
```

4. 启动3344和KSpringCloud-Config-Eureka

访问地址localhost：7001

出现Eureka中心默认样式，配置成功

#### 服务提供者改造

其实步骤都是差不多的，额，再来一遍

1. 在远程仓中新建**config-dept.yml** 

```yml
spring:
  profiles:
    active: dev


---
server:
  port: 8001

#mybatis
mybatis:
  type-aliases-package: com.sha.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml
  config-location: classpath:mybatis/mybatis-config.xml

#spring
spring:
  profiles: dev
  application:
    name: KSpringCLoud-Dept
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db01?useUnicode=true&characterEncording=utf-8
    username: root
    password: shasha123

#Eureka
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
  instance:
    instance-id: xy_mdel_serverhystrix8001
    prefer-ip-address: true  #显示 IP地址，而非localhost

# info配置
info:
  app.name: KS
  company.name: KSTWO

---
server:
  port: 8001

#mybatis
mybatis:
  type-aliases-package: com.sha.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml
  config-location: classpath:mybatis/mybatis-config.xml

#spring
spring:
  profiles: test
  application:
    name: KSpringCLoud-Dept
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db02?useUnicode=true&characterEncording=utf-8
    username: root
    password: shasha123

#Eureka
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
  instance:
    instance-id: xy_mdel_serverhystrix8001
    prefer-ip-address: true  #显示 IP地址，而非localhost

# info配置
info:
  app.name: KS
  company.name: KSTWO
```

2. 新建module  KSpringCloud-Config-Provider，复制KSpringCloud-Provider全部代码、pom 文件
3. 导入pom 依赖

```xml
<!--config-->
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-config -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
    <version>2.1.1.RELEASE</version>
</dependency>
```



4. 删除application.yml,新建bootstrap.yml，配置配置文件的信息

```yml
spring:
  cloud:
    config:
      name: config-dept
      label: master
      profile: dev
      uri: http://localhost:3344
```



5. 启动测试

启动之后，服务已经正常注册进入Eureka，且可以正常使用



### 总结：

需要一个服务端，其余为客户端

服务端的依赖

```xml
    <!--config-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
            <version>2.1.1.RELEASE</version>
        </dependency>
```

服务端需要配置远程仓

```yml
spring:
  application:
    name: springcloud-config-server
  # 连接码云远程仓库
  cloud:
    config:
      server:
        git:
          # 注意是https的而不是ssh
          uri: https://gitee.com/shashar/kspring-cloud-config.git
          # 通过 config-server可以连接到git，访问其中的资源以及配置~
```



所有需要集中管理的客户端，需要导入依赖

```xml
<!--config-->
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-config -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
    <version>2.1.1.RELEASE</version>
</dependency>
```

本地需要配置一个bootstrap.yml，里面配置服务端的地址以及配置文件的名称等信息

```yml
#系统级别配置，配置配置文件读取的位置、名称、远程分支和环境
spring:
  cloud:
    config:
      name: config-eureka  #远程仓中的名称
      label: master        #分支
      profile: dev         #开发环境
      uri: http://localhost:3344   #服务器地址
```













