# vue中v-model和v-bind区别

## Vue中的数据绑定

绑定数据有三种方式：

插值，也就是{{name}}的形式，以文本的形式和实例data中对应的属性进行绑定

v-bind：

v-model：

## v-bind

eg：v-bind:class 可简写为 :class

当加上v-bind:之后，它的值classe不是字符串，而是vue实例对应的data.classed的这个变量。也就是说data.classed是什么值，它就会给class属性传递什么值，当data.classed发生变化的时候，class属性也发生变化，这非常适合用在通过css来实现动画效果的场合。他只是单向变动

## v-bind支持的类型

html中的属性、css的样式、对象、数组、number 类型、bool类型

v-bind使用：
// 绑定文本

```html
// 绑定文本
<p v-bind="message"></p>
 
// 绑定属性
<p v-bind:src="http://...."></p>
<p v-bind:class="http://...."></p>
<p v-bind:style="http://...."></p>
 
// 绑定表达式
:class{className:true}
```



## v-model

主要是用在表单元素中，它实现了双向绑定。在同事使用v-bind和v-model中，v-model建立的双向绑定对输入型元素input, textarea, select等具有优先权，会强制实行双向绑定。很多时候v-model使用在表单的<input>中实现双向绑定。

```html
<input v-model="something">
```

