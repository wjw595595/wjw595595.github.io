https://blog.csdn.net/andyzhaojianhui/article/details/78872415



Nginx提供了rewrite指令，用于对地址进行重写，语法规则：

登录后复制

```
rewrite   "用来匹配路径的正则"       重写后的路径           [指令];
```

登录后复制

```
 我们实现把**/api/upload**重写为**/upload**的功能，在Nginx的配置文件中配置如下内容：
```

登录后复制

```
# 上传路径的映射
        location /api/upload {  
            proxy_pass http://127.0.0.1:8082;
            proxy_connect_timeout 600;
            proxy_read_timeout 600;

            rewrite "^/api/(.*)$" /$1 break; 
        }

        location / {
            proxy_pass http://127.0.0.1:10010;
            proxy_connect_timeout 600;
            proxy_read_timeout 600;
        }
```

------

登录后复制

```
     首先，我们映射路径是/api/upload，而下面一个映射路径是 / ，根据最长路径匹配原则，/api/upload优先级更高。也就是说，凡是以/api/upload开头的路径，都会被第一个配置处理。
```

1. proxy_pass：反向代理，这次我们代理到8082端口，也就是upload-service服务；
2. rewrite "^/api/(.*)$" /$1 break，路径重写：
   (1)"^/api/(.*)$"：匹配路径的正则表达式，用了分组语法就是**(.*)**，把/api/以后的所有部分当做1组；
   （2）/$1：重写的目标路径，这里用$1引用前面正则表达式匹配到的分组（组编号从1开始，也就是api），即/api/后面的所有。这样新的路径就是除去/api/以外的所有，就达到了去除/api前缀的目的；
3. break：指令，常用的有2个，分别是：last、break；
   （1）last：重写路径结束后，将得到的路径重新进行一次路径匹配；
   （2）break：重写路径结束后，不再重新匹配路径。