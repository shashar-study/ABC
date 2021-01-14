## quickStart

1.导入依赖

```java
<dependencies>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-core</artifactId>
        <version>1.4.1</version>
    </dependency>

    <!-- configure logging -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>1.7.30</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.30</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
</dependencies>
```

2.配置文件

这里的配置文件我是在官网上下载的，直接粘贴在resources文件下

第一个，文件名：log4j.properties，配置了日志打印的一些规则

```properties
log4j.rootLogger=INFO, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m %n

# General Apache libraries
log4j.logger.org.apache=WARN

# Spring
log4j.logger.org.springframework=WARN

# Default Shiro logging
log4j.logger.org.apache.shiro=INFO

# Disable verbose logging
log4j.logger.org.apache.shiro.util.ThreadContext=WARN
log4j.logger.org.apache.shiro.cache.ehcache.EhCache=WARN
```

第二个，文件名：shiro.ini  配置了一些角色和权限

```ini
[users]
# user 'root' with password 'secret' and the 'admin' role
root = secret, admin
# user 'guest' with the password 'guest' and the 'guest' role
guest = guest, guest
# user 'presidentskroob' with password '12345' ("That's the same combination on
# my luggage!!!" ;)), and role 'president'
presidentskroob = 12345, president
# user 'darkhelmet' with password 'ludicrousspeed' and roles 'darklord' and 'schwartz'
darkhelmet = ludicrousspeed, darklord, schwartz
# user 'lonestarr' with password 'vespa' and roles 'goodguy' and 'schwartz'
lonestarr = vespa, goodguy, schwartz

# -----------------------------------------------------------------------------
# Roles with assigned permissions
# 
# Each line conforms to the format defined in the
# org.apache.shiro.realm.text.TextConfigurationRealm#setRoleDefinitions JavaDoc
# -----------------------------------------------------------------------------
[roles]
# 'admin' role has all permissions, indicated by the wildcard '*'
admin = *
# The 'schwartz' role can do anything (*) with any lightsaber:
schwartz = lightsaber:*
# The 'goodguy' role is allowed to 'drive' (action) the winnebago (type) with
# license plate 'eagle5' (instance specific id)
goodguy = winnebago:drive:eagle5
```

3. QuickStart.java

```java
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.*;
import org.apache.shiro.config.IniSecurityManagerFactory;
//import org.apache.shiro.ini.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.session.Session;
import org.apache.shiro.subject.Subject;
//import org.apache.shiro.lang.util.Factory;
import org.apache.shiro.util.Factory;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;


/**
 * Simple Quickstart application showing how to use Shiro's API.
 *
 * @since 0.9 RC2
 */
public class Quickstart {

    //使用日志输出
    private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);


    public static void main(String[] args) {
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        SecurityManager securityManager = factory.getInstance();
        SecurityUtils.setSecurityManager(securityManager);

        // Now that a simple Shiro environment is set up, let's see what you can do:

        // get the currently executing user:
        Subject currentUser = SecurityUtils.getSubject();

        // Do some stuff with a Session (no need for a web or EJB container!!!)
        Session session = currentUser.getSession();
        session.setAttribute("someKey", "aValue");
        String value = (String) session.getAttribute("someKey");
        if (value.equals("aValue")) {
            log.info("Retrieved the correct value! [" + value + "]");
        }

        // let's login the current user so we can check against roles and permissions:
        if (!currentUser.isAuthenticated()) {
            UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
            token.setRememberMe(true);
            try {
                currentUser.login(token);
            } catch (UnknownAccountException uae) {
                log.info("There is no user with username of " + token.getPrincipal());
            } catch (IncorrectCredentialsException ice) {
                log.info("Password for account " + token.getPrincipal() + " was incorrect!");
            } catch (LockedAccountException lae) {
                log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                        "Please contact your administrator to unlock it.");
            }
            // ... catch more exceptions here (maybe custom ones specific to your application?
            catch (AuthenticationException ae) {
                //unexpected condition?  error?
            }
        }

        //say who they are:
        //print their identifying principal (in this case, a username):
        log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");

        //test a role:
        if (currentUser.hasRole("schwartz")) {
            log.info("May the Schwartz be with you!");
        } else {
            log.info("Hello, mere mortal.");
        }

        //test a typed permission (not instance-level)
        if (currentUser.isPermitted("lightsaber:wield")) {
            log.info("You may use a lightsaber ring.  Use it wisely.");
        } else {
            log.info("Sorry, lightsaber rings are for schwartz masters only.");
        }

        //a (very powerful) Instance Level permission:
        if (currentUser.isPermitted("winnebago:drive:eagle5")) {
            log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                    "Here are the keys - have fun!");
        } else {
            log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
        }

        //all done - log out!
        currentUser.logout();

        System.exit(0);
    }
}
```

