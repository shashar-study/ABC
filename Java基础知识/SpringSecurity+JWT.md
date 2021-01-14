





# SpringSecurity

### 简介

Spring Security 是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入 spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！

记住几个类：

- WebSecurityConfigurerAdapter：自定义Security策略
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：开启WebSecurity模式

Spring Security的两个主要目标是 “认证” 和 “授权”（访问控制）。

**“认证”（Authentication）**

身份验证是关于验证您的凭据，如用户名/用户ID和密码，以验证您的身份。

身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。

 **“授权” （Authorization）**

授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置，几乎任何内容）的完全权限。

这个概念是通用的，而不是只在Spring Security 中存在。

### 实战步骤

#### 导入依赖

引入 Spring Security 模块

```pom
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

#### 编写 Spring Security 配置类

编写一个类，实现WebSecurityConfigurerAdapter，例如

```java
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@EnableWebSecurity // 开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {

   @Override
   protected void configure(HttpSecurity http) throws Exception {
       
  }
}
```

@EnableWebSecurity注解用于开启webSecurity 功能，格式都是统一的，想要开启某个功能，就可以使用@EnableXXX这个注释

##### 自定义授权方法

重写WebSecurityConfigurerAdapter中的方法，在代码自动生成generator中选择override  methods，再选择传参是HttpSecurity http的configure方法，确定，即可得到上述空的代码模块

在这个方法里可以自定义授权方法

###### 放行和拦截

首先，http.authorizeRequests()，这个方法可以自定义拦截和放行规则

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201020112117002.png" alt="image-20201020112117002" style="zoom:33%;" />

这个方法中有这些子方法，类似的配置如下

```java
 http.authorizeRequests()
        .antMatchers("/").permitAll()//放行
        .antMatchers("/a/**").hasRole("a")
        .antMatchers("/b/**").hasRole("b")
        .antMatchers("/c/**").hasRole("c");
```

###### 登陆与注销登陆

在这个configure大方法中，还可以开启自动配置的登录功能

只需要一行代码：http.formLogin();

这个方法默认访问SpringSecurity自带的/login界面，进行登录，若是登陆失败，会重定向到/login？error

我们可以修改这个方法的默认配置，以实现自定义登陆逻辑

例如：

```java
http.formLogin().loginPage("/toLogin");
```

可以指定登陆界面，这样默认的登陆界面就被覆盖掉了

```java
http.formLogin()
  .usernameParameter("usernameX")
  .passwordParameter("passwordX")
  .loginPage("/toLogin")
  .loginProcessingUrl("/login"); // 登陆表单提交请求
```

.usernameParameter("username")
  .passwordParameter("password")用于接收前端界面参数名称是usernameX和passwordX的参数传入username和password，两个带X的地方是可以自己修改的，但是要和前端对应上

.loginPage("/toLogin")会覆盖原有的登陆界面

.loginProcessingUrl("/login"); 会将表单提交到/login这个路径中去



我们还可以开启自带的登陆注销功能和记住我功能

```java
//开启自动配置的注销的功能
      // /logout 注销请求
   http.logout();
```

这个默认注销后会来到登录界面，如果想要修改注销后转到的界面，可以使用

```java
// .logoutSuccessUrl("/"); 注销成功来到首页
http.logout().logoutSuccessUrl("/");
```



如果注销404了，就是因为它默认防止csrf跨站请求伪造，因为会产生安全问题，我们可以将请求改为post表单提交，或者在spring security中关闭csrf功能；我们试试：在 配置中增加 `http.csrf().disable();`

```java
http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
http.logout().logoutSuccessUrl("/");
```

###### 记住我功能

```java
   //记住我
   http.rememberMe();
```

如果是使用SpringSecurity自带的login的话，页面会出现记住我的提示框，选中之后可正常操作

若是使用自定义登陆逻辑，需要获取前端提交的参数，如下：

```java
http.rememberMe().rememberMeParameter("remember");
```

##### 自定义认证

实现自定义认证需要重写这个方法，和自定义授权传参不同configure(AuthenticationManagerBuilder auth)

AuthenticationManagerBuilder auth中有途中所示的这些方法

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201020111933538.png" alt="image-20201020111933538" style="zoom:33%;" />

认证有两种大的来源，一个是内存，这个可以在后端将代码写死，注册固定的用户分配权限，登陆时从内容中读取信息进行验证，即inMemoryAuthentication，两一个是读取数据库中，即jdbcAuthentication，后者需要注册Driver，调用数据库。

这里使用了内存方式，如下：

```java
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
 auth.inMemoryAuthentication()
     .passwordEncoder(new BCryptPasswordEncoder())
     .withUser("one").password(new BCryptPasswordEncoder().encode("1234")).roles("a")
     .and()
     .withUser("two").password(new BCryptPasswordEncoder().encode("123")).roles("a","b")
     .and()
     .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("a","b","c");
 
}
```

需要注意的是，为了安全考虑，密码不可以以明文方式存储在内存中，需要进行加密，这里选用了一种加密方式，可以换的，其次，一个用户可以拥有多个角色，用逗号隔开即可

调用数据库的方式我之后会在补上，暂时还不会。

























