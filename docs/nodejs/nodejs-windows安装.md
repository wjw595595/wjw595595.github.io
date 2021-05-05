# windows安装

## 下载

https://nodejs.org/en/  直接下一步

https://nodejs.org/zh-cn/download/

node -v

npm -v

## 更改 node全局下载路径

```cmd
npm config set prefix "D:\Program Files\nodejs\npm_global"
npm config set cache "D:\Program Files\nodejs\npm-cache"
npm config set registry "https://registry.npm.taobao.org"
#查看
npm config ls
```

配置文件：C:\Users\admin\.npmrc

环境变量path 添加：D:\Program Files\nodejs\npm_global

## 修改为淘宝镜像

在文件C:\Users\admin\.npmrc添加

registry=https://registry.npm.taobao.org

全局安装

```npm
npm install -g <package_name>
```

局部安装：局部安装就是在当前项目中建立包，在当前项目中起作用

```npm
npm install <package_name>
```

## 通过nvm-windows安装

https://github.com/coreybutler/nvm-windows/releases

参考：https://www.jianshu.com/p/96f9317db0b5

​	第三个：傻瓜安装![image-20210323164452039](https://i.loli.net/2021/03/23/BxMnEmdzcRDNHr4.png)

```
#查看
nvm list available
#安装
nvm install 10.24.0
#使用
nvm use 10.24.0
#查看
node -v

```

### 安装乱码

![image-20210323165833457](https://i.loli.net/2021/03/23/c4nePZsG3jEYgyX.png)



因为 安装在了 

D:\Program Files  ，目录有空格，移动到别的目录修改  文件安装目录的 settings.txt文件

### npm安装失败

settings.txt 

修改添加地址：

node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/

参考：https://blog.csdn.net/fenfeidexiatian/article/details/96993384

### 命令

1,nvm nvm list 是查找本电脑上所有的node版本

- nvm list 查看已经安装的版本
- nvm list installed 查看已经安装的版本
- nvm list available 查看网络可以安装的版本

2,nvm install 安装最新版本nvm

3,nvm use <version> ## 切换使用指定的版本node

4,nvm ls 列出所有版本

5,nvm current显示当前版本

6,nvm alias <name> <version> ## 给不同的版本号添加别名

7,nvm unalias <name> ## 删除已定义的别名

8,nvm reinstall-packages <version> ## 在当前版本node环境下，重新全局安装指定版本号的npm包

9,nvm on 打开nodejs控制

10,nvm off 关闭nodejs控制

11,nvm proxy 查看设置与代理

12,nvm node_mirror [url] 设置或者查看setting.txt中的node_mirror，如果不设置的默认是 https://nodejs.org/dist/
　　nvm npm_mirror [url] 设置或者查看setting.txt中的npm_mirror,如果不设置的话默认的是： https://github.com/npm/npm/archive/.

13,nvm uninstall <version> 卸载制定的版本

14,nvm use [version] [arch] 切换制定的node版本和位数

15,nvm root [path] 设置和查看root路径

16,nvm version 查看当前的版本

