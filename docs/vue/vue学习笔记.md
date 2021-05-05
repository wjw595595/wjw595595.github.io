# 学习笔记

## 环境搭建

<a name="jump" href="vue环境搭建">vue环境搭建</a>

<a name="jump" href="vue命令行安装脚手架">vue环境搭建-命令行模式</a>

## vue项目的目录结构

- src/main.js :是整个项目的入口文件

  ```js
  new Vue({
    router,
    render: h => h(App)
  }).$mount('#app')
  //new 一个vue实例，通过render函数把app跟组件渲染到页面，同时把路由router挂载在实例中
  ```

- App.vue :跟组件

  template：UI结构

  script：行为

  style：样式

- 路由：router/index.js  
  
- routes:设置路由规则：import 导入组件
  
- import路径提示插件：Path Intellisense  Vue VSCode Snippets：快捷命令  如 ：vbase-less

- element.js： 可以按需导入

```js
import {
    Button,  Message
} from 'element-ui'
//vue
Vue.use(Button)
//原型挂载  js中直接  this.$message 使用
Vue.prototype.$message = Message
```

  里面用到了阿里的第三方图标库： https://www.iconfont.cn/

## 登录和退出

### 技术点

- http是无状态的（同一个会话的连续两个请求互相不了解 ）http应用层面向对象协议：超文本传输协议

解决http无状态的方法

- cookie在客户端记录状态
- session在服务端记录状态
- token 方式维持状态（存在跨域问题用）

### 登录页面布局

####      用到的组件

- el-form：表单
- el-form-item：表单项
- el-imput：输入框
- el-button 组件
- 字体图标

1. 创建模板 Login.vue: 可以用vbase
2. 在 /route/index.js 中配置路由规则，渲染在app.vue根组件中

### 表单处理

表单属性

- model :数据绑定

- rules：表单验证

表单方法：

- resetFields：重置  this.$refs.loginFormRef.resetFields()
- validate：this.$refs.loginFormRef.validate(async valid => {}
- this.$router.push('/home')  页面跳转
- ref：通过ref获取到表单的实例对象

```html
<el-form :model="loginForm" :rules="rules" ref="loginFormRef"

data() {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ]
      }
    }
```



## 权限管理

### 权限列表

用到的组件

- 面包屑导航：el-breadcrumb

- 卡片容器：el-card

- table:el-table  边框：border ，隔行变色：stripe

  1. 获取每一行的数据： 用作用域插槽  <template slot-scope="scope">

     ```html
     #scope.row 获得行数据对象  slot-scope="scope" scope.row：行数据
             <el-table-column prop="level" label="权限层级">
               <template slot-scope="scope">
                 <el-tag v-if="scope.row.level === '0'" >标签一</el-tag>
                 <el-tag v-else-if="scope.row.level === '1'" type="success">标签二</el-tag>
                 <el-tag v-else type="warning">标签三</el-tag>
               </template>
             </el-table-column>
     ```

  2. v-if ,v-else-if,v-else标签使用
  
  3. 用到 el-tab 
  
- 异步请求： 返回是promise的可以进行同步优化：async和await

### 权限管理业务分析

用户绑定不同的角色，角色绑定不同的权限，用户和权限不关联

![用户](https://i.loli.net/2021/03/10/HoVma6PgXJezScx.png)

### 角色列表

效果

![image-20210310182339576](https://i.loli.net/2021/03/10/BtQFjZnAvw6odyf.png)

用到到组件

- 面包屑导航：el-breadcrumb

- 卡片容器：el-card

- able:el-table ：列扩展 属性

  ```html
  <el-table-column type="expand">
  ```

- v-for 循环 ：格栅系统布局加for循环

  ```html
  <el-row v-for="(item1, it1) in scope.row.children" :key="item1.id" :class="['bgbottom', it1 === 0 ? 'bgtop' : '']">
      <!-- 渲染一级权限 -->
      <el-col :span="5">
          <el-tag>{{ item1.authName }}</el-tag>
          <i class="el-icon-caret-right"></i>
      </el-col>
      <!-- 渲染二级权限 -->
      <el-col :span="19">
          <!-- 通过for循环 嵌套渲染二级权限  -->
          <el-row v-for="(item2, it2) in item1.children" :key="item2.id" :class="[it2 === 0 ? '' : 'bgtop']">
              <el-col :span="6">
                  <el-tag type="success">{{ item2.authName }}</el-tag>
                  <i class="el-icon-caret-right"></i>
              </el-col>
              <el-col :span="18">
                  <!-- 通过for循环 嵌套渲染三级权限  -->
                  <el-tag type="warning" v-for="(item3, it3) in item2.children" :key="item3.id">{{item3.authName}}</el-tag>
              </el-col>
          </el-row>
      </el-col>
  </el-row>
  <style lang="less" scoped>
  .bgbottom {
      <!-- 下划线  border-top-->
    border-bottom: 1px solid #eee;
  }
  .bgtop {
    border-top: 1px solid #eee;
  }
  </style>
  ```

  1. 三目运算符
  2. 嵌套循环

#### tag:标签删除

- closable

- 关闭函数 close

  关闭确认窗口MessageBox

  

## 商品分类

属性结构插件 ：vue-table-with-tree-grid

https://github.com/MisterTaki/vue-table-with-tree-grid