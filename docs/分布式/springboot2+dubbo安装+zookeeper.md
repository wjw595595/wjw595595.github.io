### 一、dubbo-admin安装
https://dubbo.apache.org/zh-cn/ 下载
https://github.com/apache/incubator-dubbo-admin
主分支
https://github.com/apache/dubbo-admin/tree/master  springboot项目 编译成jar运行

### 二、zookeeper安装

### 三、工程结构
API接口：提供接口定义，数据结构定义
服务端：提供接口实现
客户端：去使用接口

@Service  分spring注解和dubbo注解

方法一、
启动类加 @EnableDubbo
@Service
import com.alibaba.dubbo.config.annotation.Service;
@Service(version = "${provider.service.version}") 可以加版本号
加配置文件
```language
#当前服务/应用名称
dubbo.application.name=provider
#注册中心的协议和地址
#dubbo.server=true
dubbo.registry.address=zookeeper://192.168.210.72:2181
#通信规则(通信协议和接口)
dubbo.protocol.name=dubbo
dubbo.protocol.port=20880
dubbo.scan.base-packages=com.geekq.provider.service.impl
## Service version
provider.service.version=1.0.0
```
方法二、
用
import org.springframework.stereotype.Service;
@Service("goodGroupService")
启动类加
@ImportResource(value={"classpath:provider.xml"})
```language
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
		
	<!-- 为当前服务提供者取个名字，并且提供给注册中心 -->
	<dubbo:application name="miaosha-order-service"></dubbo:application>
	
	<!-- 注册中心的配置，使用zk暴露服务 -->
	<dubbo:registry protocol="zookeeper" address="192.168.210.72:2181"></dubbo:registry>

	<!-- 定义暴露服务的端口号 -->
	<dubbo:protocol name="dubbo" port="20880" ></dubbo:protocol>

	<!-- 暴露具体的服务接口 本地伪装-->
	<dubbo:service retries="3" interface="com.geekq.api.service.GoodsService"
		ref="goodsService" group="goods2" timeout="6000" mock="true">
		<dubbo:method name="listGoodsVo" timeout="3000" ></dubbo:method>
	</dubbo:service>

	<dubbo:service interface="com.geekq.api.service.GoodsService" ref="goodGroupService" group="goods1"></dubbo:service>


</beans>
```
