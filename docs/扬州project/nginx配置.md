```conf

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       8087;
        server_name  localhost;
        location / {
            root   D:/GreenSoft/nginx-1.19.10/vue-project;     #站点根目录，即网页文件存放的根目录, 默认主页目录在nginx安装目录的html子目录。 
            index  index.html index.htm;    #目录内的默认打开文件,如果没有匹配到index.html,则搜索index.htm,依次类推
        }

         location /api {
		    rewrite ^/api/(.*)$ /$1 break; 
            proxy_pass  http://127.0.0.1:8080/idc-services;    #node api server 即需要代理的IP地址
            add_header Access-Control-Allow-Origin *;
             add_header Access-Control-Allow-Credentials true;
         }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

}

```

