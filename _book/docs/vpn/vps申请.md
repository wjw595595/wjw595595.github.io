https://github.com/Alvin9999/new-pac/wiki/%E8%87%AA%E5%BB%BAss%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%95%99%E7%A8%8B

# vps申请

一、申请虚机

[www.vultr.com](http://www.vultr.com/)

[315824016@qq.com](mailto:315824016@qq.com)  WJWwjw59510....

1、点左侧server

进行购买

2、测试ip是否是东京的

[https://ip.cn/](https://ip.cn/)

3、xshell连接

二、执行命令

wget --no-check-certificate  [https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh](https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh)

chmod +x shadowsocks.sh

./shadowsocks.sh 2>&1 | tee shadowsocks.log

#设置连接密码和端口

设置的结果：

Your Server IP        :  66.42.45.247

Your Server Port      :  8888

Your Password         :  106511

Your Encryption Method:  aes-256-cfb  #一定要选这个7，iphone支持这个

三、加速

锐速不支持Openvz！！！锐速不支持Openvz！！！锐速不支持Openvz！

检测：

#执行一键安装锐速命令

wget -N --no-check-certificate [https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh](https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh) && bash serverspeeder-all.sh

要是有提示：锐速不支持该内核

rpm -ivh [http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-3.10.0-229.1.2.el7.x86_64.rpm](http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-3.10.0-229.1.2.el7.x86_64.rpm) --force

然后：重启 reboot

再次执行安装命令

wget -N --no-check-certificate [https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh](https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh) && bash serverspeeder-all.sh

选 1

其他：

卸载

chattr -i /serverspeeder/etc/apx* && /serverspeeder/bin/serverSpeeder.sh uninstall -f

锐速相关命令

service serverSpeeder status 查看serverSpeeder的状态

service serverSpeeder start | stop | restart 停止暂停重启锐速

四、配置多客户端

配置文件路径及代码：

vi /etcconfig.json

按i编辑，esc退出编辑，然后按Shift+Q编辑状态输入wq保存

{

   "port_password":{

        "8989":"password0",

        "9001":"password1",

        "9002":"password2",

        "9003":"password3",

        "9004":"password4"

   },

   "method":"aes-256-cfb",

   "timeout":600

}

五、客户端安装

iphone：

1、iphone下载wingy（免费的），app store里搜wingy（中国app store已经下架，可以换到美国账户下载）

[https://itunes.apple.com/us/app/wingy-proxy-for-http-s-socks5/id1178584911?mt=8](https://itunes.apple.com/us/app/wingy-proxy-for-http-s-socks5/id1178584911?mt=8)

点击选择线路--新增线路--shadowsocks--依次填入刚才记录的信息就好--保存--点击连接就可以了

这样就翻墙成功了

2、windows

[https://github.com/shadowsocks/shadowsocks-windows/releases](https://github.com/shadowsocks/shadowsocks-windows/releases)

下载Shadowsocks-4.0.6.zip

解压：填入之前记录的自己的服务器信息，点击确定

在电脑右下角任务栏找到ss图标，右键点击，点击启用系统代理就可以了，试试上google吧

3、android

[https://github.com/shadowsocks/shadowsocks-android/releases](https://github.com/shadowsocks/shadowsocks-android/releases)

添加配置就可以了

[https://www.amz520.com/articles/25048.html