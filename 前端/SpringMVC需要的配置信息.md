这个是我用来弄前后端交互JSON和AJAX的时候弄得，整个配置流程如下：

> 创建项目

首先新建一个module

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210421163447455.png" alt="image-20210421163447455" style="zoom: 33%;" />

初始生成了一个包含main和webapp的包

> 导包

SpringMVC有很多包，我使用了如下依赖，包含大部分所 需要的包

```xml
   <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>3.2.17.RELEASE</version>
    </dependency>
```

> 配置文件编写



> 配置web.xml

位置在WEB-INF之下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
  <!--1.注册servlet-->
  <servlet>
    <servlet-name>SpringMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class><!--通过初始化参数指定SpringMVC配置文件的位置，进行关联--><init-param><param-name>contextConfigLocation</param-name><param-value>classpath:springmvc-servlet.xml</param-value></init-param><!-- 启动顺序，数字越小，启动越早 --><load-on-startup>1</load-on-startup>
  </servlet>
  <!--所有请求都会被springmvc拦截 -->
  <servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
  <filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param></filter>
  <filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/</url-pattern>
  </filter-mapping>
</web-app>
```



> 配置springmvc-servlet.xml

位置在resources下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xmlns:context="http://www.springframework.org/schema/context"
              xmlns:mvc="http://www.springframework.org/schema/mvc"
               xsi:schemaLocation="http://www.springframework.org/schema/beans
               http://www.springframework.org/schema/beans/spring-beans.xsd
               http://www.springframework.org/schema/context
               https://www.springframework.org/schema/context/spring-context.xsd
               http://www.springframework.org/schema/mvc
               https://www.springframework.org/schema/mvc/spring-mvc.xsd">
        <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
        <context:component-scan base-package="com.sha.controller"/>
        <!-- 视图解析器 -->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
                 id="internalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!-- 后缀 -->
        <property name="suffix" value=".jsp" />
        </bean>
</beans>
```

