这两个注解最容易被误导的一句话是：

> `@RequestParam` 处理 GET，`@RequestBody` 处理 POST。

这句话**不准确**。准确的说法是：

> **`@RequestParam` 处理“参数”，`@RequestBody` 处理“请求体”。而参数可能在 URL，也可能在表单请求体里。**

它们和 GET/POST 不是一一对应的，真正决定用哪个的是：**数据放在哪里、Content-Type 是什么。**

---

# 一、先看 HTTP 请求的数据放在哪

一个 HTTP 请求可以携带数据的地方主要有三个：

1. **URL 查询字符串**
   ```http
   GET /user?name=zs&age=18
   ```

2. **请求体**
   ```http
   POST /user
   Content-Type: application/json

   {"name":"zs","age":18}
   ```

3. **路径**
   ```http
   GET /user/1
   ```

SpringMVC 对应三个注解：

| 数据位置 | 注解 |
|---|---|
| URL 查询参数 / 表单参数 | `@RequestParam` |
| 请求体 JSON | `@RequestBody` |
| 路径变量 | `@PathVariable` |

---

# 二、@RequestParam 本质

`@RequestParam` 是从 **Servlet 的 parameter map** 里取值。

Tomcat 会把下面两种数据解析成 parameter：

1. URL 查询字符串：`?name=zs&age=18`
2. `application/x-www-form-urlencoded` 表单请求体：`name=zs&age=18`

所以：

```java
@GetMapping("/get")
public String get(@RequestParam String name,
                  @RequestParam Integer age) {
    return name + "," + age;
}
```

可以接收：

```http
GET /get?name=zs&age=18
```

也可以接收：

```http
POST /form
Content-Type: application/x-www-form-urlencoded

name=zs&age=18
```

后端照样写：

```java
@PostMapping("/form")
public String form(@RequestParam String name,
                   @RequestParam Integer age) {
    return name + "," + age;
}
```

所以 `@RequestParam` **不是 GET 专属**，POST 表单也能用。

它的特点是：

- 一次绑定一个参数
- 参数名和形参名对应
- 可以设置 `required`、`defaultValue`
- 数据必须是 key-value 形式
- 不能直接读 JSON 请求体

例如：

```java
@RequestParam(value = "name", required = false, defaultValue = "张三") String name
```

---

# 三、@RequestBody 本质

`@RequestBody` 是读取 **整个请求体**，然后交给 `HttpMessageConverter` 转换。

最常见的是 JSON：

```http
POST /json
Content-Type: application/json

{"name":"zs","age":18}
```

后端：

```java
@PostMapping("/json")
public String json(@RequestBody User user) {
    return user.toString();
}
```

SpringMVC 内部做的是：

1. 判断 `Content-Type` 是 `application/json`
2. 找到 Jackson 的 `MappingJackson2HttpMessageConverter`
3. 读取请求体 InputStream
4. 用 Jackson 把 JSON 反序列化成 `User` 对象
5. 反射调用你的方法

所以 `@RequestBody` 的特点是：

- 读取整个请求体
- 依赖 `Content-Type`
- 通常用于 JSON
- 可以绑定对象、List、Map
- 一个方法只能有一个 `@RequestBody`，因为请求体只有一个
- 不能用于 `application/x-www-form-urlencoded` 表单

如果你对表单数据用 `@RequestBody`，通常会报：

```text
415 Unsupported Media Type
```

或者参数为 null。

---

# 四、它们和 GET/POST 的真正关系

看这张表就清楚了：

| 场景 | 前端发法 | Content-Type | 数据位置 | 后端注解 |
|---|---|---|---|---|
| GET 查询 | `axios.get("/user?name=zs")` | 无 | URL | `@RequestParam` |
| GET 路径参数 | `axios.get("/user/1")` | 无 | 路径 | `@PathVariable` |
| POST 表单 | `axios.post("/user", "name=zs&age=18")` | `application/x-www-form-urlencoded` | 请求体 | `@RequestParam` |
| POST JSON | `axios.post("/user", {name:"zs", age:18})` | `application/json` | 请求体 | `@RequestBody` |
| PUT JSON | `axios.put("/user", {id:1, name:"zs"})` | `application/json` | 请求体 | `@RequestBody` |
| DELETE 路径 | `axios.delete("/user/1")` | 无 | 路径 | `@PathVariable` |

所以：

- GET 通常用 `@RequestParam` 或 `@PathVariable`
- POST 表单用 `@RequestParam`
- POST/PUT JSON 用 `@RequestBody`

不是“GET 用 RequestParam，POST 用 RequestBody”，而是：

> **URL 参数和表单参数用 `@RequestParam`，JSON 请求体用 `@RequestBody`。**

---

