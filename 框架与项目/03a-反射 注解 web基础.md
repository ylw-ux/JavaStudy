搞懂三件事：
1. **反射**：框架怎么在运行时创建对象、调用方法。
2. **注解**：框架怎么知道你写了 `@Controller`、`@RequestMapping`。
3. **HTTP/Web**：请求和响应到底长什么样，SpringMVC 在中间做了什么。
# 一、反射的本质：框架的“运行时操作能力”

## 1. 反射是什么？

正常写代码：

```java
User user = new User();
user.setName("张三");
```

这是**编译时**就知道要创建 `User`，调用 `setName`。

反射是：

```java
Class<?> clazz = Class.forName("com.example.User");
Object user = clazz.getDeclaredConstructor().newInstance();
Method method = clazz.getMethod("setName", String.class);
method.invoke(user, "张三");
```

这是**运行时**才知道类名、方法名，然后动态创建、动态调用。

一句话：

> 反射让程序在运行时，能够查看类的信息，并创建对象、调用方法、访问字段。

## 2. Class 对象是什么？

Java 里每个类被加载后，JVM 都会为它创建一个 `Class` 对象。这个对象代表“这个类本身”。

获取 Class 的三种方式：

```java
Class<?> c1 = User.class;
Class<?> c2 = new User().getClass();
Class<?> c3 = Class.forName("com.example.User");
```

`Class` 对象里有什么？

- 类名、包名、父类、接口
- 构造器 `Constructor`
- 方法 `Method`
- 字段 `Field`
- 注解 `Annotation`

## 3. 反射核心 API

```java
Class<?> clazz = User.class;

// 创建对象
User user = (User) clazz.getDeclaredConstructor().newInstance();

// 获取方法
Method setName = clazz.getMethod("setName", String.class);
setName.invoke(user, "张三");

// 获取字段
Field name = clazz.getDeclaredField("name");
name.setAccessible(true);
name.set(user, "李四");

// 获取注解
if (clazz.isAnnotationPresent(Controller.class)) {
    Controller controller = clazz.getAnnotation(Controller.class);
}
```

注意：

- `getMethod` 只能获取 public 方法，包括继承的。
- `getDeclaredMethod` 获取本类所有方法，包括 private。
- 私有成员要 `setAccessible(true)` 才能访问。
- `Method.invoke` 就是反射调用方法。

## 4. 为什么框架离不开反射？

Spring 启动时：

- 扫描包，找到 `@Controller`、`@Service`、`@Component`
- 用反射创建这些类的对象，放进 Spring 容器
- 用反射给 `@Autowired` 字段赋值
- 解析 `@RequestMapping`，把 URL 和 `Method` 存起来
- 请求来了，用反射调用 Controller 方法

MyBatis：

- 查数据库返回 `ResultSet`
- 用反射创建实体对象
- 用反射调用 `setXxx` 或直接设置字段

所以：

> 没有反射，就没有 Spring、SpringMVC、MyBatis 这些框架。

---

# 二、注解的本质：注解是接口，反射是读取器

## 1. 注解不是魔法

你写：

```java
@Controller
public class UserController {
}
```

`@Controller` 本质是一个接口：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Controller {
}
```

编译后，它就是一个继承 `java.lang.annotation.Annotation` 的接口。

JVM 在运行时，会为注解生成一个代理对象。你通过反射拿到这个代理对象，调用它的方法，就能拿到注解属性值。

例如：

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyRequestMapping {
    String value();
    String method() default "GET";
}
```

使用：

```java
@MyRequestMapping(value = "/users", method = "POST")
public String add() {
    return "ok";
}
```

反射读取：

```java
Method method = UserController.class.getMethod("add");
MyRequestMapping ann = method.getAnnotation(MyRequestMapping.class);
System.out.println(ann.value());   // /users
System.out.println(ann.method());  // POST
```

## 2. 元注解：注解的注解

