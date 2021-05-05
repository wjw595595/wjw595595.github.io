# extundelete  恢复工具

查看磁盘：

df -Th

extundelete  对 ext3 与 ext4 文件系统都有效

## 安装

下载：http://sourceforge.net/projects/extundelete/

 wget https://nchc.dl.sourceforge.net/project/extundelete/extundelete/0.2.4/extundelete-0.2.4.tar.bz2

```shell
#依赖包
#离线 可以http://www.rpmfind.net/linux/rpm2html/search.php?下载rpm   rpm -ivh --nodeps --force xxx（强制安装）  
#加上 --nodeps 属性，不考虑依赖关系
#--replacefiles  属性
yum -y install gcc+ gcc-c++ e2fsprogs.x86_64 e2fsprogs-devel.x86_64 e2fsprogs-libs
#解压
tar -jxvf extundelete-0.2.4.tar.bz2
cd extundelete-0.2.4
./configure
#生成 Makefile文件:./configure 可以指定安装位置./configure --prefix=/usr/local/java/extundelete
#编译并安装
make & make install

#进入extundelete安装目录:./extundelete -v，图下安装成功  默认在/usr/local/bin 目录下
默认安装： extundelete -v
```

## 恢复

**前提：如果确定文件被误删，在没有备份的情况下请马上对分区实施写入保护，（预防新的写入覆盖误删的块数据）****mount -o remount,ro /dev/sdb1或者直接umount /dev/sdb1/解挂载目录，\**df -h命令可以看出你的数据目录挂载在那个分区下（fdisk磁盘管理）\****

恢复指定文件：

**extundelete /dev/sdb1 --inode 2**

参考：

https://www.cnblogs.com/zhangan/p/10917780.html

**extundelete /dev/sdb1 --inode 2**

extundelete /dev/sdb1 --restore-inode 13  根据inode信息进行文件恢复

extundelete /dev/sdb1 --restore-file apache-tomcat-8.0.24.tar.gz  根据文件名进行文件修复

## 目录恢复操作过程

extundelete /dev/sdb1 --restore-directory /tomcat-app1 根据目录名称恢复目录

4.重新挂载磁盘目录或者reboot重启都是ok的。