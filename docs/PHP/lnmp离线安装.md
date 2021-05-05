# lnmp离线安装

项目地址：https://gitee.com/SimplerWorker/ollnmp
git 克隆： git  clone  https://gitee.com/SimplerWorker/ollnmp.git
前言：有时候，项目需要完全离线安装PHP环境，环境安装的时候，各种依赖让我痛苦不已，经过多次试验，终究练就此如来神掌，开源给大家。
环境： centos7.x+nginx1.15+mysql5.7.23+php7.2+redis4.0.0+python3+thinkphp5    and so on
第一步：准备一个centos7.x系统（这里以centos7.2为例）
第二步：挂载一个跟系统同一版本的镜像（everything版本的，yum源会更全）

- 上传一个centos7.2 everything版本的iso到已经安装好的centos7.2的 /opt 目录下
- 创建挂载目录： mkdir /media/CentOS7
- 挂载ISO： mount -t iso9660 -o loop /opt/CentOS-7-x86_64-DVD-1511_7.2.iso /media/CentOS7/
- 设置开机自动挂载镜像： echo mount -t iso9660 -o loop /opt/CentOS-7-x86_64-DVD-1511_7.2.iso /media/CentOS7/  >>  /etc/rc.local
- 配置源：
  1. mkdir /etc/yum.repos.d/bk
  2. mv /etc/yum.repos.d/* /etc/yum.repos.d/bk
- vi /etc/yum.repos.d/local.repo
  添加如下内容
```shell
[c7-media]
name=CentOS-$releasever - Media
baseurl=file:///media/CentOS7
gpgcheck=0
enabled=1
```

保存后退出

- 生成本地缓存
  1. yum clean all  （清除缓存）
  2. yum makecache （建立新缓存）
  3. 测试是否生效： yum install telnet
- 将项目下载后，上传到系统里面，例如/root/下
- 解压ollnmp后，进入ollnmp，执行 ./install lnmp
- 安装过程中，会要求填写相关信息，如实填写或者全部使用默认即可，遇到卡顿的地方，手动回车下

1、下载CentOS光盘镜像

```shell
cd /root && wget http://mirrors.163.com/centos/7/isos/x86_64/CentOS-7-x86_64-Everything-1810.iso
```

2、挂载光盘镜像

```shell
mkdir /mnt/dvd
mount -o loop /root/CentOS-7-x86_64-Everything-1810.iso /mnt/dvd
```


这样就将光盘挂载 /mnt/dvd 目录了。
当然这个挂载命令只是一次性的，系统重启或者自己umount后就没了，需要使用本地源yum安装时需要线执行这个挂载命令。
PS：如果像有多张ISO光盘的CentOS 6之类的版本，可以 mkdir /mnt/dvd2，再参考前面的命令将第二张挂载到 /mnt/dvd2 上。

3、备份yum源配置文件

将/etc/yum.repos.d/ 所有的以.repo结尾的文件全部重命名为：xxxx.repo.backup

4、配置新yum本地源

使用winscp、nano、vim之类的软件编辑 /etc/yum.repos.d/CentOS-Media.repo
添加如下内容：
[local-media]
name=CentOS-$releasever - Media
baseurl=file:///mnt/dvd/
#file:///mnt/dvd2/
#如果有第二张光盘将前面dvd2行前面的 # 注释符号去掉
gpgcheck=1
enabled=1
gpgkey=file:///mnt/dvd/RPM-GPG-KEY-CentOS-7
保存
gpgcheck 签名检查可以改成 0 就会不检查。
gpgkey 最后面如果是CentOS-6就把最后面数字改成6
CentOS 8本地源配置文件写法与CentOS6和7不同，配置文件内容如下：
[LocalRepo_BaseOS]
name=LocalRepository_BaseOS
baseurl=file:///mnt/dvd/BaseOS
enabled=1
gpgcheck=0
[LocalRepo_AppStream]
name=LocalRepository_AppStream
baseurl=file:///mnt/dvd/AppStream
enabled=1
gpgcheck=0
保存

5、测试yum本地源是否正常工作

执行以下命令，清空以下缓存并创建新的缓存
yum clean all
yum makecache
然后 yum install wget 试一下能否正常安装依赖包。
没有报错的话就是正常工作了，当然wget也可能已经安装了，也可以换其他软件包尝试。
如果是要离线安装lnmp一键安装包，需要添加 CheckMirror=n 参数实现，例子 CheckMirror=n ./install.sh lnmp。