最重要的两个：

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
```

### @Retention

表示注解保留到什么时候：

| 值 | 含义 |
|---|---|
| SOURCE | 只在源码，编译后丢掉 |
| CLASS | 保留到 class 文件，运行时拿不到 |
| RUNTIME | 运行时还能通过反射拿到 |

SpringMVC 的注解基本都是 `RUNTIME`，否则框架读不到。

### @Target

表示注解能写在什么地方：

- `TYPE`：类、接口
- `METHOD`：方法
- `FIELD`：字段
- `PARAMETER`：方法参数
- `CONSTRUCTOR`：构造器

`@Controller` 是 `TYPE`，`@RequestMapping` 可以是 `TYPE` 和 `METHOD`，`@RequestParam` 是 `PARAMETER`。

## 3. 注解 + 反射 = 框架配置

框架的工作模式：

1. 定义注解，例如 `@Controller`
2. 你写代码时加上注解
3. 框架启动时扫描类
4. 用反射判断：这个类有没有 `@Controller`
5. 有，就创建对象
6. 再看方法有没有 `@RequestMapping`
7. 有，就把 URL 和 `Method` 存进 Map
8. 请求来了，根据 URL 找到 `Method`
9. 反射调用这个方法

这就是 SpringMVC 的核心。

---

# 三、写一个迷你 SpringMVC：理解反射和注解

我们定义三个注解：

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface MyController {
}
```

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD})
public @interface MyRequestMapping {
    String value();
}
```

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.PARAMETER)
public @interface MyRequestParam {
    String value();
}
```

写一个 Controller：

```java
@MyController
public class UserController {

    @MyRequestMapping("/user/save")
    public String save(@MyRequestParam("name") String name,
                       @MyRequestParam("age") Integer age) {
        return "save: " + name + ", " + age;
    }
}
```

迷你 MVC 容器：

```java
public class MiniMvc {
    private final Map<String, Object> beans = new HashMap<>();
    private final Map<String, Method> mappings = new HashMap<>();

    public void scan(String packageName) throws Exception {
        String path = packageName.replace('.', '/');
        URL url = Thread.currentThread().getContextClassLoader().getResource(path);
        File dir = new File(url.toURI());

        for (File file : dir.listFiles()) {
            if (!file.getName().endsWith(".class")) continue;

            String className = packageName + "." + file.getName().replace(".class", "");
            Class<?> clazz = Class.forName(className);

            if (!clazz.isAnnotationPresent(MyController.class)) continue;

            Object bean = clazz.getDeclaredConstructor().newInstance();
            beans.put(clazz.getName(), bean);

            for (Method method : clazz.getDeclaredMethods()) {
                MyRequestMapping ann = method.getAnnotation(MyRequestMapping.class);
                if (ann != null) {
                    mappings.put(ann.value(), method);
                }
            }
        }
    }

    public String invoke(String path, Map<String, String> params) throws Exception {
        Method method = mappings.get(path);
        if (method == null) return "404";

        Object bean = beans.get(method.getDeclaringClass().getName());
        Parameter[] parameters = method.getParameters();
        Object[] args = new Object[parameters.length];

        for (int i = 0; i < parameters.length; i++) {
            MyRequestParam ann = parameters[i].getAnnotation(MyRequestParam.class);
            if (ann != null) {
                String value = params.get(ann.value());
                if (parameters[i].getType() == Integer.class) {
                    args[i] = Integer.valueOf(value);
                } else {
                    args[i] = value;
                }
            }
        }

        Object result = method.invoke(bean, args);
        return String.valueOf(result);
    }
}
```

