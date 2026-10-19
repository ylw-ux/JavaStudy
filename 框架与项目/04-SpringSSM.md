# cookie
前端会话信息保存技术
```java
@RestController  
@RequestMapping("cookie")  
public class CookieTest1 {  
  
    @GetMapping("/addcookie")  
    public String responsecookie(HttpServletResponse response){  
        Cookie cookie = new Cookie("name", "lihau");  
        response.addCookie(cookie);  
        return "success";  
    }  
  
    @GetMapping("getcookie")  
    public void requestcookie(HttpServletRequest request){  
        Cookie[] cookies = request.getCookies();  
        for(Cookie c :cookies){  
            System.out.println("cookie = " + c);  
            String name = c.getName();  
            String value = c.getValue();  
            System.out.println("name,value = "+name+value);  
        }  
    }  
}
```

cookie广泛运用的还有设置消失时间（生命周期）,单位时间是一秒
```java
@GetMapping("/addcookie")  
    public String responsecookie(HttpServletResponse response){  
        Cookie cookie = new Cookie("name", "lihau");
        cookie.setMaxAge(7*24*3600);   
        response.addCookie(cookie);  
        return "success";  
    }  
```
设置了7天后cookie自动消失，比如网站7天就要你重新登录一次 
# session
1. Session是存储在服务端 而Cookie是存储在客户端
2. Session是基于Cookie的
3. Session的生命周期一般就是你一次打开网页进行什么操作的时间，关闭网页全部清除，无操作默认30分钟
4. Session实现的是一次会话中的多次请求之间的数据共享，比如你点击主页，又要设置个人信息，2者都要用到你的所有信息对象，2者每次都要去数据库请求一次，服务端设置存储Session，不用每一次都要重新请求了
5. 存储在客户端的数据容易被窃取和截获，存储在服务端的数据更安全。
基础session对象：HttpSession session
存储session：setAttribute(String key, Object o);
根据 key，获取对象，删除对象：
Object getAttribute(String key)，
void removeAttribute(String key)

 ```java
@RestController  
@RequestMapping("session")  
public class session {  
  
    @RequestMapping("setsession")  
    public String setsession(HttpSession session){  
        StudentDTO studentDTO =  
                new StudentDTO("name", "123456", true);  
        session.setAttribute("student",studentDTO);  
        return"sussess";  
    }  
    @RequestMapping("getsession")  
    public StudentDTO getsession(HttpSession session){  
        StudentDTO studentDTO = (StudentDTO) session.getAttribute("student");  
        if(studentDTO != null){  
                System.out.println(studentDTO);  
                return studentDTO;  
        }else{  
            System.out.println("no session object");  
            return null;  
        }  
    }  
}
 ```
 登录的cookie设置以及登陆后的session设置
 ```java
 @PostMapping("/login")
    public String login(@RequestBody Student student,
    HttpServletResponse response,
    HttpSession session) {
        System.out.println("student = " + student);
        Student u = studentService.login(student);
        if (u != null) {
            if (Boolean.TRUE.equals(student.getRemember())) {
                Cookie c1 = new Cookie("username", student.getUsername());
                Cookie c2 = new Cookie("password", student.getPassword());
                c1.setMaxAge(60 * 60 * 24 * 7);
                c2.setMaxAge(60 * 60 * 24 * 7);
                c1.setPath("/");
                c2.setPath("/");
                response.addCookie(c1);
                response.addCookie(c2);
                System.out.println("存入到cookie中");
                session.setAttribute("student", u);
                return "success";
            }
            session.setAttribute("student", u);
            return "success";
        }
        return "error";
    }
 ```
 
 ```txt
 用户提交登录
  → 查库验证（这一次必须查）
  → 失败：返回 error，什么都不写
  → 成功：
      → 写 session（本次会话免查库）
      → 若勾了记住我：写 Cookie（下次免登录）
 ```

**把密码明文存进 Cookie 是绝对不能做的。** 原因：
1. **Cookie 存在用户浏览器本地**，用户自己、任何能访问该电脑的人、任何 XSS 脚本都能直接看到明文密码。
2. **Cookie 随每个请求明文传输**（除非全站 HTTPS，且即使 HTTPS，本地存储仍暴露）。
3. **Cookie 可被篡改**，攻击者可以改用户名去撞别人账号。
4. 一旦泄露，用户在所有平台的同一密码都遭殃。
## 正确的"记住我"做法
不应该存密码，而应该存一个**不可逆的、可吊销的令牌**：
```txt
登录成功 + 勾选记住我
  → 生成随机 token（如 UUID / SecureRandom）
  → 服务端存 token → userId 的映射（DB 或 Redis），设过期时间
  → 只把 token 写进 Cookie（HttpOnly + Secure + SameSite）
下次请求
  → 读 Cookie 里的 token
  → 服务端查 token 对应的 userId，验证未过期未吊销
  → 恢复登录态
退出登录
  → 删除服务端 token，清除 Cookie
```

