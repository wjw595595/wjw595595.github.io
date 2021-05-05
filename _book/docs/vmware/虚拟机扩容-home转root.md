
# mkdir /backup
# mv /home/* /backup/
# umount /home
# lvremove /dev/centos/home
# lvcreate -L 50G -n home cents
# mkfs -t xfs /dev/centos/home
# mv /backup/* /home/
# lvextend -L +xxxG /dev/centos/root
# xfs_growfs root
# rm -rf /backup



# mkdir /backup && mv /home/* /backup

# umount /home
#lvremove /dev/centos/home //删除逻辑卷home
# lvcreate -L 50G -n home cents
vgchange -ay centos
# mkfs -t xfs /dev/centos/home
# mount /dev/centos/home /home/
# mv /backup/* /home
# lvextend -L +xxxG /dev/centos/root
# vgchange -ay centos
#xfs_growfs /dev/centos/root


# mkdir /backup && mv /home/* /backup

# umount /home
#lvremove /dev/centos/home //删除逻辑卷home
# vgdisplay //查看卷组可用空间 Free PE / Size 中显示的空间为卷组的空闲空间 home释放的
# lvcreate -L 50G -n home centos //L表示大小，默认单位为M；n表示卷名；这里的centos是CentOS7安装系统的时候就默认建立好的卷组名
# lvdisplay //查看逻辑卷home
## vgdisplay //再次查看卷组空间大小
#vgchange -ay centos  //可选步骤：激活卷组centos，使得这个新建的home逻辑卷生效
# mkfs -t xfs /dev/centos/home   //在新建的逻辑卷home上建立xfs文件系统
# mount /dev/centos/home /home/  //把这个新逻辑卷home挂到之前的文件夹/home中去，直接重启用fstab来挂载也行
# df -h //现在来查看磁盘使用情况
# mv /backup/* /home  //再把之前拷出来的东西拷回新建的/home中
# lvextend -L +xxxG /dev/centos/root  //把剩下的823G现在分配给root卷，剩下那点渣渣空间让它闲着；+号表示在原来的基础上额外增加，不要加好则设定为具体额度
# vgchange -ay centos
#xfs_growfs /dev/centos/root

#查看磁盘使用情况
#df -h
# vgdisplay //查看逻辑卷组情况
# lvdisplay //查看逻辑卷情况，默认三个，root、home和交换空间swap

fdisk -l