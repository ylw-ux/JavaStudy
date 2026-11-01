依赖导入
```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
```

# 主要注解：
- @Configuration  认证类
- @EnableWebSecurity 
- @EnableMethodSecurity
	- 方法级鉴权的开关。开了它，@PreAuthorize / @PostAuthorize / @PreFilter 才会被解析。不开会怎样：注解被完全忽略，不报错、不提示，你的接口谁都能访问——静默失效，最难查的那种 bug。来历：旧版叫 @EnableGlobalMethodSecurity(prePostEnabled = true)，6.x 改名并默认开启 prePost。放哪：任意一个 @Configuration 类头上。
- @Slf4j
- @PreAuthorize("hasAuthority('admin:select')")
	- 方法执行前做检查。括号里是 SpEL 表达式，返回 false 就抛 AccessDeniedException → 403。
	- 数据从哪来：SecurityContextHolder 里的 Authentication.getAuthorities()——也就是你 LoginUser.getAuthorities() 返回的那些字符串。

# 主要接口以及参数：
1. AuthenticationManager 认证管理
2. UserDetailsService 官方用户认证接口，返回UserDetails信息
3. UserDetails 官方返回体格式接口 （7 个方法，其中 4 个是 default，所以只写 3 个也能编译）
4. Serializable
5. OncePerRequestFilter 用在令牌过滤
	1. 在config里加了 addFilterBefore 后，这个 filter 会被注册两次（Boot 的普通过滤器 + 安全链）。OncePerRequestFilter 会在 request 上打一个标记，嵌套的第二圈发现标记还在就直接跳过 → 只会执行一次。

6. HttpServletRequest request,  
7. HttpServletResponse response,  
8. FilterChain filterChain 
	1. 在自定义过滤器里面用，作为doFilterInternal（FilterChain filterChain ）参数，
	2. 放行方法： filterChain.doFilter(request, response); 继续往下走

```
 Tomcat 的管道（FilterChain）每请求跑一遍：
   AuthFilter（验令牌，把人塞进 SecurityContext）         ← 你写的过滤器
      ↓ doFilter
   AuthorizationFilter（要不要登录）                      ← 官方
      ↓ doFilter
   DispatcherServlet → Controller
      ↓ 进入方法前
   @PreAuthorize 检查（有没有权限，需要 @EnableMethodSecurity）← 官方 + 你写的注解
```

# 主要类：
	用户 角色 权限
	User Role Permission

# 如何分工：

controller：
- 第一次接受login
- 每个角色对应权限对应方法/资源

configuration类：
- 配置过滤逻辑。密码怎么比对，要注入filter，也就是我自定义的过滤类
- 哪些请求可以放行，哪些请求可以通过

返回类：
- LoginUser，implements UserDetails：重写方法返回User Role Permission，
- ResponseResult， implements Serializable ：自定义返回结果集{code，msg，data}，对应前端就是res->data的一整个data就是它
	
authentication:
- 认证（不是过滤器，只有一次，第一次登陆时，除非后面已认证的资源被清除了）：根据前端request调库查角色，查对应权限，封装到LoginUser对象

filter：
- 令牌过滤器：获取前端request请求头中的令牌，并解析，在resis里查询具体数据，最后封装到SecurityContextHolder
	- SecurityContextHolder：是 ThreadLocal：一个线程一份，Tomcat 一线程一请求，天然隔离
	- JWT 没告诉我这个人有什么权限，但 Redis 里的 LoginUser 有 → 我把 LoginUser 包装成 Authentication 塞进 SecurityContext → 后面的人就都能问了。

Service：
- 根据前端获取它所讲的用户信息，但不知道对不对，封装为Token(UsernamePasswordAuthenticationToken)，交给AuthenticationManager 认证管理，框架自动去调用UserDetailsService 官方用户认证接口，也就是认证过滤filter。根据返回结果判断是否认证成功
	- 成功就把用户详细信息存入Redis，同时封装JWT令牌，作为ResponseResult的data返回
		- 具体如何封装看前端一般为：
```
map1:["token",jwt |]  
				  ->jwt:["token",key |]  
									  ->"Login::id" (Redis的key)
//前端res-->data-->data-->token:jwt  
//res就是response,res.date就是这个返回的响应结果集，也就是responseResult,  
// 然后这个里面的点date也就是res.data.data就是我这个封装的map1
```
mapper：
- 过滤器调用查询数据库

| 动作                        | 什么时候发生     | 发生几次     |
| ------------------------- | ---------- | -------- |
| 认证（authentication）验证"你是谁" | 只有登录那一次    | 每个会话 1 次 |
| 过滤（filter）管道上的关卡          | 每个 HTTP 请求 | 每个请求 1 次 |
怎么一眼分辨？看方法签名
```java
// 过滤器：有 request、response、chain 三件套
protected void doFilterInternal(HttpServletRequest request,
                                HttpServletResponse response,
                                FilterChain filterChain)

// UserDetailsService：只有一个用户名，没有 request，没有 chain
UserDetails loadUserByUsername(String username)

每个请求（不管登录没登录）：
   Tomcat → FilterChainProxy → AuthFilter.doFilter(request, response, chain)
                                    ↑ 这是过滤器，你写的

登录那一次：
   AuthServiceImpl.login() → authenticationManager.authenticate()
       → DaoAuthenticationProvider → 你的 loadUserByUsername(username)
                                          ↑ 这是"回调"，不是你调的，也不是过滤器
```
什么叫**"回调"**
你从没写过 new MyUserDetailServiceimpl().loadUserByUsername(...)，但它却执行了。因为：框架定好了流程，中间留了一个空位（接口），到那一步就反过来调用你的实现。


# 全流程：
1. 前端request携带用户账号和密码到后端，第一次先用户认证，
2. 包装Token给认证管理框架自动去查库，成功就返回UserDetails，失败就拦截
3. 成功后就存入Redis，下一次就不用查了，封装令牌签名，返回给前端
4. 前端把response携带令牌保存，
5. 之后每个请求：带 token → AuthFilter 验签 → 拿 key 查 Redis → 成功就放行，塞 SecurityContext
6. 带着用户具体信息进入下一层，继续走 AuthorizationFilter 判"要不要登录" → @PreAuthorize 判"有没有权限" → Controller

![[Pasted image 20261026194109.png|720]]