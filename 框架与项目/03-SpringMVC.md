服务器接收请求，转换为json格式，给客户端
# 服务器

**什么是Web服务器**
Web服务器是一个应用程序（软件），对 HTTP协议的操作进行封装，使得程序员不必直接对协议进行操作，让Web开发更加便捷。主要功能是“提供网上信息浏览服务”
- Web服务器:负责解析 HTTP 协议，解析请求数据，并发送响应数据
- 浏览器按照HTTP协议发送请求和数据，后台就需要一个Web服务器软件来根据HTTP协议解析请求和数据，然后把处理结果再按照HTTP协议发送给浏览器
- Web服务器软件有很多，我们课程中将学习的是目前最为常用的==Tomcat==服务器



# 初步创建连接
1. 创建web工程（Maven结构）√
2. 导入坐标（boot整合SpringMVC） 
3. 在springboot中 只需要引入依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
设置一下端口号`server:  port: 8081`

4. 定义处理请求的功能类（Controller）
```java
@Controller  
//指定当前类是springmvc的控制器bean  
public class TestController {  
//    public  String test(){  
//        return "test.html";  
//        //有@ResponseBody默认返回是String，不是网页，就可能404  
//    }  

    //设置映射路径为/save，即外部访问路径  
    @RequestMapping("/save")  
    @ResponseBody  
    //设置当前操作返回结果为指定json数据（本质上是一个字符串信息）  
    //位置：类上/方法上作用：当前方法的响应内容就是返回值，无需解析  
    public String save(){  
        System.out.println("user save ...");  
        return "{'info':'springmvc'}";  
    }  
}
```
然后我们运行Application启动类
在浏览器输入`http://localhost:8081/save`
就可以就看到结果页面了
如果在public class TestController 上面加了@RequestMapping("/test") 
那就相当于嵌套了 我们就不能直接访问save了，要写`http://localhost:8081/test/save`

5. 设置请求映射（配置映射关系）
6. 将SpringMVC加载到Tomcat容器中

| 注解                      | 作用                                                      | 位置                                        | 例                                                                                                                                                         |
| ----------------------- | ------------------------------------------------------- | ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| @Controller             | 注册Bean                                                  | 类上                                        | @Controller<br>public class UserController {<br>}                                                                                                         |
| @RequestMapping（“映射路径”） | 设置映射路径为，即外部访问路径                                         | 方法上/其实@RequestMapping注解还可以写到类上面，笔记后面会介绍到。 | @RequestMapping("/save")<br>public void save(){<br>    System.out.println("user save ...");<br>}                                                          |
| @ResponseBody           | **该方法的返回值将直接作为 HTTP 响应体（Body）返回给浏览器**，而不是作为“视图名称”去查找页面。 | 当前（类下所有）方法的响应内容就是返回值，无需解析                 | @RequestMapping("/save")<br>@ResponseBody<br>public String save(){<br>    System.out.println("user save ...");<br>    return "{'info':'springmvc'}";<br>} |

# 静态资源调用
![[Pasted image 20261008184201.png]]
Tip-- 一定是static文件夹下放静态资源，否则spring不会自动解析
**直接访问静态资源**

其实对于 `resources/static` 目录下的静态 HTML，Spring Boot 默认已经把它们映射为根路径了。  
你完全不需要通过 `/save` 接口来展示页面，直接访问 `http://localhost:8081/test.html` 就能看到表单了。
```java
  
@Controller  
//指定当前类是springmvc的控制器bean  
public class TestController {  
    @RequestMapping("/save")  
    public String  save(HttpServletRequest req){  
        //获取请求行里的信息   数据是由发起者（浏览器）  
        //1.获取请求方式  
        String method = req.getMethod();  
        System.out.println("获取请求方式："+method);  
        //3.获取URL(统一资源定位符):请求的网址，不是发起的页面。  
        //http://localhost:81/webservlet/requestdemo1  
        StringBuffer requestURL = req.getRequestURL();  
        System.out.println("获取URL(统一资源定位符):"+requestURL);  
        //4. 获取URI(统一资源标识符)(更常用):/webservlet/requestdemo1  
        String requestURI = req.getRequestURI();  
        System.out.println("获取URI(统一资源标识符):"+requestURI);  
        //获取请求头  
        String header = req.getHeader("Accept-Encoding");  
        System.out.println("请求头Accept-Encoding="+header);  
        //获取请求头: user-agent: 浏览器的版本信息  
        String agent = req.getHeader("user-agent");  
        System.out.println(agent);  
        return "test.html";  //这就没有设置responsebody 会根据这个去查找对应的静态资源
    }  
    @RequestMapping("/post")  
    @ResponseBody  
    public String postTest(String username,String password){  
        System.out.println("username = " + username);  
        System.out.println("password = " + password);  
        System.out.println("xxxx");  
        return "success";  //Success会返回给前端
    }
```
test.html
```html
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
<form action="/post" method="post">  我的表单提交绝对路径/post
    <input type="text" name="username"><br>  
    <input type="text" name="password"><br>  
    <input type="submit" value="发送">  
</form>  
</body>  
</html>
```

#  前端发送请求
##  先搞清楚：浏览器发请求的三种方式
1. 地址栏输入 URL
- 浏览器发 GET 请求
- 整页刷新，响应是 HTML
- 页面完全被替换
2. 表单提交 `<form>`
- 默认发 GET 或 POST
- 整页刷新或跳转
- 响应由浏览器直接展示
3. AJAX（JavaScript 发请求）
- JS 代码控制发请求
- **不刷新页面**
- 拿到响应数据后，由 JS 决定怎么更新页面

前两种是“浏览器默认行为”，第三种才是“前端主动控制”。AJAX 的本质是：

> **用 JavaScript 在后台发 HTTP 请求，拿到响应后局部更新页面。**

对应之前学的 HTTP 报文，
AJAX 一样有：
请求行、请求头、请求体、
响应行、响应头、响应体。AJAX 只是换了“谁来发”。
## ajax
现在的 AJAX 更准确叫法是：**用 JS 异步发 HTTP 请求，用 JSON 传数据。** x原本是xml
![[Pasted image 20261008222124.png|720]]

## axios：对 AJAX 的封装