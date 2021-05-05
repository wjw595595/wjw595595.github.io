# vscode使用教程

## 命令面板

命令面板：ctrl+shift+p  命令快捷键

调试： ctrl+shift+y

命令面板中可以搜索命令，然后执行

## 界面介绍

![img](https://i.loli.net/2020/12/11/YcRim5PfBv62Qe4.png)



右侧设置 多个标签页

https://www.pianshen.com/article/8212968259/

工作区放大缩小

https://www.jianshu.com/p/530df9a13fca



## 添加注释

可以看到如果想给已有的代码直接添加行注释 ctrl+k ctrl+c，取消是ctrl+k ctrl+/
如果是想在新的一行中开始一段新的行注释（toggle line comment）是ctrl+/
添加块注释shif+alt+a

## vscode vue 格式化

1.安装 vetur

2.在User Setting中增加设置:

"vetur.format.defaultFormatter.html": "js-beautify-html"

![image-20210305035729715](https://i.loli.net/2021/03/05/UIq9Mn2jy71rvkp.png)

3.搞定

格式化快捷键：Alt+Shift+F

## 鼠标滚动改变字体大小

设置中setting中

![image-20210416232957863](https://i.loli.net/2021/04/16/lg57JTFsUwNcpZ1.png)

ctrl+鼠标滚轮

## 关闭缩略图

设置里面搜索 "editor.minimap.enabled"，设置为false即可。