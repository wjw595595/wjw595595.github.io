# linux常用技巧

## rpm强制升级

```
--nodeps就是安装时不检查依赖关系
﻿rpm -ivh --replacefiles --force --nodeps
```

在做RPM软件适配的时候，经常会出现需要自己安装额外的安装包，包名中的版本号不一致也会出现提示，要安装统一版本号的软件包，并且还会出现对更新软件包的依赖，在已经存在软件包的情况下，按需升级软件包，可以使用升级安装：

```
rpm -Uvh *.rpm
```


1
如果升级出错，可以执行强制安装实现对软件包的版本更新：

```
rpm -ivh --replacefiles --force --nodeps *.rpm
```


1
如果想要强制卸载某软件包，可以使用如下命令：

```
rpm -e *.rpm --nodeps

rpm -q 软件包名

whereis influxdb
ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz
再使用rpm -qf查询系统文件属于哪个软件包(file)
rpm -qf  /usr/bin/ls

rpm -qa | grep xxx
-qa代表query，a代表all
 rpm -qa|grep influxdb
```


1
注意：在有很多依赖时，不推荐强制卸载，如果非要试一试，要做好系统备份啊~~

另外，一个使用的查看软件包安装脚本的命令：

```
rpm --script -qp *.rpm
```


![title](https://i.loli.net/2021/03/09/kNMyYQaldITqpKu.png)

```
[root@localhost ~]# netstat -ntlp   //查看当前所有tcp端口·
[root@localhost ~]# netstat -ntulp |grep 80   //查看所有80端口使用情况·
[root@localhost ~]# netstat -an | grep 3306   //查看所有3306端口使用情况·
 
[root@localhost ~]# netstat -nlp |grep LISTEN   //查看当前所有监听端口·

netstat -nupl (UDP类型的端口)
netstat -ntpl (TCP类型的端口)
netstat -anp 显示系统端口使用情况
 列出谁在使用某个端口
lsof -i :3306
全显示
netstat -tunlp|grep
https://jingyan.baidu.com/article/656db9183861cde381249c87.html
```

## 压缩解压缩

zip -r file.zip file/

https://www.cnblogs.com/mingyue5826/p/14281153.html

https://www.cnblogs.com/wangluochong/p/7194037.html

### 查看隐藏文件

```
ls -al
```

## lsof详解

https://blog.csdn.net/kozazyh/article/details/5495532

**列出谁在使用某个端口**

lsof -i :3306

**列出所有的网络连接**

**lsof -i**

 **列出所有tcp 网络连接信息**

lsof  -i tcp

**查看谁正在使用某个文件**

lsof  /filepath/file

**列出某个程序所打开的文件信息**

**lsof -c mysql**

lsof  -u username

备注: -u 选项，u其实是user的缩写

## linux自建回收站

https://www.cnblogs.com/llxx07/p/6515943.html



当前目录搜索

## 查询文本中内容

grep 递归搜索文本内容

- `r`递归、 `n` 行号

  ```
  # grep -rn "ipmitool"
  ```

  - `i` 不区分大小写、`r` `n`

    ```
    grep -irn 3306 ./
    ```

    - `l`显示文件名

    - ```
      grep -ilr 3306 ./
      ```

替换

```
grep -irn "192.168.0.1" ./
grep -irl "192.168.0.1" ./ | xargs sed -i "s/192.168.0.1/192.168.0.2/g"
```



grep -rn "localhost:3000" *

grep时加入了-Hna，其分别输出文件路径，行号，及对应的文本内容：

grep -rn erlsrv

/etc 目录  

RemoteWare Server

grep -rn "RemoteWare " *

- grep -i pattern files：不区分大小写地搜索,默认情况区分大小写
- grep -l pattern files：只列出匹配的文件名
- grep -L pattern files：列出不匹配的文件名
- grep -w pattern files：只匹配整个单词，而不是字符串的一部分（如匹配"magic"，而不是"magical"）
- grep -C number pattern files：匹配的上下文分别显示[number]行
- grep pattern1 | pattern2 files：显示匹配 pattern1 或 pattern2 的行
- grep pattern1 files | grep pattern2：显示既匹配 pattern1 又匹配 pattern2 的行
- 明确要求搜索子目录：grep -r
- 或忽略子目录：grep -d skip



查磁盘信息

```
df -Th
[root@xhrmyy_it system]# df -Th
文件系统                          类型      容量  已用  可用 已用% 挂载点
devtmpfs                          devtmpfs  3.8G     0  3.8G    0% /dev
tmpfs                             tmpfs     3.8G  8.0K  3.8G    1% /dev/shm
tmpfs                             tmpfs     3.8G  389M  3.4G   11% /run
tmpfs                             tmpfs     3.8G     0  3.8G    0% /sys/fs/cgroup
/dev/mapper/centos_xhrmyy_it-root xfs        50G   12G   39G   24% /
/dev/sda1                         xfs      1014M  199M  816M   20% /boot
/dev/mapper/centos_xhrmyy_it-home xfs       499G  2.1G  497G    1% /home
tmpfs                             tmpfs     773M  4.0K  773M    1% /run/user/42
tmpfs                             tmpfs     773M   60K  773M    1% /run/user/0
[root@xhrmyy_it system]# df -Th^C

fdisk -l
```

## vmstat

执行vmstat命令获得系统CPU负载情况,vmstat 2 10表示2秒钟输出一次共输出10组数据