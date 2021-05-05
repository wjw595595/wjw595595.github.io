## 一、获取镜像
#指定版本，该版本包含了web控制页面
docker pull rabbitmq:management

## 二、运行镜像

#方式一：默认guest 用户，密码也是 guest
docker run -d --hostname my-rabbit --name rabbit -p 15672:15672 -p 5672:5672 rabbitmq:management

#方式二：设置用户名和密码
docker run -d --hostname my-rabbit --name rabbit -e RABBITMQ_DEFAULT_USER=user -e RABBITMQ_DEFAULT_PASS=password -p 15672:15672 -p 5672:5672 rabbitmq:management

## 三、访问url
http://localhost:15672/

rabbitmq 启用日志跟踪
docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmq rabbitmq:management

docker exec -it rabbitmq bash

rabbitmq-plugins enable rabbitmq_tracing

