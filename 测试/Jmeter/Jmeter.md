# Jmeter

## 安装与启动

1. 需要Java环境1.8以上
2. 官网下载zip

https://jmeter.apache.org/download_jmeter.cgi

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210414143924237.png" alt="image-20210414143924237" style="zoom:33%;" />

Win10，就下载Binaries下的zip 即可，下载之后，解压到指定目录

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210414144023440.png" alt="image-20210414144023440" style="zoom:25%;" />

3. 打开bin目录下的jmeter.bat，即可启动Jmeter

这两个界面，命令行不可以关闭

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210415150132149.png" alt="image-20210415150132149" style="zoom: 25%;" />

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210414144319920.png" alt="image-20210414144319920" style="zoom: 25%;" />

4. 插件的下载安装

​     下载地址

https://jmeter-plugins.org/install/Install/

![image-20210415142051934](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210415142051934.png)

## 目录介绍

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210415150410667.png" alt="image-20210415150410667" style="zoom:25%;" />

> bin目录--存放启动脚本、配置文件、模板等文件

- examples：该目录下存放Jmeter官方给的请求模板
- report-template：该目录下存放Jmeter的报告模板
- templates：该目录下存放Jmeter的各类配置模板，例如：JDBC、Beanshell、ThinkTime等
- ApacheJMeter.jar和jmeter.bat：windows环境下，JmeterGUI工具的启动脚本，ApacheJMeter.jar如果启动失败则需要配置环境变量
- jmeter.properties：配置文件
- jmeter-server.bat：用于分布式
- shutdown.cmd：硬停止
- stoptest.cmd：软停止
- xxx.sh：sh后缀的脚本文件用于Linux或者Mac操作系统下
- user.properties：用户配置文件

- jmeter.bat  Windows下启动文件
- jmeter.ssh  Linux 下启动文件
- jmeter.properties 系统配置文件，每次修改后需要重启才能生效

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210415150446949.png" alt="image-20210415150446949" style="zoom:50%;" />

> docs目录

接口文档目录

> extras目录-存放Build等配置，用于第三方集成构建

扩展插件目录，可以使用Ant来实现自动化测试，例如批量脚本执行

> lib——Jmeter依赖的或者二次开发的jar包、第三方插件

ext：第三方插件

> licenses——许可证等

> printable_docs——帮助文档

index.html：帮助文档的主入口

component_reference.html 核心组件的帮助文档



## 测试入门

> 案例一：向百度发送请求

1. 启动jmeter.bat ，打开之后默认创建了测试计划
2. 邮件测试计划，添加线程组
3. 右键线程组，添加HTTP请求和查看结果树
4. 在HTTP请求中，进行如下配置

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419095912975.png" alt="image-20210419095912975" style="zoom:33%;" />

5. 点击绿色运行键

![image-20210419100058324](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419100058324.png)



测试结果可以在查看结果树之中看



### 入门组件详解

https://blog.csdn.net/automationwei/article/details/80999631

- 测试计划：项目名

- 线程组：业务流程

  ​      线程属性：

  ​            线程数：表示请求的虚拟用户数量

  ​            ramp-up：准备时长。启动所有线程数所需要的时间，单位：秒若需要立               即启动所有线程，将值设置为0即可

  ​            循环次数：线程的循环次数，每个线程发送请求的次数，若选择了永远，会一直执行，直到脚本停止。还可以勾选调度器，设置开始和结束时间。

  

  取样器：向服务器发送请求并记录相应数据

  

  

  > 运行方式 

  按照线程方式运行

  GUI运行模式地电脑本身资源消耗较大，无法实现大的并发和压力测试 

  使用命令行模式进行大型并发和压力测试

  使用GUI模式主要目的是为了编写和调试Jmeter测试脚本

  

  > ​     测试计划要素

  测试计划

  测试计划中至少一个线程组

  线程组中至少一个取样器

  测试计划中必须要有监听器，了解发送的请求和服务器的响应内容

### BadBoy脚本录制

用处：可以生成Jmeter脚本

> 使用步骤

1. 安装
2. 打开

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419160153928.png" alt="image-20210419160153928" style="zoom:33%;" />

在地址栏输入需要访问的地址

![image-20210419160622641](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419160622641.png)

确认开始之后会自动进行录制，进行所需要的测试步骤

测试步骤结束之后，点击红色录制按钮停止录制

在File--》Export to Jmeter导出脚本

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419160743290.png" alt="image-20210419160743290" style="zoom:33%;" />

然后Jmeter客户端打开

最后添加监听器--查看结果树



### Jmeter自身代理录制移动端

> 配置Jmeter

1. 打开Jmeter，创建新的测试计划 
2. 测试计划右键，添加线程组和==非测试元件--》HTTP代理服务器==
3. 配置HTTP代理服务器
   -  端口号默认:8888
   - HTTPS Domains: localhost或是自己的IP
   - 目标控制器：测试计划-->线程组
   - 点击HTTP代理服务器里面State:启动，然后如果弹出对话框，确认

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419164556194.png" alt="image-20210419164556194" style="zoom:50%;" />![image-20210419164832802](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419164832802.png)

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419164556194.png" alt="image-20210419164556194" style="zoom:50%;" />![image-20210419164832802](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419164832802.png)



然后发现弹出了如下界面：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419165011210.png" alt="image-20210419165011210" style="zoom:50%;" />

接下来配置移动端



> 配置移动端--手机

这个我没有做，但是大致 步骤是：

创建手机模拟器

更改网络连接，连上本机IP和端口，然后开始操作，Jmeter这边会自动开启录制

选择停止，生成脚本

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210419165410225.png" alt="image-20210419165410225" style="zoom:50%;" />

## 配置文件修改

> 中文乱码问题解决

打开jmeter.properties

搜索：sampleresult.default.encoding

将默认的编码改为utf-8,去掉注释#号，修改之后如图：



<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210420105142746.png" alt="image-20210420105142746" style="zoom:50%;" />



> 修改默认语言

打开jmeter.properties

搜索：language

去掉注释，改成zh_CN，修改之后如图：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210420105458093.png" alt="image-20210420105458093" style="zoom: 50%;" />



## 接口测试回顾

> 知识回顾

接口三要素

​         请求地址/方式 

​         请求参数

​         返回值

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

HTTP协议:

​     请求方式：

​             get：

​                    带参数

​                     不带参数

​             post:

​                     x-www-form-urlencoded:键值对形式  content-type:application/ x-www-form-urlencoded

​                     JSON格式数据：content-type:application/json



​            put

​            delete



## Jmeter测试











​                                  

