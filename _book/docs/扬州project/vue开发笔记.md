## table-的expend 手风琴效果

https://blog.csdn.net/sinat_33312523/article/details/78928236

## 父子组件传值

```html
#父组件
<template>
    <div>
        <children-components :options="dcamera_option"></children-components>
    </div>
</template>

<script>
export default {
    name: 'index',
    components: {
        'children-components': childrenComponents
    },
    data(){
         dcamera_option: { input_value: 6 }
    }
}
</script>
```

```html
#子组件
<template>
    <div>
        <input type="text" v-model="options.input_value"/>
    </div>
</template>

<script>
export default {
    name: 'ChildrenComponents',
    props: {
        options: { type: Object, required: true },
    }
}
</script>
```

## vue中created、mounted、activated的区别

created：在模板渲染成html之前调用，即通常初始化某些属性值，然后再渲染成视图；但是注意，只会触发一次

mounted：在渲染成html之后调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作。是挂载vue实例后的钩子函数，钩子在主页挂载时执行一次，如果没有缓存的话，再次回到主页时，此函数还会执行。

activated：是组件被激活后的钩子函数，每次回到页面都会执行
执行顺序：`created => mounted => activated`



this.$message.error(data.msg)

## echart和v-chart相关

官方文档：https://v-charts.js.org/#/

百度：https://echarts.apache.org/zh/option.html#title

npm i v-charts echarts -S

报错： --save echarts/lib/visual/dataColor

解决： npm i v-charts echarts@4.9.0 -S

如不行用下面

**npm install v-echarts echarts --save**



Vue项目中，Cannot find module 'node-sass' 报错找不到解决

cnpm install node-sass@latest -S



问题：

vue2引入v-charts为什么需要f12才会显示

解决：组件加入宽度和高度

<ve-line :data="chartData" :settings="chartSettings" width="800px" height="400px"></ve-line> 

父子加载顺序

https://blog.csdn.net/weixin_43878906/article/details/108274025



折线图例子：

https://www.oschina.net/p/v-charts

https://www.cnblogs.com/cina33blogs/p/10750169.html

参考例子 vchart ：https://blog.csdn.net/weixin_44824839/article/details/103140933

拓展参考：

https://echarts.apache.org/zh/option.html#title

y轴固定内容滚动

https://blog.csdn.net/qq_42714690/article/details/103655087



## 时区转换

https://blog.csdn.net/sinat_32849897/article/details/110848673

## vue中的数据格式化filters、formatter

https://blog.csdn.net/messicr7/article/details/105435314/?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-1&spm=1001.2101.3001.4242

```
{{ message | filterA | filterB }}
```

这个 写法的功能是，把message 传递给filterA 返回值传递给filterB最后返回显示结果到页面上，可以看出是按函数的先后顺序调用下去最后返回结果显示。

```
<!-- 在双花括号中 -->
{{ mobile | formatmobile}}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="bytesize | formatbytes"></div>
```

跨域解决：

https://blog.csdn.net/lindali1115/article/details/108096631

json 格式的问题

## js保留两位小数

https://www.jb51.net/article/134067.htm



## 页面不跳转：，vue-springboot整包部署

在controller层应该用@Controller注解，不能用@ResController注解，如果使用@ResController注解，此注解是将json转换成字符串,那么就无法转到html页面，只会返回字符串

或者用

1. 
   @RestController
2. @RequestMapping("rmsLogin")  多加个RequestMapping

整包部署：

```
dist包放在了
/static/static/目录下

shiro配置中加入 filterMap.put("/static/**", "anon");
访问地址
http://localhost:8080/idc-services/static/index.html

application.properties 中加入
spring.web.resource.static-locations=classpath:/
spring.mvc.view.suffix=.html

SysLoginController添加
@RequestMapping("")
```

```
启动命令
$nohup java -jar idc-services-1.0.0.jar  > log.file  2>&1 &
```

## vue自定义全局方法

https://www.cnblogs.com/conglvse/p/10062449.html

全局过滤器

https://blog.csdn.net/weixin_33749131/article/details/93396673

## js保留两位小数

https://www.jb51.net/article/134067.htm

## docker启动错误

ERROR: for prometheus  Cannot restart container 950af762c9501a7ee07db430d92da45a8a515980bace5a04192496f6173b1369: driver failed programming external connectivity on endpoint prometheus (ffec7c8009254b20c7605546ed2b03be313596b2f9fd81108bf1b3e3d40951e0):  (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 9090 -j DNAT --to-destination 172.17.0.8:9090 ! -i br-e69a795d182b: iptables: No chain/target/match by that name.

解决：https://blog.csdn.net/u013948858/article/details/83115388

https://blog.51cto.com/17099933344/1929664

极端解决：

```

systemctl stop firewalld
 
systemctl stop iptables
```



泛型接口实例化

https://blog.csdn.net/weixin_34075551/article/details/86132595

https://blog.csdn.net/aiyaya_/article/details/79212852

## 仪表盘

https://www.cnblogs.com/pqblog/p/8478865.html

![image-20210428161511497](https://i.loli.net/2021/04/28/AmXOu47aqCJlr8h.png)

　https://antv.alipay.com/  图标工具

https://www.bilibili.com/read/cv6183470/   Gauge和Bar Gauge使用



**http://echarts.baidu.com/**

**教程：http://www.cnblogs.com/jerehedu/p/4538459.html**

prometheus 函数https://www.cnblogs.com/wayne-liu/p/9273492.html