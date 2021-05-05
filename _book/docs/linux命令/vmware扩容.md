# vmware虚拟机扩容

## 扩展vmware-centos7硬盘空间

关闭Vmware的centos7系统，才能在VMWare菜单中设置需要增加到的磁盘大小

![img](https://i.loli.net/2021/02/22/GCMwySUJ8lscLjm.png)

如果这个选项是灰色的，说明此虚拟机建有快照，把快照全部删除再试试!

或者web界面：

选中虚拟机->虚拟机设置->硬盘->实用工具->扩展->设置最大磁盘大小->点击扩展 

## 对新增加的硬盘进行分区、格式化

```shell
我们增加了空间的硬盘是 /dev/sda

分区： 
[root@localhost]# fdisk /dev/sda　　　　 
p　　　　　　　查看已分区数量（我看到有两个 /dev/sda1 /dev/sda2） 
n　　　　　　　新增加一个分区
p　　　　　　　分区类型我们选择为主分区 
　　　　　　     分区号输入3（因为1,2已经用过了,sda1是分区1,sda2是分区2,sda3分区3） 
回车　　　　　  默认（起始扇区） 
回车　　　　　  默认（结束扇区） 
t　　　　　　　 修改分区类型 
　　　　　　     选分区3 
8e　　　　　 　修改为LVM（8e就是LVM）
w　　　　　  　写分区表 
q　　　　　  　完成，退出fdisk命令

使用partprobe 命令 或者重启机器 

格式化分区3命令:

mkfs.ext3 /dev/sda3
```

![img](https://i.loli.net/2021/02/22/Pt3mYXHdzaSLxw4.png)

![img](https://i.loli.net/2021/02/22/LlyB76aciMQGsWO.png)

![img](https://i.loli.net/2021/02/22/SLBxY9KpmdfQ7hT.png)

![img](https://i.loli.net/2021/02/22/QD5IZnPqj8FNRTx.png)

## 添加新lvm到已有的lvm组，实现扩容

```shell
lvm　　　　　　　　　　　　           进入lvm管理

lvm>pvcreate /dev/sda3　　           这是初始化刚才的分区3

lvm>vgextend centos /dev/sda3     将初始化过的分区加入到虚拟卷组centos (卷和卷组的命令可以通过 vgdisplay )

lvm>vgdisplay -v或者vgdisplay查看free PE /Site

lvm>lvextend -l+6143 /dev/mapper/centos-root　　扩展已有卷的容量（6143 是通过vgdisplay查看free PE /Site的大小）

lvm>pvdisplay 查看卷容量，这时你会看到一个很大的卷了

lvm>quit 　退出
```

![img](https://i.loli.net/2021/02/22/9JXcLvxznS1frlG.png)

![img](https://i.loli.net/2021/02/22/wmv9fsd4iBSRoC2.png)

![img](https://i.loli.net/2021/02/22/Jmf4aYCwXAKFZpx.png)

上面只是卷扩容了，下面是文件系统的真正扩容，输入以下命令：

CentOS7下面由于使用的是XFS命令:

/dev/mapper/centos-root是df -h查看到根目录的挂载点

```
xfs_growfs /dev/mapper/centos-root
```

参考文章：

https://blog.csdn.net/Wang_Xin_SH/article/details/77872885?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&dist_request_id=192a8808-ed95-4e2d-9a6e-d54b687bcbc5&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control

https://blog.csdn.net/zzchances/article/details/89918277