**GitBook + Typora + Git**

https://blog.csdn.net/qq_43528771/article/details/107949010

注意：

安装node 版本（目前测试就这个版本 不然gitbook 卡死）

v10.21.0

官网：

https://www.npmjs.com/package/gitbook-cli/v/2.3.2

先安装：要安装3.0.0 的  3.2.3初始化有问题

## 安装

```undefined
npm install -g gitbook-cli
gitbook -V
#等待安装完成
npm config set registry "https://registry.npm.taobao.org"
```

## 安装Calibre（导出pdf）

https://calibre-ebook.com/download_windows

测试导出pdf插件的命令行

ebook-convert --version


使用方法
在当前书籍的目录中执行

gitbook pdf

也可以指定目录和文件名

gitbook pdf ./ ./myBook.pdf --log=debug


## 创建笔记文件夹

在你想要的位置新建一个文件夹，然后打开命令行，cd到这个文件夹下。

```kotlin
gitbook init
```

执行完后，文件夹里会多两个文件

README.md（书籍的介绍在这个文件里）

SUMMARY.md（书籍的目录结构在这里配置）

## 生成目录插件

**[gitbook-plugin-summary](https://link.jianshu.com/?t=https://github.com/julianxhokaxhiu/gitbook-plugin-summary)**

```shell
#安装 
npm i gitbook-plugin-summary --save
#book.json 笔记根目录

{
	"title" : "文档库",
	"theme-default": {
		"showLevel": true
	},
	"plugins": ["summary","toggle-chapters", "theme-comscore"]
}
#说明  
#summary: 自动生成SUMMARY.md文件
#toggle-chapters: 菜单可以折叠
#theme-comscore: 主题插件, 修改标题和表格颜色
gitbook install
gitbook build 
#执行
gitbook serve
```

参考：https://www.jianshu.com/p/2160f1ba68a0?utm_campaign

https://www.npmjs.com/package/gitbook-plugin-toggle-chapters

gitbook-plugin-theme-comscore

```
#中文搜索
npm install gitbook-plugin-search-pro --registry=https://registry.npm.taobao.org/
#自动生成菜单
npm i gitbook-plugin-summary --save
# expandable-chapters-small
npm install gitbook-plugin-expandable-chapters
#主题插件
npm -i install gitbook-plugin-theme-comscore
#splitter 侧边栏可调节
npm install gitbook-plugin-splitter
#其他插件 http://gitbook.zhangjikai.com/plugins.html
```

踩坑：

运行 gitbook -V或 gitbook init 时一直卡在 Installing GitBook 3.2.3

一、换node 版本  v10.21.0

或： git bash或者win10的terminal来代替CMD



## 使用

gitbook init

#book.json 笔记根目录

{
	"title" : "文档库",
	"theme-default": {
		"showLevel": true
	},
	"plugins": ["summary","toggle-chapters", "theme-comscore"]
}

gitbook serve

![img](https://i.loli.net/2021/04/18/Q9F1ju2UCidGWXV.jpg)

![img](https://i.loli.net/2021/04/18/EwClLZp7gnq8ToA.jpg)

```
 git add .
 git commit -m 'init'
 git push -u origin master    
 git subtree push --prefix=_book origin gh-pages
 
 #https://wjw595595.github.io/note/
```

https://baijiahao.baidu.com/s?id=1666708486591555944&wfr=spider&for=pc

https://wjw595595.github.io/e-book/

常用插件：

https://segmentfault.com/a/1190000019806829?utm_source=tag-newest

官方文档

http://gitbook.zhangjikai.com/plugins.html