查询所有
pageinfo 多表查询 要加依赖


Code类
Result类--- code data msg
统一结果集--状态码Integer code 数据Object data 信息 String msg
# 统一结果集
前端req具体内容分类
 ```java
 @Data
public class Result<T> {
    private Integer code;
    private String msg;
    private T data; // 注意这里是 data
}
 @Data
public class User {
    private Integer id;
    private String name;
}
 ```
```json
res对象 ( 前端res接收的response结果)= {
    status: 200,           // HTTP 状态码
    statusText: "OK",      // 状态描述
    headers: {...},        // 【纠正】响应头 (拼写是 headers)
    config: {...},         // 请求配置 (baseURL, method 等)
	data( res.data) (对应后端return的统一结果集 Result类，axios框架处理的固定格式，代表http响应体) ={
		code/*Result里的code变量*/=
		msg/*msg变量*/=
		data /*泛型T->User 类*/= { // 前端怎么写，完全看后端 `Result` 类里的属性名。
			id/*User类里的变量*/=
			name =
		}
	}
}
```

Objicts.isNUll(对应目标) 判断是否为null

### 为什么后端要多此一举包一层 Result？

直接返回 `User` 对象不行吗？行，但不规范。  
包装 `Result` 的好处：
1. **统一状态码**：HTTP 状态码（200, 404, 500）只能表示网络请求的状态，无法表示业务状态（比如“密码错误”、“余额不足”）。所以后端用 `Result.code` 来表示业务状态码。
2. **统一提示信息**：`Result.msg` 可以携带“登录成功”、“库存不足”等提示，前端直接弹窗显示。
3. **数据防篡改**：把真正的数据藏在 `Result.data` 里，结构更清晰。
# 异常处理器 
后端原本遇到异常可以throws，throw，try-catch etc.
但是要对每个代码逻辑都加上异常处理会很麻烦，既然错误会抛异常，那我对这些异常进行统一管理，只要抛了对应的异常，就对应到提前写好的处理上去，这样，无论那个文件，那段代码抛的异常都可以捕获处理了，还不麻烦。

目录软件包--创建exception包

@RestControllerAdvice 指定当前类为异常处理器

@ExceptionHandler （ 目标异常类）指定要捕获的异常
public Result getException（目标异常类 形参） {
sout("had catch exception")
return new Result（code:9000,data:null,msg:形参.getMessage()）
}
 ```java
 @RestControllerAdvice //用于标识当前类为REST风格对应的异常处理器  
public class ExceptionsManage {  
	//统一处理所有的Exception异常的处理器  
	@ExceptionHandler(Exception.class)  
	public Result doOtherException(Exception ex){  
	    return new Result(5000,ex.getMessage(),"异常被捕获！");  
	}  
	//这个是处理索引越界的异常处理器  
	@ExceptionHandler(IndexOutOfBoundsException.class)  
	public Result doIndexOutOfBoundsException(IndexOutOfBoundsException idx){  
	    System.out.println("IndexOutOfBoundsException");  
	    return new Result(5001,idx.getMessage(),"index越界");  
	}
	//其他异常处理器...
}
 ```

# 拦截器
创建拦截器具体怎么做，判断什么，
return false表示不放行，true表示放行
 ```java
 @Component//被Spring管理  
  
//引入接口,定义拦截器  
public class Interceptor implements HandlerInterceptor {  
    //重写方法  
    @Override//方法调用前  
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {  
        System.out.println("请求前拦截判断");  
        HttpSession session = request.getSession();  
        Object student = session.getAttribute("Student");  
        if(Objects.isNull(student)){  
            System.out.println("请先登录 ");  
             return false;   
        }else{  
            return true;  
        }  
    }  
  
    @Override //方法调用开始时  
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {  
        HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);  
    }  
  
    @Override//方法调用完成后  
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {  
        HandlerInterceptor.super.afterCompletion(request, response, handler, ex);  
    }  
}
 ```
config配置拦截器具体拦截什么
 ```java
 @Configuration  
public class IntercepterConfig implements WebMvcConfigurer {  
    @Autowired  
    public Interceptor interceptor;  
  
    @Override  
    public void addInterceptors(InterceptorRegistry registry) {  
        WebMvcConfigurer.super.addInterceptors(registry);  
        registry.addInterceptor(interceptor).addPathPatterns("/student/login");  
    }  
}
 ```

多拦截器的执行顺序（执行前，中，后）：
![[Pasted image 20261019104736.png]]
以3个前中后都包含的为例：
前1，前2，前3--->方法开始--->中3，中2，中1--->方法结束--->后3，后2，后1
		前3被拦截--->后2，后1
	前2被拦截--->后1
 前1被拦截--->直接结束

为什么：
前3被拦截，3的进程就被中断了，都没有进入到方法里，‘中’要在方法里才会调用，所以被跳过，直接进入‘后’，后3属于3，前3就被中断了，不执行，执行后2，后1
前2前1同理