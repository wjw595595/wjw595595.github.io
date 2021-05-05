# VSCode 开发Vue必备插件

　　对于很多使用vscode编写vue项目的新手同学来说，可能不知道使用什么插件，这里简单说一下我常用的几款插件。

## vetur

vetur能够实现在 .vue 文件中：

语法错误检查，包括 CSS/SCSS/LESS/Javascript/TypeScript
语法高亮，包括 html/jade/pug css/sass/scss/less/stylus js/ts emmet 支持
代码自动补全（目前还是初级阶段），包括 HTML/CSS/SCSS/LESS/JavaScript/TypeScript 配合 ESLint 插件使用效果更佳

设置标签不自动换行

设置--扩展--vetur

![image-20210311114259693](https://i.loli.net/2021/03/11/JXdbGSCeHlzfpFn.png)

或右上角   打开设置json，

```json
    "vetur.format.defaultFormatter.html": "js-beautify-html",//html不换行
    "vetur.format.defaultFormatterOptions": {

        "js-beautify-html": {
            //html 标签属性 换行设置[auto|force|force-aligned|force-expand-multiline] ["auto"]
            "wrap_attributes": "auto" ,  
            // 设置多个字符后换行 0 表示忽略
            "wrap_line_length": 120,
            // 在文件结尾添加新行
            "end_with_newline": false 
        },
        "prettier": {
            "singleQuote": true,
            "semi": false, //不加分号
            "tabWidth": 4
        },
        "prettyhtml": {
            "printWidth": 100,
            "singleQuote": false,
            "wrapAttributes": false,
            "sortAttributes": false
        }
    },
    // js代码不自动换行
    "vetur.format.defaultFormatter.js": "vscode-typescript",
```

注意：会与prettier冲突，右键格式化的文件，选择格式文档的方式 --选择vetur为默认的

## eslint

eslint插件能够检测代码语法问题，与格式问题，对项目代码风格统一至关重要。
三.EditorConfig for Visual Studio Code
EditorConfig 是一种被各种编辑器广泛支持的配置，使用此配置有助于项目在整个团队中保持一致的代码风格。
四.Path Intellisense
在编辑器中输入路径时，自动补全。

## View In Browser

在浏览器中查看静态文件。

## 六.Live Server

　　这个插件很有用，安装之后可以打开一个简单的服务器。而且还会自动更新。安装之后，打开项目文件夹，再在文件上点击右键就会出现一个Open with Live Server的选项，

就会自动打开浏览器了。默认端口号是5500

## 七.GitLens----- Git Supercharged(必备)

查看git文件提交历史。

## 八.Document This

注释文档生成

## 九.Debugger for Chrome

直接在vscode里面进行调试js文件，跟谷歌的控制台是一样的功能，下载了它就不用打开浏览器的控制台就能进行打断点。
需要配置vscode的lauch.json的谷歌调试相关配置
十.HTML CSS Support
在编写样式表的时候，自动补全功能大大缩减了编写时间，推荐！

## 十一.JavaScript Snippet Pack

针对js的插件，包含了js的常用语法关键字，很实用。

## 十二.HTML Snippets

这款插件包含html标签，非常全，很实用。

## 十三.One Monokai Theme

能够选择自己喜欢的颜色主题，来编写代码，比较喜欢用的是monokai。

## 十四.vscode-icons(很好用)

能够选择自己喜欢的图标主题，比较推荐vscode icons

## Vue VSCode Snippets

快捷生成代码： vbase-less

![image-20210311152804727](https://i.loli.net/2021/03/11/wEUcRPzyt1Do7g2.png)

![image-20210311152836371](https://i.loli.net/2021/03/11/GijcNStKT6vrJhA.png)

![image-20210311152903077](https://i.loli.net/2021/03/11/WjaZskoCtTS5I2B.png)

## Path Intellisense

vue import路径提示

## vue-helper

自定义组件跳转 ctrl

## VS Color Picker

取色

CSS Peek ：css跳转

SVG Viewer：右键可以看SVG



Icon Fonts：图库支持



## element-ui 快捷提示

vue-helper（shenjiaolong）  或Element UI Snippets

[vscode-element-helper](https://github.com/ElemeFE/vscode-element-helper)