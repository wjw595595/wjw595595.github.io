# gitbook安装

## 安装

### 环境要求

nodejs（v10.21.0）

gitbook-cli 2.3.2

gitbook:3.2.3

### 通过npm安装

```cmd
npm install gitbook-cli -g
#gitbook-cli是gitbook的一个命令行工具, 通过它可以在电脑上安装和管理gitbook的多个版本
gitbook -V
#等待安装完成
#可以配置淘宝镜像
#npm config set registry https://registry.npm.taobao.org
#npm config get registry
#预览书籍 http://localhost:4000/

```



## 创建gitbook仓库

### 目录结构

http://gitbook.zhangjikai.com/structure.html

```
.
├── book.json
├── README.md
├── SUMMARY.md
├── chapter-1/
|   ├── README.md
|   └── something.md
└── chapter-2/
    ├── README.md
    └── something.md

```

.gitignore
SUMMARY.md（撰写左边导航栏）
README.md（简介）
book.json（对gitbook基本的配置，有的时候你一个这样的仓库，可能没有该文件，可自行创建）

### 命令 

 [命令网址](http://gitbook.zhangjikai.com/commands.html) 

```cmd
gitbook init
#执行完后，文件夹里会多两个文件
#README.md（书籍的介绍在这个文件里）
#SUMMARY.md（书籍的目录结构在这里配置）
#生成静态网页并运行服务器 _book目录 localhost:4000
gitbook serve 

```

### 配置（book.json）

[配置文档](http://gitbook.zhangjikai.com/settings.html)

```json
{
   
  "title": "David的笔记",
  "description": "David的笔记,java开发的参考手册",
  "author": "david wang",
  "language": "zh-hans",
  "gitbook": "3.2.3",
  "fontState": {
    "size": "2",
    "family": "sans",
    "theme": "night"
  },
  "links": {
    "sidebar": {
         /* 主页地址*/
      "home": "https://wjw595595.github.io/note/"
    }
  },
  "theme-default": {
		"showLevel": true
	},
  "plugins": [
      /*自动生成菜单 */
      "summary",
      /*侧边栏可移动 */
    "splitter",
      /* -:是卸载，卸载原始英文搜索，换中文 */
    "-lunr", 
	"-search", 
	"search-pro",
    /* 菜单折叠 */
    "expandable-chapters",
      /* 样式 */
      "theme-comscore"
  ],
  "pluginsConfig": {
    "search-pro": {
      "cutWordLib": "nodejieba",
      "defineWord": [
        "搜索"
      ]
    }
  }
}

{
	"title" : "文档库",
	"theme-default": {
		"showLevel": true
	},
	"plugins": ["summary","expandable-chapters", "theme-comscore"]
}
```



## 常用插件

[插件文档](http://gitbook.zhangjikai.com/plugins.html)

插件用 npm安装

```cmd
#中文搜索
npm install gitbook-plugin-search-pro --registry=https://registry.npm.taobao.org/
#自动生成菜单
npm install gitbook-plugin-summary 
# 菜单折叠
npm install gitbook-plugin-expandable-chapters
#主题插件
npm  install gitbook-plugin-theme-comscore
#splitter 侧边栏可调节
npm install gitbook-plugin-splitter
#其他插件 http://gitbook.zhangjikai.com/plugins.html
```

## 参考文档：

http://gitbook.zhangjikai.com/

插件推荐

https://segmentfault.com/a/1190000019806829?utm_source=tag-newest



## 和github整合

- github创建项目  note

- 克隆本地 ，在note中创建 gitbook 目录

- 在note目录 添加配置 book.json

- 在gitbook中添加笔记

- 在note目录运行 npm安装相关插件

- 运行  note目录运行 gitbook serve

  - 提交代码到github  

    ```cmd
     git add .
     git commit -m 'init'
     git push -u origin master    
     git subtree push --prefix=_book origin gh-pages
     #https://<你的用户名称>.github.io/<你的项目名称>/
     #如：https://wjw595595.github.io/note/
    ```

    