在这里比较重要的语法整理如下：

```java
  //获取当前用户，使用Subject获取
  Subject currentUser = SecurityUtils.getSubject();
  //通过Session获取用户的信息，也可以设置
  Session session = currentUser.getSession();
  currentUser.isAuthenticated()
  currentUser.getPrincipal()
  currentUser.isPermitted("lightsaber:wield")
  currentUser.hasRole("schwartz")
```



## 整合SpringBoot

### 架构：

```
shiro中的主要部分
Subject 用户
SecurityManager  用户管理
Realm   连接数据库
```

使用步骤：

##### 导入依赖

```java
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-spring</artifactId>
    <version>1.6.0</version>
</dependency>
```

除此之外，我还导入了thymeleaf，依赖如下：

```java
<!--thymeleaf 模板-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring5</artifactId>
</dependency>
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-java8time</artifactId>
</dependency>
```

##### 编写配置类

在架构中给出了shiro 的三个核心对象，在配置文件中，需要对他们分别进行自定义

ShiroFilterFactoryBean          用户

DefaultWebSecurityManager  用户管理

Realm 数据库连接对象

配置的时候我们可以倒着完成三个Bean的配置，因为这三个是嵌套的关系

声明一个总的配置类ShiroConfig.java

###### 数据库连接配置

1. 新建类UserRealm.java，extends AuthorizingRealm，并重写该类中的两个方法（授权）doGetAuthorizationInfo和（认证）doGetAuthenticationInfo
2. 连接数据库

​      在数据库中创建表格User，具体如下：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201021165919162.png" alt="image-20201021165919162" style="zoom:33%;" />

导入依赖，这里使用了Druid、mybatis、mysql

```java
<!--连接数据库-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.21</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.24</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>

<!--mybatis-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.3</version>
</dependency>
```



在resources下新建文件application.yml，进行数据库连接配置

```yml
spring:
  datasource:
    url: jdbc:mysql://XXX:3306/shiro-springboot?serverTimezone=Asia/Shanghai&characterEncoding=utf8&useSSL=false
    username: XXX
    password: XXXX
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource



#druid 数据源专属配置
initialSize: 5
minIdle: 5
maxActive: 20
maxWait: 60000
timeBetweenEvictionRunsMillis: 60000
minEvictableIdleTimeMillis: 30000
validationQuery: SELECT 1 FROM DUAL
testWhileIdle: true
testOnBorrow: false
testOnReturn: false
poolPreparedStatements: true

filters: stat,wall,log4j
maxPoolPreparedStatementPerConnectionSize: 20
useGlobalDataSourceStat: true
connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis:500
```



这里需要注意的是，yml 的格式是key:   value,而不是key:value,没有加空格会报错

在application.properties 中进行配置mybatis实体类包（pojo）和mapper 映射位置的配置，接下来，我们要根据所配置的位置，将对应的实体类和xml文件写好

```Properties
mybatis.type-aliases-package=com.ls.study.pojo
mybatis.mapper-locations=classpath:mapper/*.xml
```

这里，实体类User.java代码如下：

```java
package com.ls.study.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * Author：shasha<br>
 * Time：2020/10/21 <br>
 * Description： <br>
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
  private int id;
  private String name;
  private String pwd;
  private String perms;
}
```

可以看到，这里使用了Lombok注解 ，省略了get、set方法，以及生成了全参、无参构造方法

xml文件位置如下：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201022095058228.png" alt="image-20201022095058228" style="zoom:33%;" />

