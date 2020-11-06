# SpringBoot中的JPA的使用

## 预备工作 

1. 创建工程

创建一个SpringBoot工程，必须选上MySQL和JPA

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20200902151318855.png" alt="image-20200902151318855" style="zoom:50%;" /> 

2.在application.properties中添加数据库配置信息

```properties
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://localhost:3306/jpaStudy
spring.datasource.username=root
spring.datasource.password=Shashar123
```

3.根据数据库表格形式创建实体类，dao和controller

实体类，加上@entity注解 ，字段加上@ID、@GeneratedValue(strategy = GenerationType.IDENTITY)

dao层，接口，需要继承JpaRepository,例如

```java

public interface UserRepository extends JpaRepository<User, Long> {
}

```

里面定义方法，如 User findUserById(int id);

controller层，路径@Controller @RequestMapping("/user")

service 接口，定义方法，serviceImplements实现类，定义接口实现方法，调用dao 种方法

