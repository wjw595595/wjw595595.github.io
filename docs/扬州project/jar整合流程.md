## 前端打包

```
npm run build
/dist 生成包
#整体复制到  idc-services/ resource/static/static

```

![image-20210423181620584](https://i.loli.net/2021/04/23/2CzNeacskqUjoM6.png)

修改对应的后端地址

![image-20210423181823575](https://i.loli.net/2021/04/23/qgO1acJB7SGIsMV.png)

maven 打包

上传  192.168.210.80

/usr/local/bin

运行

```
$nohup java -jar idc-services-1.0.0.jar  > log.file  2>&1 &
```

地址

http://192.168.210.80:8081/idc-services/static/index.html#/login