代码为：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.ls.study.mapper.UserMapper">

    <select id="queryUserByName" parameterType="String" resultType="User">
        SELECT * FROM `shiro-springboot`.user where name = #{name}
    </select>
</mapper>
```

接下来，补充完整UserMapper

```java
package com.ls.study.mapper;

import com.ls.study.pojo.User;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

/**
 * Author：shasha<br>
 * Time：2020/10/21 <br>
 * Description： <br>
 */
@Repository
@Mapper
public interface UserMapper {

  public User queryUserByName(String name);
    
}
```

以及service和serviceImpl

```java
package com.ls.study.service;

import com.ls.study.pojo.User;

/**
 * Author：shasha<br>
 * Time：2020/10/21 <br>
 * Description： <br>
 */
public interface UserService {
  public User queryUserByName(String name);
}
```

```java
package com.ls.study.service.impl;

import com.ls.study.mapper.UserMapper;
import com.ls.study.pojo.User;
import com.ls.study.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

/**
 * Author：shasha<br>
 * Time：2020/10/21 <br>
 * Description： <br>
 */
@Service
public class UserServiceImpl implements UserService {
  @Autowired
  UserMapper userMapper;

  @Override
  public User queryUserByName(String name) {
      return userMapper.queryUserByName(name);
  }
}
```

至此，数据库的连接配置就已经完成了,我们在预先声明的ShiroConfig.java 中注入Realm的Bean，如下：

```java
/**
 * Realm 数据库连接对象
 * */
@Bean
public UserRealm userRealm(){
  return new UserRealm();
}
```

接下来，重写UserRealm中的方法

```java
package com.ls.study.config;

import com.ls.study.pojo.User;
import com.ls.study.service.UserService;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.session.Session;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.subject.Subject;
import org.springframework.beans.factory.annotation.Autowired;

/**
 * Author：shasha<br>
 * Time：2020/10/20 <br>
 * Description： <br>
 */
public class UserRealm extends AuthorizingRealm {
  @Autowired
  UserService userService;
  //授权
  @Override
  protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
    System.out.println("执行了授权doGetAuthorizationInfo方法");
    //给用户授权
    SimpleAuthorizationInfo info=new SimpleAuthorizationInfo();
    /*给用户授予user：add权限
    这个是直接写死了的做法，这样每个登陆进来的用户都有这个权限，接下来要从数据库中获取
    info.addStringPermission("user:add");
    */
   //获取当前用户
    Subject subject = SecurityUtils.getSubject();
    User currentUser =  (User)subject.getPrincipal();
    info.addStringPermission(currentUser.getPerms());
    return info;
  }
  //认证
  @Override
  protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
    System.out.println("执行了认证doGetAuthorizationInfo方法");
    //连接数据库做查询
    UsernamePasswordToken usernamePasswordToken = (UsernamePasswordToken) token;
    User user = userService.queryUserByName(usernamePasswordToken.getUsername());
    if(user==null){
      //没有这个用户
      return null;
    }
    /**
     * 登陆之后将用户存储到session中，
     * 前端可以通过读取session来判断用户是否已经登陆，
     * 进而进行登陆注销按钮的显示
     * */
    Subject currentSubject = SecurityUtils.getSubject();
    Session session = currentSubject.getSession();
    session.setAttribute("loginUser",user);
    //密码认证，shiro来做
    return new SimpleAuthenticationInfo(user,user.getPwd(),"");
  }
}
```

shiro会将前端传来的数据封装成一个用户名和密码封装成token，可以连接数据库进行查询，如果没有 该用户名直接返回null，shiro 会进行处理提示用户不存在，关于密码的认证shiro已经是实现了，可以直接调用；

SecurityUtils.getSubject()可以获取subject，即当前用户，可以将当前用户的信息存入session，然后前端 thymeleaf可以通过session判断当前是否已经登陆，以及权限等内容；认证也是同理，将数据库中查询到的权限通过subject--》SimpleAuthorizationInfo.addStringPermission()传递给当前用户；

###### 用户管理

同理，我们在预先声明的ShiroConfig.java 中注入DefaultWebSecurityManager的Bean，如下：

```java
/**
 * DefaultWebSecurityManager
 * 用户管理
 * */
