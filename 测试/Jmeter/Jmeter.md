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
2. 添加线程组和HTTP请求
3. 