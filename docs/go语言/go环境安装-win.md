

[官网](https://studygolang.com/dl)

## 安装

安装在c盘 不然后面运行会报错

go version

## 配置环境变量

在任意盘里新建文件夹GoWorks，里面再新建三个文件夹： bin、src、pkg

GOPATH配置：

![img](https://i.loli.net/2021/04/21/ScEgvjkqMdNhsiH.jpg)



## 查看环境变量

go env

代理

```
#开启go module：
go env -w GO111MODULE=on

#设置代理：
go env -w GOPROXY=https://goproxy.io,direct

查看效果：go env

项目下
go mod init
然后
go build(在当前目录生成)
go install 在GOPATH/bin目录生成

```

go. go mod ,goland参考下面，go mod 相当于下载依赖包  （go mod init）

https://www.jianshu.com/p/c6605c89e5b3

代理参考：

https://blog.csdn.net/qq_35941092/article/details/104986253

