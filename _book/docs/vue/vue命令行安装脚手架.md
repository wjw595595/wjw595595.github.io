# 命令行安装

## 概念

1. **npm**: Nodejs下的包管理器。

2. **webpack**: 它主要的用途是通过CommonJS的语法，把所有浏览器端需要发布的静态资源，做相应的准备，比如资源的合并和打包。

3. **vue-cli**:  用户生成Vue工程模板。（帮你快速开始一个vue的项目，也就是给你一套vue的结构，包含基础的依赖库，只需要 npm install就可以安装）

## npm安装

[nodejs官网](https://nodejs.org/en/)

## npm常用命令

```cmd
echo  %PATH% 
node -v
npm -v
#修改npm本地仓库地址
npm config set prefix  "D:\Program Files\nodejs\node_global"
npm config set cache  "D:\Program Files\nodejs\node_cache"
npm list --global

```

## npm安装插件

1. 配置淘宝镜像

   ```cmd
   npm config set registry=https://registry.npm.taobao.org
   npm config list
   
   #安装更新
   npm install [插件名称] -g
   #-g:是全局
   ```

## 配置vue

### 配置node和node_path

- 修改path: D:\Program Files\nodejs\node_global;
- b、新增NODE_PATH：D:\Program Files\nodejs\node_global\node_modules

### 配置vue和vue-router

```cmd
npm install vue -g
npm install vue-router -g
```

### 安装vue脚手架vue-cli

```cmd
npm install vue-cli –g
#验证
vue  -V
```

## 创建vue项目

找到存放项目的空目录

```cmd
#初始化项目
vue init webpack vueproject
#进入项目
cd vueproject
#安装项目依赖
npm install 
#运行项目
npm run dev
#生成静态文件
npm run build
#(生成静态文件，打开dist文件夹下新生成的index.html文件)
```

## 项目目录描述

![img](https://i.loli.net/2021/03/11/5Acro6EMsPZO3Cq.png)