# 网址生成exe文件流程

## 第一步：安装nativefier (基于electron)

1、安装node  

2、安装 nativefile

```
npm install nativefier -g   --registry=https://registry.npm.taobao.org
```

3、生成exe文件

```
nativefier --name "YouForever"  "http://www.baidu.cn"
或
nativefier -n "yunpan" -p "windows" -a "ia32" -i "e:\\yunpan\favicon.ico" "http://10.1.5.98" --file-download-options "{\"saveAs\": true}"
nativefier -n "yunpan" -p "windows" -a "x64" -i "e:\\yunpan\favicon.ico" "http://10.1.5.98" --file-download-options "{\"saveAs\": true}"


注解：第一次时间有点长，耐心等待
```
--file-download-options "{\"saveAs\": true}" 是保存文件的
说明：1、路径默认放在 cd路径下

2、nativefier参数配置：https://github.com/nativefier/nativefier/blob/HEAD/docs/api.md

3、logo必须用.ico格式   转换工具：http://www.ico8.net/

## 第二步：exe生成  exe可安装文件

工具：Inno Setup Compiler  默认安装没有中文

url：http://www.jrsoftware.org/isdl.php

中文语言包：https://gitee.com/jiuyong/Lunar-Markdown-Editor/blob/master/ChineseSimplified.isl

放到安装路径语言包中

或：https://jrsoftware.org/files/istrans/

1、使用操作流程： file

[![exe生成一键式安装版exe（中文安装，安装后exe图标自定义）_第2张图片](https://img.it610.com/image/info8/e762369039ee480495ccc516901d91de.jpg)](https://img.it610.com/image/info8/e762369039ee480495ccc516901d91de.jpg)

[![exe生成一键式安装版exe（中文安装，安装后exe图标自定义）_第3张图片](https://img.it610.com/image/info8/c5de01ca55534a25a49b2a487a971cd5.png)](https://img.it610.com/image/info8/c5de01ca55534a25a49b2a487a971cd5.png)

[![exe生成一键式安装版exe（中文安装，安装后exe图标自定义）_第4张图片](https://img.it610.com/image/info8/eaed7219d5aa41c8acca6a60c85a6284.png)](https://img.it610.com/image/info8/eaed7219d5aa41c8acca6a60c85a6284.png)

[![exe生成一键式安装版exe（中文安装，安装后exe图标自定义）_第5张图片](https://img.it610.com/image/info8/08a531c276184055ad5f01566e1f8c5d.jpg)](https://img.it610.com/image/info8/08a531c276184055ad5f01566e1f8c5d.jpg)

选择所需的exe程序，注意添加全部的依赖文件

[![exe生成一键式安装版exe（中文安装，安装后exe图标自定义）_第6张图片](https://img.it610.com/image/info8/595abd314e94492989305e75d03989a1.jpg)](https://img.it610.com/image/info8/595abd314e94492989305e75d03989a1.jpg)

[![exe生成一键式安装版exe（中文安装，安装后exe图标自定义）_第7张图片](https://img.it610.com/image/info8/935ca35547fc4ff883d64b400670996d.png)](https://img.it610.com/image/info8/935ca35547fc4ff883d64b400670996d.png)

[![exe生成一键式安装版exe（中文安装，安装后exe图标自定义）_第8张图片](https://img.it610.com/image/info8/c15a45b5ba1a40c088b8ea6cc4ba7382.png)](https://img.it610.com/image/info8/c15a45b5ba1a40c088b8ea6cc4ba7382.png)

[![exe生成一键式安装版exe（中文安装，安装后exe图标自定义）_第9张图片](https://img.it610.com/image/info8/5ad4bcc2967b493e83f2ed07079592dd.png)](https://img.it610.com/image/info8/5ad4bcc2967b493e83f2ed07079592dd.png)



[![exe生成一键式安装版exe（中文安装，安装后exe图标自定义）_第10张图片](https://img.it610.com/image/info8/e70c686c6aaf4dd4a43d7312b9e7f728.png)](https://img.it610.com/image/info8/e70c686c6aaf4dd4a43d7312b9e7f728.png)



然后直接finish，这就会在桌面（你选择的生成位置）生成安装的XXsetup.exe

备注：

其中--platform是配置打包成什么平台的安装文件，下面是可选的值

win系统： win或者win32，即--platform=win或者--platform=win32
 mac系统：mac或者darwin，即--platform=mac或者--platform=darwin
 Linux系统：linux， 即--platform=linux
 所有平台：all， 即--platform=all
 其中--arch是指定系统是什么架构的，常见的例如32位和64位操作系统，这个参数的可选值有

ia32， 即--arch=ia32， 32位操作系统，也可以在64位操作系统中安装
 x64， 即--arch=x64， 64位操作系统，使用本架构打包无法再32位操作系统中安装
 armv7l， 即--arch=armv7l， 使用比较少
 arm64， 即--arch=arm64， 使用比较少
 参数--platform和--arch已经被标志为过期，新的写法如下

