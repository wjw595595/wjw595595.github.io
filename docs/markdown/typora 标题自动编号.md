[toc]

# typora markdown 标题自动编号

1. 用 .css 文件实现 markdown 文件的标题自动标号。
2. 在 Typora 界面中，点 File -> Preference... -> Open Theme Folder 会打开一个文件夹（如：C:\Users\iTom\AppData\Roaming\Typora\themes），在这里新建一个叫 base.user.css 的文件，内容见下，然后重启 Typora 即可。

## 序号从一级标题开始的

base.user.css

```css
/** initialize css counter */
#write {
    counter-reset: h1
}

h1 {
    counter-reset: h2
}

h2 {
    counter-reset: h3
}

h3 {
    counter-reset: h4
}

h4 {
    counter-reset: h5
}

h5 {
    counter-reset: h6
}

/** put counter result into headings */
#write h1:before {
    counter-increment: h1;
    content: counter(h1) ". "
}

#write h2:before {
    counter-increment: h2;
    content: counter(h1) "." counter(h2) ". "
}

#write h3:before,
h3.md-focus.md-heading:before /** override the default style for focused headings */ {
    counter-increment: h3;
    content: counter(h1) "." counter(h2) "." counter(h3) ". "
}

#write h4:before,
h4.md-focus.md-heading:before {
    counter-increment: h4;
    content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) ". "
}

#write h5:before,
h5.md-focus.md-heading:before {
    counter-increment: h5;
    content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) ". "
}

#write h6:before,
h6.md-focus.md-heading:before {
    counter-increment: h6;
    content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) "." counter(h6) ". "
}

/** override the default style for focused headings */
#write>h3.md-focus:before,
#write>h4.md-focus:before,
#write>h5.md-focus:before,
#write>h6.md-focus:before,
h3.md-focus:before,
h4.md-focus:before,
h5.md-focus:before,
h6.md-focus:before {
    color: inherit;
    border: inherit;
    border-radius: inherit;
    position: inherit;
    left:initial;
    float: none;
    top:initial;
    font-size: inherit;
    padding-left: inherit;
    padding-right: inherit;
    vertical-align: inherit;
    font-weight: inherit;
    line-height: inherit;
}
```

## 序号从二级标题开始的

```css
/** initialize css counter */
#write,.md-toc-content,.sidebar-content {
    counter-reset: h2
}

h1 , .outline-h1, .md-toc-item.md-toc-h1 {
    counter-reset: h2
}

h2 ,.outline-h2, .md-toc-item.md-toc-h2{
    counter-reset: h3
}

h3 , .outline-h3, .md-toc-item.md-toc-h3{
    counter-reset: h4
}

h4 , .outline-h4, .md-toc-item.md-toc-h4{
    counter-reset: h5
}

h5 , .outline-h5, .md-toc-item.md-toc-h5{
    counter-reset: h6
}

/** put counter result into headings */
#write h1:before ,
.outline-h1>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h1>.md-toc-inner:before {
    counter-increment: h1;
    content: counter(h1) ". "
}
/* 使用h1标题时，去掉前两个h1标题的序号，包括正文标题、目录树和大纲 */
/* nth-of-type中的数字表示获取第几个h1元素，请根据情况自行修改。 */
#write h1:nth-of-type(1):before,
.outline-h1:nth-of-type(1)>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h1:nth-of-type(1)>.md-toc-inner:before,
#write h1:nth-of-type(2):before,
.outline-h1:nth-of-type(2)>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h1:nth-of-type(2)>.md-toc-inner:before{
	counter-reset: h1;
	content: ""
}

#write h2:before ,
.outline-h2>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h2>.md-toc-inner:before {
    counter-increment: h2;
    content:  counter(h2) ". "
}

#write h3:before,
h3.md-focus.md-heading:before ,/** override the default style for focused headings */ 
.outline-h3>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h3>.md-toc-inner:before {
    counter-increment: h3;
    content: counter(h2) "." counter(h3) ". "
}

#write h4:before,
h4.md-focus.md-heading:before,
.outline-h4>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h4>.md-toc-inner:before  {
    counter-increment: h4;
    content: counter(h2) "." counter(h3) "." counter(h4) ". "
}

#write h5:before,
h5.md-focus.md-heading:before ,
.outline-h5>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h5>.md-toc-inner:before {
    counter-increment: h5;
    content: counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) ". "
}

#write h6:before,
h6.md-focus.md-heading:before ,
.outline-h6>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h6>.md-toc-inner:before {
    counter-increment: h6;
    content:  counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) "." counter(h6) ". "
}

/** override the default style for focused headings */
#write>h3.md-focus:before,
#write>h4.md-focus:before,
#write>h5.md-focus:before,
#write>h6.md-focus:before,
h3.md-focus:before,
h4.md-focus:before,
h5.md-focus:before,
h6.md-focus:before {
    color: inherit;
    border: inherit;
    border-radius: inherit;
    position: inherit;
    left:initial;
    float: none;
    top:initial;
    font-size: inherit;
    padding-left: inherit;
    padding-right: inherit;
    vertical-align: inherit;
    font-weight: inherit;
    line-height: inherit;
}
```