@Bean(name="securityManager")
public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
  DefaultWebSecurityManager securityManager=new DefaultWebSecurityManager();
  //绑定UserRealm
  securityManager.setRealm(userRealm);
  return  securityManager;
}
```

使用@Qualifier绑定userRealm，传参

###### 用户

```java
/**
 * ShiroFilterFactoryBean
 * 用户
 * */
  @Bean
  public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager  defaultWebSecurityManager){
    ShiroFilterFactoryBean filterFactoryBean = new ShiroFilterFactoryBean();
    //设置安全管理器
    filterFactoryBean.setSecurityManager(defaultWebSecurityManager);
    //添加shiro的内置过滤器
        /*
            anon: 无需认证就可访问
            authc：必须认证才能访问
            user：必须拥有记住我功能才能访问
            perms: 拥有对某个资源的权限才能访问
            role:拥有某个角色权限才能访问
       */

    Map<String, String> filterMap = new LinkedHashMap<>();

    //授权
    filterMap.put("/user/add","perms[user:add]");
    filterMap.put("/user/update","perms[user:update]");

//        filterMap.put("/user/add","authc");
//        filterMap.put("/user/update","authc");
    filterMap.put("/user/*","authc");
    //设置登出
    filterMap.put("/logout", "logout");

    filterFactoryBean.setFilterChainDefinitionMap(filterMap);

    //设置登录请求
    filterFactoryBean.setLoginUrl("/toLogin");
    //设置未授权页面
    filterFactoryBean.setUnauthorizedUrl("/noauth");

    return filterFactoryBean;
  }
```

在ShiroFilterFactoryBean做拦截，以及一些页面做了授权，有某些权限才可以访问某些controller下的路径

##### 一些资源

 MyController.java

```java
package com.ls.study.controller;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.subject.Subject;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.ModelAndView;

/**
 * Author：shasha<br>
 * Time：2020/10/20 <br>
 * Description： <br>
 */
@RestController
public class MyController {

    @RequestMapping({"/","/index"})
    public  ModelAndView   toIndex(Model model){
      model.addAttribute("msg","测试环境");
      return new ModelAndView("index");
    }

    @RequestMapping("/user/add")
    public ModelAndView add(){
      return new ModelAndView("/user/add");
    }

  @RequestMapping("/user/update")
  public ModelAndView update(){
    return new ModelAndView("/user/update");
  }

  @RequestMapping("/toLogin")
  public ModelAndView tologin(){
    return new ModelAndView("login");
  }

  @RequestMapping("/login")
  public ModelAndView login(String username,String password,Model model){
    //获取当前用户
    Subject subject = SecurityUtils.getSubject();
    //封装用户登录数据
    UsernamePasswordToken token =new UsernamePasswordToken(username,password);

    try{
      subject.login(token);
      return new ModelAndView("index");
    }catch (UnknownAccountException e){//用户名不存在
      model.addAttribute("msg","用户名错误");
      return new ModelAndView("login");
    }catch (IncorrectCredentialsException e){
      model.addAttribute("msg","密码错误");
      return new ModelAndView("login");
    }
  }

  @RequestMapping("/noauth")
  public String unauthorized(){
    return "未授权";
  }
}
```

index.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org"
                xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h2>首页</h2>
<p th:text="${msg}"></p>
<hr>
<div th:if="${session.loginUser==null}">
    <a th:href="@{/toLogin}">登陆</a>
</div>
<div shiro:hasPermission="user:add">
    <a th:href="@{/user/add}">add</a>
</div>
<div shiro:hasPermission="user:update">
    <a th:href="@{/user/update}">update</a>
</div>

</body>
</html>
```

login.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf,org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>登录</h1>
<hr>
<p th:text="${msg}" style="color:red;"></p>
<form th:action="@{/login}">
    <p>用户名：<input type="text" name="username"></p>
    <p>密码：<input type="text" name="password"></p>
    <p><input type="submit"></p>

</form>
</body>
</html>
```

add.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>add用户</h1>
</body>
</html>
```

update.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>update用户</h1>
</body>
</html>
```

整个项目的结构如下：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201022105708702.png" alt="image-20201022105708702" style="zoom:50%;" />