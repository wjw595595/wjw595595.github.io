```shell
docker pull zabbix/zabbix-server-mysql:centos-4.4-latest
```

第二步：

```shell
docker run --name some-zabbix-server-mysql -e DB_SERVER_HOST="172.17.0.2" -e MYSQL_USER="root" -e MYSQL_PASSWORD="123456" -d zabbix/zabbix-server-mysql:centos-4.4-latest
```

第三步：

```shell
docker run --name some-zabbix-web-nginx-mysql -p 80:8080 --link some-zabbix-server-mysql:zabbix-server -e DB_SERVER_HOST="172.17.0.2" -e MYSQL_USER="root" -e MYSQL_PASSWORD="123456" -e ZBX_SERVER_HOST="172.17.0.3" -e PHP_TZ="Asia/Shanghai" -d zabbix/zabbix-web-nginx-mysql:centos-4.4-latest
```