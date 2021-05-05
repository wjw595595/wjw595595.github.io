关闭SELinux：

1、临时关闭（不用重启机器）：

setenforce 0                  ##设置SELinux 成为permissive模式                         ##setenforce 1 设置SELinux 成为enforcing模
2、修改配置文件需要重启机器：

修改/etc/selinux/config 文件

将SELINUX=enforcing改为SELINUX=disabled

重启机器即可


1、docker下载地址
https://download.docker.com/linux/static/stable/x86_64/docker-18.06.3-ce.tgz

2、解压
tar -xvf docker-xxxx.taz

3、将解压出来的docker文件内容移动到 /usr/bin/ 目录下
cp docker/* /usr/bin/
4、将docker注册为service
vim /etc/systemd/system/docker.service

将下列配置加到docker.service中并保存

```language
[Unit]

Description=Docker Application Container Engine

Documentation=https://docs.docker.com

After=network-online.target firewalld.service

Wants=network-online.target

[Service]

Type=notify

# the default is not to use systemd for cgroups because the delegate issues still

# exists and systemd currently does not support the cgroup feature set required

# for containers run by docker

ExecStart=/usr/bin/dockerd

ExecReload=/bin/kill -s HUP $MAINPID

# Having non-zero Limit*s causes performance problems due to accounting overhead

# in the kernel. We recommend using cgroups to do container-local accounting.

LimitNOFILE=infinity

LimitNPROC=infinity

LimitCORE=infinity

# Uncomment TasksMax if your systemd version supports it.

# Only systemd 226 and above support this version.

#TasksMax=infinity

TimeoutStartSec=0

# set delegate yes so that systemd does not reset the cgroups of docker containers

Delegate=yes

# kill only the docker process, not all processes in the cgroup

KillMode=process

# restart the docker process if it exits prematurely

Restart=on-failure

StartLimitBurst=3

StartLimitInterval=60s

 

[Install]

WantedBy=multi-user.target

```


4、启动

chmod +x /etc/systemd/system/docker.service             #添加文件权限并启动docker

systemctl daemon-reload                                                       #重载unit配置文件

systemctl start docker                                                             #启动Docker

systemctl enable docker.service                                           #设置开机自启

5、验证

systemctl status docker                                                         #查看Docker状态

docker -v                                                                                     #查看Docker版本