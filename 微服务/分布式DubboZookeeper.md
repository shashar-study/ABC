##  分布式系统

> 概念

分布式系统是一个硬件或软件组件分布在不同的网络计算机上，彼此之间仅仅通过消息传递进行通信和协调的系统。简单来说就是一群独立计算机集合共同对外提供服务，但是对于系统的用户来说，就像是一台计算机在提供服务一样。

> 网站应用的演进

随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，亟需一个治理系统确保架构有条不紊的演进。

![image](https://dubbo.apache.org/imgs/user/dubbo-architecture-roadmap.jpg)

> 单一应用架构

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

> 垂直应用架构

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，提升效率的方法之一是将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。

> 分布式服务架构

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的分布式服务框架(RPC)是关键。

> 流动计算架构

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于提高机器利用率的资源调度和治理中心(SOA)是关键。

## RPC

RPC（Remote Procedure Call）远程过程调用.

核心：通信、序列化与反序列化



## Zookeeper

### 安装

http://archive.apache.org/dist/zookeeper/

下载后标为tar.gz的压缩包，并解压到想要安装的目录下，3.5之后需要下载带bin 的

解压完成之后，进入bin目录，win电脑点击zkServer.cmd，双击运行，发现出现闪退情况

```
#闪退问题解决思路
#1.查看错误信息
#编辑zkServer.cmd文件，在底部endlocal前添加pause,确保出错时，cmd不闪退可以看到错误提示信息
#2.重新运行zkServer.cmd,查看报错提示，发现程序找不到conf\zoo.cfg
#复制一份zoo_sample.cfg，改为zoo.cfg
```

解决完成后，双击zkServer.cmd运行，可正常启动

![image-20210322165038226](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210322165038226.png)

可以双击打开客户端zkCli.cmd，测试连接

![image-20210322165116562](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210322165116562.png)

### zkCli.cmd简单命令使用测试

下面进行了一些简单的客户端命令

```shell
#1.ls /  列出Zookeeper根下保存的所有节点
[zk: localhost:2181(CONNECTED) 0] ls /
[zookeeper]                           #由于没有配置其他节点，所以仅有zookeeper一个节点

#2.create -e /name value    创建一个节点，节点名称为name，值为value
[zk: localhost:2181(CONNECTED) 1] create -e /node 123
Created /node                         #
[zk: localhost:2181(CONNECTED) 2] ls /
[node, zookeeper]            

#3.get /name 获取节点name 的值
[zk: localhost:2181(CONNECTED) 3] get /zookeeper

[zk: localhost:2181(CONNECTED) 4] get /node
123
```

## Dubbox

 <img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210322142102651.png" alt="image-20210322142102651" style="zoom: 50%;" />



init 初始化 ；async 异步；sync 同步   

除invoke之外，都是异步的

### 注册中心可视化面板

> 安装

https://github.com/apache/dubbo-admin/tree/master

选择master分支，下载zip，解压到安装目录

这是一个SpringBoot项目，打开dubbo-admin\src\main\resources\application.properties文件

将这里的注册中心地址修改为自己配置的Zookeeper地址和端口号，如果没有 修改Zookeeper默认选项，则不需要修改

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210323110745953.png" alt="image-20210323110745953" style="zoom: 67%;" />

> 使用

1. 在项目目录下打包dubbo-admin

```shell 
mvn clean package -D maven.test.skip=true
```

该目录下有三个子项目，生成的jar包分别位于三个项目中的target目录之下

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210323112325312.png" alt="image-20210323112325312" style="zoom:50%;" />

2. 执行dubbo-admin下的jar 包

在jar包目录下打开cmd，使用Java -jar命令

一开始是报错的，因为之前关闭了Zookeeper的server端，但是，由于Dubbox会处于持续监听的状态，因此，不会影响程序运行，只需要打开zkServer.cmd即可

打开之后，注册中心会被监听到并连接上，输入http://localhost:7001/  进入控制面板，==默认的用户名和密码是root-root==，登录之后，即可进入如下界面

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210323113253053.png" alt="image-20210323113253053" style="zoom:33%;" />

在系统管理中，可以查看到注册中心的信息

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210323113332405.png" alt="image-20210323113332405" style="zoom:50%;" />

### 服务的注册

按照角色划分，有服务提供者和服务消费者，均需要向服务注册中心提供服务

> 服务提供者注册服务

1. 导入依赖

```
       <!--Dubbox和Zookeeper-->
        <!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo-spring-boot-starter -->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>2.7.9</version>
        </dependency>
        <!--Zookeeper-->
        <!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
        <dependency>
            <groupId>com.github.sgroschupf</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.1</version>
        </dependency>

        <!--解决日志冲突-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>5.1.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
            <version>5.1.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.6.2</version>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

2. 配置

```properties
server.port=8001

#服务名
dubbo.application.name=server-provider
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
#需要被扫描的服务
dubbo.scan.base-packages=com.sha.provider.service.Impl
```

3. 编写服务

基本流程为interface、interfaceImplement，并且，需要在Impl上加注解

```java
public interface TicketService {

  public String getTicket();
}
```

```java
@Component
@DubboService
public class TicketServiceImpl implements TicketService {
  @Override
  public String getTicket() {
    return "AAABBBCCCDDD";
  }
}
```

到这一步，服务提供者的注册已经完成。整个流程比较清晰，首先，导包；其次，配置服务的一些基本信息；最后，实际的调用API

新版的Dubbo不适用@service而是使用@DubboService作为扫描的标志，这里注意，需要和properties中配置的包目录相同

启动服务，在可视化面板中，出现了以下内容：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210323162911366.png" alt="image-20210323162911366" style="zoom:50%;" />

> 服务消费者进行注册

1. 导包

这一步同上

2. 配置

```properties
server.port=8002

#服务名
dubbo.application.name=server-customer
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

3. 调用服务

```java
import org.apache.dubbo.config.annotation.DubboReference;
import org.springframework.stereotype.Service;

@Service //放入容器中
public class UserService {

  @DubboReference//引用，方法一：POM坐标  方法二：在相同包下定义同名接口
  TicketService ticketService;

  public void BuyTicket(){
    String ticket = ticketService.getTicket();
    System.out.println("服务消费者"+ticket);
  }
}
```

在这里，使用了方法二，将服务提供者的TicketService复制过来，放到相同目录之下即可

```java
//调用测试
@SpringBootTest
class CustomerApplicationTests {

  @Autowired
  UserService userService;

  @Test
  void contextLoads() {
    userService.BuyTicket();
  }
}
```

最终的结果如下：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210324104412506.png" alt="image-20210324104412506" style="zoom:50%;" />

启动该应用，在可视化面板中，可以看到：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210324104513854.png" alt="image-20210324104513854" style="zoom:50%;" />

应用有两个，但是注册的服务仅有一个



> 小结 

前提：zookeeper  server 开启

注册服务提供者

1. 导包
2. 配置注册中心地址、自己的服务名、服务名所在的包（需要扫描的包）
3. 在需要注册的 服务上加注解@DubboxService



注册服务消费者

1. 导包
2. 配置注册中心地址、自己的服务名
3. 从远程调用服务@DubboReference

