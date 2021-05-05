```shell
--storage.tsdb.retention=90d
```

该参数决定何时删除旧数据，默认为15天。
在启动脚本里更改`--storage.tsdb.retention=90d`可以延长，或者启动时带上这个参数即可。