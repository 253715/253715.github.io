
---
title: 房屋出租管理系统
date: 2022-04-30 14:32:41
tags: 

- springboot
- 房屋出租管理系统

categories:

- Java学习

---

Verio是一个基于SSM+JSP的房屋租赁系统，包括管理员、房东和租客三种角色。
介绍地址：

## 0. 视频介绍

## 1. 项目导入，推荐插件安装，启动项目
#### 1）项目导入过程如下
- 通过IDEA导入，修改Constant里uploads目录位置
- 修改 db.properties
- 配置tomcat
- 创建数据库
- 导入数据库
- 运行tomcat

#### 2）推荐插件
- 搜索 Lombok，功能是帮我们生成getter/setter方法，在类上面加@Data注解即可，不要再写一大堆getter/setter方法啦！
- 搜索 Free MyBatis，功能是帮我们从mapper接口快速跳转到mapper xml 
- 搜索 RestfulToolkit，帮我们快速找到接口代码位置，即根据接口路径找controller类里方法位置，
- 快捷键 ctrl + \


## 2. 数据库设计讲解和功能介绍，交叉讲
- t_feedback  反馈表
- t_house	  房屋信息表
- t_mark	  收藏表
- t_news	  新闻表
- t_order     订单表
- t_user 	  用户表


## 3. 代码结构简单说明
- pom.xml 项目依赖工具
- src/main 代码父目录
- 	java  Java代码目录
- 		common 	公共的类：配置、常量、封装的对象 
- 		controller  控制器，负责接收请求，后端代码入口在这里，通常这一层会去调用 service 层(本质是service impl层)
- 		entity		实体类，跟数据库表一一对应，表名对应类名，字段名对应属性名
-		mapper		数据访问层，即直接操作数据库的接口(继承baseMapper，省去了增删改查的方法)
-		service		业务逻辑层接口
-			impl	业务逻辑层实现，通常这一层会去调用 mapper 层

- 常用调用关系：
```java
浏览器或js里的ajax方法 -> controller -> service impl -> mapper ->数据库SQL语句
```


- java  Java代码目录
- resources 配置文件目录
- webapp 前端文件目录

## 4. Spring讲解
Spring帮我们管理对象，不会重复创建对象，节约内存
#### 1. 使用方法
- 1）给需要让Spring管理的类加注解，如 @Service、@Controller、@Repository、@Component，如
```java
	@Service
	public class HouseServiceImpl implements HouseService {
		......
	}	

```
- 2）给需要创建对象的地方，@Autowired，如
```java
    @Autowired
    private HouseService houseService;
```


## 5. MyBatis讲解，JDBC、MyBatis、MyBatisPlus对比
mybatis：替代原始的JDBC的方法，直接通过定义接口，一个方法对应一个SQL语句，mybatis帮我们执行，不需要我们去处理像JDBC一样拿数据那么麻烦

举例：根据用户ID查询用户

- 1）JDBC的写法
```java
String sql = "SELECT * FROM user where id= ? ";
PreparedStatement statement = connection.prepareStatement(sql);
statement.setString(1, name);
ResultSet rs = statement.executeQuery();
while (rs.next()) {
	// 将数据库的数据转换成POJO实例
	user.setId(rs.getInt("id"));
	user.setUserName(rs.getString("userName"));
	user.setNickName(rs.getString("nickname"));
	user.setPhone(rs.getString("phone"));
	user.setEmail(rs.getString("email"));
	...... // 有几个属性需要set几次
}
```

- 2）MyBatis 写法
```java
	@Select("select * from user where id = #{value}")
	User findById(Long id);
```

- 3） MyBatis Plus写法
直接让Mapper接口基础 BaseMapper ，不需要再去写增删改查的逻辑了，直接从父类里面调用，父类的实现由mybatis plus帮我们做(动态代理实现)


## 6. SpringMVC讲解，Servlet和SpringMVC对比
SpringMVC 帮助我们管理请求接口，处理请求
如 查询用户信息页面

#### Servlet 和 SpringMVC对比

- 1 Servlet 的写法
```java

@WebServlet("/user/info")
public class UserInfoGetServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 解决乱码
        request.setCharacterEncoding("utf-8");
        // 获取用户ID
        Long id = (Long) request.getParameter("id");
       	// 去调用service 查询用户信息
       	User user = userService.findById(id);
        // 把用户信息传给前端 
        request.setAttribute("user", user);
        //向页面跳转
        request.getRequestDispatcher("/jsp/user/info.jsp").forward(request,response);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

- 2) SpringMVC写法
```java
    @RequestMapping("/user/info")
    public String profile(@RequestParam("id") Long id,  Model model) {
    	// 去调用service 查询用户信息
        User user = userService.get(id);
        // 把用户信息传给前端 
        model.addAttribute("user", user);
        // 页面跳转,在这个页面渲染
        return "admin/my-profile";
    }
```


#### SpringMVC详细讲解
1）@RequestParam
如新闻列表
```java
	@RequestMapping("/news")
    public String index(@RequestParam(value = "page", defaultValue = "1") Integer pageNumber,
                        @RequestParam(value = "size", defaultValue = "6") Integer pageSize,) {
        ...
    }

```

2）@PathVariable
房子详情页面
```java
	@RequestMapping("/house/detail/{id}")
    public String search(@PathVariable("id") Long id) {
    	...
    }	
```

3) @ResponseBody
登录提交请求
```java
	@RequestMapping(value = "/login/submit", method = RequestMethod.POST)
    @ResponseBody
    public JsonResult loginSubmit(@RequestParam("userName") String userName,
                                  @RequestParam("userPass") String userPass) {
	}
```

## 7.JSP讲解
模块分块，c:if、c:forEach 等标签的使用，${xxx} 的使用，时间格式化的使用

## 8.JavaScript 和 ajax 

onclick=""
登录代码示例


代码位置/webapp/assets/js/script.js
JS代码

```java
	function submitLogin() {
    $.ajax({
        type: 'POST',
        url: '/login/submit',
        async: false,
        data: $("#loginForm").serialize(), // 获取该表单下的所有参数
        success: function (data) {
            // 提示信息
            alert(data.msg);
            // 如果登录成功，刷新页面
            if (data.code == 1) {
                window.location.reload();
            }
        }
    });
}
```

JSP的代码
```java
<button type="button" onclick="submitLogin()" class="btn btn-md full-width pop-login bg-2">登录</button>

```

后端 `/login/submit` 这个请求的，可以通过 ctrl + \ 搜索，找到
后端代码controller这个方法必须要加 @ResponseBody


## 9. 怎么找代码和看代码？
- 全局搜索字符串，快捷键 ctrl + shift + f，比如我想搜索这个项目里所有的 Verio 的字符串，可以用这个快捷键
- 搜索接口，ctrl + \ ，比如我想搜索用户相关的接口，搜 /user，比如我要搜登录接口代码 /login/submit











