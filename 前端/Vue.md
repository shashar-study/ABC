# 前置工作

> IDEA里Vue插件的安装

File-->Settings-->Plugins 

搜索Vue安装，随后重启，即可

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201228163400351.png" alt="image-20201228163400351" style="zoom: 33%;" />

重启之后，点击new会有如下情况

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201228163722241.png" alt="image-20201228163722241" style="zoom:33%;" />

如果安装结束之后，new中没有Vue Component ，也没关系，要进行如下操作

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201228163929387.png" alt="image-20201228163929387" style="zoom:33%;" />

找到Vue Single File Component ，点击上面的复制

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20201228164037269.png" alt="image-20201228164037269" style="zoom:33%;" />

将Name改为Vue Component  ，以及name那里去掉Component_,改完之后如图，点击 OK，即可。

# 基础语法

## 787

> 七个属性

- ### **el属性**

  - 用来指示vue编译器从什么地方开始解析 vue的语法，可以说是一个占位符。

- ### **data属性**

  - 用来组织从view中抽象出来的属性，可以说将视图的数据抽象出来存放在data中。

- ### template属性

  - 用来设置模板，会替换页面元素，包括占位符。

- ### methods属性

  - 放置页面中的业务逻辑，js方法一般都放置在methods中

- ### render属性

  - 创建真正的Virtual Dom

- ### computed属性

  - 用来计算

- ### watch属性

  - watch:function(new,old){}
  - 监听data中数据的变化
  - 两个参数，一个返回新值，一个返回旧值，



# 结合ElementUI 的整体步骤

管理员运行cmd

```bash
# 1. 创建项目名为hello-vue的webpack项目，一路选择No
vue init webpack hello-vue
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210106150723246.png" alt="image-20210106150723246" style="zoom: 33%;" />

```bash
cd vue-hello
#2. 安装vue-router
npm install vue-router --save-dev
# 3.安装element-UI
npm i element-ui -S
#4. 安装依赖
npm install
#5. 安装SASS 加载器sass-loader、node-sass
cnpm install sass-loader node-sass --save-dev
#6. 启动测试
npm run dev
```

一系列步骤之后，cmd会提示

![image-20210106152057785](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210106152057785.png)

打开这个网址 

![image-20210106152124940](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210106152124940.png)

> 一些简单的npm命令
>

NPM的全称是Node Package Manager，是随同NodeJS一起安装的包管理和分发工具，它很方便让JavaScript开发者下载、安装、上传以及管理已经安装的包。

```bash
#安装模块到项目目录下
npm install moduleName
#-g指将模块安装到全局，具体安装位置为npm config prefix 的位置
npm install -g moduleName
#--save 指将模块安装到项目目录下，并在package文件的dependencies节点写入依赖，可以用简写-S代替
npm install --save moduleName
#--save-dev指模块安装到项目目录下，并在package文件的devDependencies节点写入依赖，可以用简写-D代替
npm install --save-dev moduleName
```





Ctrl +C 终止这个项目，使用IDEA 打开，删除不需要的部分，也就是Hello.vue以及在APP.vue的引用部分

创建目录结构，完成之后的目录结构如下:

![image-20210107154529700](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210107154529700.png)

- assets：用于存放资源文件
- components：用于存放Vue功能组件
- views：用于存放Vue视图组件
- router：用于存放vue-router配置(类似于Controller)

在views中创建Main.vue

```html
<template>
	<div>首页</div>
</template>
<script>
	export default {
			name:"Main"
	}
</script>
<style scoped>
</style>
```

在views目录下创建名为Login.vue的视图组件，其中el-*的元素为ElementUI组件；

```html
<template>
  <div>
    <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
      <h3 class="login-title">欢迎登录</h3>
      <el-form-item label="账号" prop="username">
        <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-on:click="onSubmit('loginForm')">登录</el-button>
      </el-form-item>
    </el-form>

    <el-dialog
      title="温馨提示"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose">
      <span>请输入账号和密码</span>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: "Login",
    data() {
      return {
        form: {
          username: '',
          password: ''
        },

        // 表单验证，需要在 el-form-item 元素中增加 prop 属性
        rules: {
          username: [
            {required: true, message: '账号不可为空', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '密码不可为空', trigger: 'blur'}
          ]
        },

        // 对话框显示和隐藏
        dialogVisible: false
      }
    },
    methods: {
      onSubmit(formName) {
        // 为表单绑定验证功能
        this.$refs[formName].validate((valid) => {
          if (valid) {
            // 使用 vue-router 路由到指定页面，该方式称之为编程式导航
            this.$router.push("/main");
          } else {
            this.dialogVisible = true;
            return false;
          }
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  .login-box {
    border: 1px solid #DCDFE6;
    width: 350px;
    margin: 180px auto;
    padding: 35px 35px 15px 35px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    box-shadow: 0 0 25px #909399;
  }

  .login-title {
    text-align: center;
    margin: 0 auto 40px auto;
    color: #303133;
  }
</style>
```

在router目录下创建一个名为`index.js`的vue-router路由配置文件

```javascript
//导入vue
import Vue from 'vue';
import VueRouter from 'vue-router';
//导入组件
import Main from "../views/Main";
import Login from "../views/Login";
//使用
Vue.use(VueRouter);
//导出
export default new VueRouter({
  routes: [
    {
      //登录页
      path: '/main',
      component: Main
    },
    //首页
    {
      path: '/login',
      component: Login
    },
  ]


})
```

App.vue

```html
<template>
  <div id="app">
  	<router-link to="/login">登录</router-view>
    <router-view></router-view>
  </div>
</template>

<script>

export default {
  name: 'App',

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```



main.js

```javascript
import Vue from 'vue'
import App from './App'
import VueRouter from "vue-router";
//扫描路由配置
import router from "./router"
//导入elementUI
import ElementUI from "element-ui"
//导入element css
import 'element-ui/lib/theme-chalk/index.css'
//使用
Vue.use(VueRouter)
Vue.use(ElementUI)
Vue.config.productionTip = false
new Vue({
  el: '#app',
  router,
  render: h => h(App),//ElementUI规定这样使用
})
```

然后npm run dev 测试运行

我这边初始运行报错 ，是因为Sass-loader版本过高的原因，在package.json 中，将devDependencies中的sass-loader改为4.1.0之后可以成功运行

