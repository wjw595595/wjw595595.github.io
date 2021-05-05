参考：

https://blog.csdn.net/weixin_38004638/article/details/99655322

@GetMapping，处理get请求
@PostMapping，处理post请求
@PutMapping，处理put请求
@DeleteMapping，处理delete请求

@PostMapping(value = "/user/login")

等于@RequestMapping(value = "/user/login",method = RequestMethod.POST)

@RequestParam("username" ) String username,

```
@RequestParam Map<String, Object> params
params.get("instance")
```

@RequestParam有三个配置参数：

required 表示是否必须，默认为 true，必须。
defaultValue 可设置请求参数的默认值。
value 为接收url的参数名（相当于key值）。
@RequestParam用来处理 Content-Type 为 application/x-www-form-urlencoded 编码的内容，Content-Type默认为该属性。@RequestParam也可用于其它类型的请求，例如：POST、DELETE等请求。不能批量操作

![img](https://i.loli.net/2021/04/14/YqDZdcyUHFASXLG.jpg)

不推荐使用@RequestParam接收application/json，这时候就需要使用到@RequestBody。

**向表中批量插入数据**

举个批量插入数据的例子，Controller层的写法如下图所示

![img](https://i.loli.net/2021/04/14/rVBJZNKCLFgk2eq.jpg)

```
@GetMapping("/winDetail/{instance}")
@PathVariable("instance")String instance
```