测试：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        MiniMvc mvc = new MiniMvc();
        mvc.scan("com.example.controller");

        Map<String, String> params = new HashMap<>();
        params.put("name", "张三");
        params.put("age", "18");

        String result = mvc.invoke("/user/save", params);
        System.out.println(result);
    }
}
```

输出：

```text
save: 张三, 18
```

这就是 SpringMVC 的雏形：

- `@MyController` 相当于 `@Controller`
- `@MyRequestMapping` 相当于 `@RequestMapping`
- `@MyRequestParam` 相当于 `@RequestParam`
- `scan` 相当于 Spring 包扫描
- `mappings` 相当于 HandlerMapping
- `invoke` 相当于 HandlerAdapter + 参数解析 + 反射调用

你把这个迷你 MVC 写一遍，SpringMVC 就不神秘了。

---

# 四、HTTP/Web 基础，不要背，要观察

你之前觉得响应头很虚，是因为你只看文字。正确学法是：**发请求，看报文，改代码，再看报文**。

## 1. HTTP 请求长什么样
**请求数据分为3部分：**

**请求行**：请求数据的第一行。其中GET表示请求方式，/表示请求资源路径，HTTP/1.1表示协议版本
**请求头**：第二行开始，格式为key：value形式。
**请求体**： POST请求的最后一部分，存放请求参数（载荷中的表单数据）

```http
GET /user/save?name=zhangsan&age=18 HTTP/1.1
Host: localhost:8080
User-Agent: curl/8.0
Accept: */*
```

- 请求行：`GET /user/save?name=zhangsan&age=18 HTTP/1.1`
- 请求头：`Host`、`User-Agent`、`Accept`
```
Host: 表示请求的主机名
User-Agent: 浏览器版本，例如Chrome浏览器的标识类似Mozilla/5.0 ... Chrome/79，IE浏览器的标识类
Mozilla/5.0 (Windows NT ...) like Gecko；
Accept：表示浏览器能接收的资源类型，如text/_，image/_或者_/_表示所有；
Accept-Language：表示浏览器偏好的语言，服务器可以据此返回不同语言的网页；
Accept-Encoding：表示浏览器可以支持的压缩类型，例如gzip, deflate等。
```
- 请求体：GET 通常没有

POST 表单：

```http
POST /user/save HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-urlencoded
Content-Length: 21

name=zhangsan&age=18
```

POST JSON：

```http
POST /user/save HTTP/1.1
Host: localhost:8080
Content-Type: application/json
Content-Length: 30

{"name":"zhangsan","age":18}
```

关键区别不在 GET/POST，而在：

- 参数放 URL 还是 body
- `Content-Type` 是什么

## 2. HTTP 响应长什么样

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 27

{"name":"张三","age":18}
```

三部分：

- 响应行：`HTTP/1.1 200 OK` 响应数据的第一行。其中HTTP/1.1表示协议版本，200表示响应状态码，OK表示状态码描述
- 响应头：`Content-Type`、`Content-Length` 第二行开始，格式为key：value形式
- 响应体：`{"name":"张三","age":18}` 最后一部分。存放响应数据

SpringMVC 帮你做的就是：

- 你的方法返回对象
- `@ResponseBody` 告诉 SpringMVC：不要找页面，把返回值写进响应体
- Jackson 把对象转成 JSON
- 设置 `Content-Type: application/json`
- 写回浏览器

## 3. 用 curl 看原始报文

```bash
curl -i http://localhost:8080/user/json
```

`-i` 会显示响应头。

```bash
curl -i -X POST http://localhost:8080/user/save \
  -H "Content-Type: application/json" \
  -d '{"name":"张三","age":18}'
```

你会看到：

```http
HTTP/1.1 200
Content-Type: application/json
...

{"code":200,"msg":"ok"}
```

比背一百遍有用。

## 4. 常见 Content-Type

| Content-Type | 含义 | 后端常用注解 |
|---|---|---|
| `application/x-www-form-urlencoded` | 表单 | `@RequestParam` |
| `multipart/form-data` | 表单 + 文件 | `@RequestParam`、`MultipartFile` |
| `application/json` | JSON | `@RequestBody` |
| `text/html` | HTML | `@Controller` 返回视图 |
| `text/plain` | 纯文本 | `@ResponseBody` 返回 String |

## 5. 状态码不用背，记分类

- 2xx：成功。200 OK，201 Created，204 No Content
- 3xx：重定向。302 临时，304 缓存
- 4xx：客户端错。400 参数错，401 未登录，403 无权限，404 找不到，405 方法不允许，415 媒体类型不支持
- 5xx：服务端错。500 服务器异常

写接口时遇到再查。

## 6. Cookie/Session 为什么存在

HTTP 是无状态的，每次请求独立。服务器不知道两次请求是不是同一个人。

解决：

- 服务器返回 `Set-Cookie: JSESSIONID=xxx`
- 浏览器下次请求自动带 `Cookie: JSESSIONID=xxx`
- 服务器根据 JSESSIONID 找到对应 Session

SpringMVC 里：

```java
@GetMapping("/login")
public String login(HttpSession session) {
    session.setAttribute("user", "张三");
    return "ok";
}

@GetMapping("/me")
public String me(HttpSession session) {
    return (String) session.getAttribute("user");
}
```

响应头里会出现 `Set-Cookie`，请求头里会出现 `Cookie`。

---

# 五、把反射、注解、HTTP、SpringMVC 串起来

现在看 SpringMVC 启动和请求过程：

## 启动时

1. Spring 扫描包。
2. 用反射判断类上有没有 `@Controller`、`@RestController`。
3. 有，就反射创建对象，放进容器。
4. 用反射遍历方法，看有没有 `@RequestMapping`、`@GetMapping`。
5. 把 URL、请求方式、`Method` 对象存进 `HandlerMapping`。

## 请求来时

1. Tomcat 收到 HTTP 请求，解析成 `HttpServletRequest`。
2. `DispatcherServlet` 拿到请求路径。
3. 根据路径去 `HandlerMapping` 找对应 `Method`。
4. 找到后，用 `HandlerAdapter` 处理。
5. 遍历 `Method` 的参数：
   - 有 `@RequestParam`，就从 URL 或表单取值。
   - 有 `@PathVariable`，就从路径取值。
   - 有 `@RequestBody`，就用 Jackson 把 JSON 转成对象。
6. 用反射 `method.invoke(bean, args)` 调用你的 Controller。
7. 返回值如果是对象，且类上有 `@RestController` 或方法有 `@ResponseBody`，就用 Jackson 转 JSON。
8. 设置响应头 `Content-Type: application/json`，写响应体。
9. Tomcat 把响应变成 HTTP 响应报文发给浏览器。

所以：

> `@Controller`、`@RequestMapping`、`@RequestParam`、`@RequestBody`、`@ResponseBody` 都不是魔法，全是“注解 + 反射 + HTTP 报文”的组合。

---

# 六、你现在该怎么练

1. **写迷你 MVC**  
   把上面的 `MiniMvc` 自己敲一遍，跑通。

2. **写一个自定义注解 + 反射读取**  
   例如 `@MyLog`，在方法上标记，反射读取并打印日志。

3. **用 curl 看 SpringMVC 响应**  
   写三个接口：
   - 返回 String
   - 返回对象
   - 返回 `ResponseEntity`
   分别看 `Content-Type`、状态码、响应体。

4. **用 F12 看 axios 请求**  
   发 GET、POST 表单、POST JSON，看 Request Headers、Payload、Response Headers。

5. **手写一个 DispatcherServlet 思路图**  
   不写完整代码，只画：
   `请求 -> HandlerMapping -> Method -> 参数解析 -> 反射调用 -> 返回值处理 -> 响应`

做完这些，你再回去看 SpringMVC 笔记，会发现它只是把迷你 MVC 做成了工业级版本。

下一步你可以继续学 SpringMVC 的参数绑定和响应，但这次不要背注解，而是每看到一个注解，就问：

- 它保留到运行时吗？
- 它写在类、方法还是参数上？
- 框架用反射怎么读到它？
- 它最终从 HTTP 请求的哪一部分取值？
- 它最终写到 HTTP 响应的哪一部分？

这样学，响应头和响应数据就不虚了。