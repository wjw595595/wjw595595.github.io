

# pagehelper配置

## 导入依赖包

```java
<dependency>
	<groupId>com.github.pagehelper</groupId>
	<artifactId>pagehelper-spring-boot-starter</artifactId>
	<version>1.2.13</version>
</dependency>
```

## 配置

在application.properties配置文件中添加以下配置：

```xml
#pagehelper分页插件配置
pagehelper.helperDialect=mysql
pagehelper.reasonable=true
pagehelper.supportMethodsArguments=true
pagehelper.params=count=countSql
```

## 使用

在controller中加入 语法

```java
//PageHelper.startPage(pageNo,pageSize);
@GetMapping("/users")
public List<User> lists(@RequestParam(defaultValue = "1") int pageNo, @RequestParam(defaultValue = "10") int pageSize) {
    PageHelper.startPage(pageNo,pageSize);
    return userService.getUsers();
}
```

说明：

- pagehelper 只对startpage下的第一个语句有用

- `pageNo`和`pageSize`两个参数是为了接收前台传过来的值，并且通过`defaultValue`为这两个参数提供了默认值。
- 分页主要代码：`PageHelper.startPage(pageNo,pageSize);`

### 实例

- 第一个返回分页数据
- 第二个返回分页信息，及包含总页数、当前页数、每页条数等分页相关的信息

#### 创建mapper

```java
package com.jeff.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

import com.github.pagehelper.Page;
import com.jeff.entity.User;

@Mapper
public interface UserMapper {
	
	@Select("select * from sys_user where id=#{id}")
    User getUserById(@Param("id") Long id);

	@Select("select * from sys_user")
	List<User> getUserList();

	@Select("select * from sys_user")
	Page<User> getUserList2();

}
```

#### 创建service

```java
package com.jeff.service;

import java.util.List;

import com.github.pagehelper.Page;
import com.jeff.entity.User;

public interface UserService {

	User getUserById(Long id);

	List<User> getUserList1();

	Page<User> getUserList2();

}
```
#### 创建serviceImpl

```java
package com.jeff.service.impl;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.github.pagehelper.Page;
import com.jeff.entity.User;
import com.jeff.mapper.UserMapper;
import com.jeff.service.UserService;

@Service
public class UserServiceImpl implements UserService {
	
	@Autowired
	private UserMapper mapper;

	@Override
	public User getUserById(Long id) {
		
		return mapper.getUserById(id);
	}

	@Override
	public List<User> getUserList1() {
		
		return mapper.getUserList();
	}

	@Override
	public Page<User> getUserList2() {
		
		return mapper.getUserList2();
	}

}
```

## 创建controller

方法二也可以这样

用`com.github.pagehelper.PageInfo`类封装`Page<User>`数据

PageInfo<User> pageInfo = new PageInfo<>(getUserList2());



```java
package com.jeff.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import com.github.pagehelper.PageInfo;
import com.jeff.entity.User;
import com.jeff.entity.request.PageEntity;
import com.jeff.service.UserService;

@RestController
@RequestMapping("user")
public class UserController {

	@Autowired
	private UserService service;

	@RequestMapping("getUserById")
	public User getUserById(Long id) {

		return service.getUserById(id);
	}

	/**
	 * 
	 * @description: 分页查询方法一
	 * @author: Jeff
	 * @date: 2020年3月14日
	 * @param page
	 * @return
	 */
	@RequestMapping("getUserList1")
	public Object getUserList1(PageEntity page) {
		PageHelper.startPage(page.getPage(), page.getRows());
		List<User> list = service.getUserList1();
		PageInfo<User> pageInfo = new PageInfo<>(list);
		return pageInfo;
	}

	/**
	 * 
	 * @description: 分页查询方法二
	 * @author: Jeff
	 * @date: 2020年3月14日
	 * @param page
	 * @return
	 */
	@RequestMapping("getUserList2")
	public Object getUserList2(PageEntity page) {
		PageHelper.startPage(page.getPage(), page.getRows());
		Page<User> list = service.getUserList2();
		return list;
	}

}

```

