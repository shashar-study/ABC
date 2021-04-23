## JSON

(JavaScript Object Notation,JS对象标记.)

前后端分离,数据交互变得异常重要,JSON就是王者.

JSON是JS对象的toString()，使用文本表示JS对象信息，本质上是字符串

### JS语法格式

- 对象标识为键值对
- 数据用逗号分割
- 花括号保存对象
- 方括号保存数组

在前端页面中，一个使用样例

```javascript
<script>
    //编写一个对象 
    var user = {
        name:"user",
        age:3,
        sex:"男"
    };
    //输出这个对象
    console.log(user);
    //{
    //   name:"user",
    //   age:3,
    //   sex:"男"
    //}
    
    //将JS对象转换为JSON格式
    var str = JSON.stringify(user);
    console.log(str);
    //{
    //"name":"user",
    // "age":3,
    //"sex":"男"
    //}
    
    //将JSON格式转换为JS对象
    var obj = JSON.parse(str);
    console.log(obj);
    //{
    //   name:"user",
    //   age:3,
    //   sex:"男"
    //}
</script>    
```

### @RequestBody

@RequestBody的作用是将前端传来的json格式的数据转为自己定义好的javabean对象，前端和后端定义的对象要能对应上

例如前端传回参数：

<img src="https://img-blog.csdnimg.cn/20190306150101340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Nob3dhZHdhbGtlcg==,size_16,color_FFFFFF,t_70" alt="img" style="zoom:50%;" />

后端对应的JavaBean如下：

<img src="https://img-blog.csdnimg.cn/20190306150929712.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Nob3dhZHdhbGtlcg==,size_16,color_FFFFFF,t_70" alt="img" style="zoom:50%;" />

后端需要接收JSON参数并转换为对象，==在方法的参数前，使用@RequestBody参数标记==，转换之后可以使用类中定义的方法，用法如下：

![img](https://img-blog.csdnimg.cn/20190306150739305.png)

### @RequestParam

将请求参数绑定到你控制器的方法参数上（是springmvc中接收普通参数的注解）

@RequestBody是接收对象的

> 语法

语法：@RequestParam(value=”参数名”,required=”true/false”,defaultValue=””)

value：参数名

required：是否包含该参数，默认为true，表示该请求路径中必须包含该参数，如果不包含就报错。

defaultValue：默认参数值，如果设置了该值，required=true将失效，自动为false,如果没有传该参数，就使用默认值

```java
package com.day01springmvc.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.ModelAndView;
 
/**
 * @ Author     ：ShaoWei Sun.
 * @ Date       ：Created in 20:58 2018/11/16
 */
@Controller
@RequestMapping("hello")
public class HelloController2 {
 
    /**
     * 接收普通请求参数
     * http://localhost:8080/hello/show16?name=linuxsir
     * url参数中的name必须要和@RequestParam("name")一致
     * @return
     */
    @RequestMapping("show16")
    public ModelAndView test16(@RequestParam("name")String name){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("hello2");
        mv.addObject("msg", "接收普通的请求参数：" + name);
        return mv;
    }
 
    /**
     * 接收普通请求参数
     * http://localhost:8080/hello/show17
     * url中没有name参数不会报错、有就显示出来
     * @return
     */
    @RequestMapping("show17")
    public ModelAndView test17(@RequestParam(value="name",required=false)String name){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("hello2");
        mv.addObject("msg", "接收普通请求参数：" + name);
        return mv;
    }
 
    /**
     * 接收普通请求参数
     * http://localhost:8080/hello/show18?name=998 显示为998
     * http://localhost:8080/hello/show18?name 显示为hello
     * @return
     */
    @RequestMapping("show18")
    public ModelAndView test18(@RequestParam(value="name",required=true,defaultValue="hello")String name){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("hello2");
        mv.addObject("msg", "接收普通请求参数：" + name);
        return mv;
    }
 
}
```





### @ResponseBody

@ResponseBody的作用是将后端以`return`返回的javabean类型数据转为json类型数据，或者说，会将string对象以json格式的字符串返回。

==@ResponseBody用在方法名上==

![，](https://img-blog.csdnimg.cn/20190306153451564.png)



### 后端处理数据全流程

SpringMVC配置一下

导包

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.8</version>
    </dependency>
```



后端处理数据

```java
@Controller
public class JsonController {

	//正常返回的时候会走视图解析器，但是json需要的是返回一个字符串。
	//@ResponseBody将服务器端返回的对象转换为json对象响应回去。
	//解决乱码问题。produces = "application/json;charset=utf-8"
	@ResponseBody
	@RequestMapping(value = "json1",produces = "application/json;charset=utf-8")
	public String json1() throws JsonProcessingException {
		User user = new User("庞杰", 22, "男");
		//需要一个jackson的对象映射器，就是一个类，使用它可以直接将对象转换为json字符串
		ObjectMapper mapper = new ObjectMapper();
		//将java对象转换为json字符串
		String string = mapper.writeValueAsString(user);
		//@ResponseBody，使用这个注解会将string对象以json格式的字符串返回
		return string;
	}
	@ResponseBody
	@RequestMapping(value = "json2",produces = "application/json;charset=utf-8")
	public String json2() {
		return JsonUtils.getJson(new User("吴亦凡",31,"男"));
	}
	@ResponseBody
	@RequestMapping(value = "json3",produces = "application/json;charset=utf-8")
	public String json3() {
		return JsonUtils.getJson(new Date());//默认返回时间戳，如何不让他返回时间戳，需关闭时间戳功能
	}
	@ResponseBody
	@RequestMapping(value = "json4",produces = "application/json;charset=utf-8")
	public String json4() {
		return JsonUtils.getJson(new Date(),"yyyy-MM-dd");//默认返回时间戳，如何不让他返回时间戳，需关闭时间戳功能
	}
	
}
```

工具类：

```java

public class JsonUtils {

	/**
	 * json工具类
	 * @param obj
	 * @return
	 */
	
	public static String getJson(Object obj) {
		//需要一个jackson的对象映射器，就是一个类，使用它可以直接将对象转换为json字符串
		ObjectMapper mapper = new ObjectMapper();
		try {
			//将java对象转换为json字符串
			String string = mapper.writeValueAsString(obj);
			//关闭时间戳
			mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
			mapper.setDateFormat(sdf);
			return string;
		} catch (JsonProcessingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return "";
	}
	
	public static String getJson(Object obj,String dateFormatStr) {
		//需要一个jackson的对象映射器，就是一个类，使用它可以直接将对象转换为json字符串
		ObjectMapper mapper = new ObjectMapper();
		mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
		SimpleDateFormat sdf = new SimpleDateFormat(dateFormatStr);
		mapper.setDateFormat(sdf);
		try {
			//将java对象转换为json字符串
			String string = mapper.writeValueAsString(obj);
			//关闭时间戳
			return string;
		} catch (JsonProcessingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return "";
	}

}
```

### 小结：

后端接收前端数据，若是需要将前端传递的JSON字符串转换为后端定义的JavaBean，需要在方法参数前加上@RequestBody注解进行标志

同样的，@RequestParam注解，用法同@RequestBody，可以将前端传回的路径中带有的参数绑定到后端方法定义的参数之前

后端向前端返回JSON数据，需要一个jackson的对象映射器，就是一个类，使用它可以直接将对象转换为json字符串，具体用法是：

ObjectMapper mapper = new ObjectMapper();

 mapper.writeValueAsString(obj);

