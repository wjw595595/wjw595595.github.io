查看UDP端口

windows
 nc -vuz 172.16.10.248 123

 linux
 ncat -vuz 102.19.193.223 161    # 先在服务器上测试客户端的udp端口是不

返回 open 就是可以

linux安装： 

yum install -y nc

windows

https://eternallybored.org/misc/netcat/ 下载

把 nc.exe 放到 C:\Windows\System32

cmd  nc -h