# 五、为什么设计成这样？

## 1. HTTP 协议本身的分工

GET 的语义是“获取资源”。参数放在 URL 里，表示“我要获取哪个资源”。

```http
GET /users?page=1&size=10
```

POST 的语义是“提交数据”。数据放在请求体里，表示“我要提交这些内容”。

```http
POST /users
Content-Type: application/json

{"name":"张三","age":18}
```

URL 是用来定位资源的，请求体是用来携带数据的。SpringMVC 的注解就是尊重这个分工。

## 2. Servlet 容器的解析能力不同

Tomcat 能自动把：

- URL 查询字符串
- `application/x-www-form-urlencoded` 表单

解析成 `request.getParameter("name")`。

所以 `@RequestParam` 底层就是：

```java
request.getParameter("name")
```

但 Tomcat **不会**自动把 JSON 解析成参数。

JSON 对 Tomcat 来说只是一段输入流。必须由框架读取并转换。

所以 `@RequestBody` 底层是：

```java
InputStream in = request.getInputStream();
User user = objectMapper.readValue(in, User.class);
```

这就是为什么表单用 `@RequestParam`，JSON 用 `@RequestBody`。

## 3. 前端 axios 的写法决定了 Content-Type

```js
axios.post("/user", "name=zs&age=18");
```

axios 默认发：

```http
Content-Type: application/x-www-form-urlencoded
```

后端用 `@RequestParam`。

```js
axios.post("/user", { name: "zs", age: 18 });
```

axios 默认发：

```http
Content-Type: application/json
```

后端用 `@RequestBody`。

所以前后端必须约定好：

- 前端怎么发
- 后端怎么收

否则就是 400、415 或参数为 null。

---

# 六、GET 和 POST 的区别，为什么要这样

| 对比 | GET | POST |
|---|---|---|
| 语义 | 查询/获取 | 提交/创建 |
| 参数位置 | URL | 请求体 |
| 是否幂等 | 是 | 否 |
| 是否安全 | 是（不修改数据） | 否 |
| 能否缓存 | 能 | 通常不能 |
| 长度限制 | 浏览器/服务器有限制 | 基本无限制 |
| 书签/历史 | 可以 | 不可以 |
| 安全性 | 参数暴露在 URL | 相对好一点，但也不加密 |

设计原因：

- GET 用于“读”，所以参数放 URL，方便缓存、书签、分享。
- POST 用于“写”，所以数据放 body，适合大量数据、文件、敏感信息。
- 这不是 SpringMVC 规定的，是 HTTP 协议和浏览器长期约定。

注意：HTTP 规范并没有绝对禁止 GET 带 body，但浏览器、axios、表单默认都不带，服务器也经常忽略。所以实际开发就记：

> GET 参数在 URL，POST 参数在 body。

---

# 七、最容易踩的坑

## 1. 前端发 JSON，后端用 @RequestParam

```js
axios.post("/user", { name: "zs" });
```

```java
@PostMapping("/user")
public String add(@RequestParam String name) { ... }
```

结果：`name` 为 null，或者 400。

因为 JSON 不在 parameter map 里。

## 2. 前端发表单，后端用 @RequestBody

```js
axios.post("/user", "name=zs&age=18");
```

```java
@PostMapping("/user")
public String add(@RequestBody User user) { ... }
```

结果：415 或参数为空。

因为 `@RequestBody` 默认需要 JSON 转换器。

## 3. 一个方法写两个 @RequestBody

```java
public String add(@RequestBody User user, @RequestBody Address address)
```

报错。请求体只有一个，只能读一次。

需要多个对象就包一个 DTO：

```java
public class UserDTO {
    private User user;
    private Address address;
}
```

## 4. GET 请求想用 @RequestBody

技术上可能能读到，但浏览器和 axios 默认不发 body，所以实际不可用。

GET 就用 `@RequestParam` 或 `@PathVariable`。

---

# 八、一句话总结

- `@RequestParam`：从 URL 查询参数或表单参数中取值，底层是 `request.getParameter()`。
- `@RequestBody`：读取整个请求体，用 Jackson 把 JSON 转成对象。
- GET 通常用 `@RequestParam` / `@PathVariable`。
- POST 表单用 `@RequestParam`。
- POST/PUT JSON 用 `@RequestBody`。
- 决定用哪个的不是 GET/POST，而是 **数据在 URL、表单还是 JSON 请求体**。
- 前端 axios 的 `data` 写法决定 Content-Type，Content-Type 决定后端用哪个注解。

记住这条链：

```text
前端怎么发
→ Content-Type
→ 数据在 URL 还是 body
→ 后端用 @RequestParam 还是 @RequestBody
```

这样你就不会再把它们和 GET/POST 简单绑死了。