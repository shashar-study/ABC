问题发生的情况如下：

配置SpringMVC环境之后，后端传递JSON数据，

controller层代码如下：

```java
@ResponseBody
  @RequestMapping(value = "/json2",produces = "application/json;charset=utf-8")
  public String json2() {
    return JSONUtils.getJson(new User("吴亦凡",31,"男"));
  }
```

前端显示的数据中，中文显示为问号

网上说的produces = "application/json;charset=utf-8"方式不能适用

配置文件修改如下：

首先是web.xml，request和response请求都需要设置过滤器，修改后全貌如下下：

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
    <!--针对Request-->
    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param>
    <!--针对Response-->
    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/</url-pattern>
  </filter-mapping>

</web-app>
```

其次是springmvc-servlet.xml，对所有application/json;charset=UTF-8和text/html;charset=UTF-8进行扫描

修改后全貌如下：

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
        <!-- 配置自动扫描:解决中文乱码问题 -->
        <mvc:annotation-driven>
                <mvc:message-converters>
                        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                                <property name="supportedMediaTypes">
                                        <list>
                                                <value>application/json;charset=UTF-8</value>
                                                <value>text/html;charset=UTF-8</value>
                                        </list>
                                </property>
                        </bean>
                </mvc:message-converters>
        </mvc:annotation-driven>
</beans>
```



