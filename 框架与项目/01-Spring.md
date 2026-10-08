首先就是写入spring依赖在pom文件里
```java
<dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-context</artifactId>  
    <version>6.1.14</version>  
</dependency>
```

然后在主项目包下创建几个标准文件夹，以后在config里面配置好了主包目录了，Bean创建，也就只会扫描这里面的类

| 包名         | 层级/职责         | 常用注解/技术                                           | 示例                              |
| ---------- | ------------- | ------------------------------------------------- | ------------------------------- |
| utils      | 工具类，放通用方法     | 静态方法，必要时 `@Component`                             | `JwtUtils`、`DateUtils`          |
| pojo       | 实体类/DTO/VO    | `@Data`、`@Entity`、`@TableName`                    | `User`、`UserDTO`                |
| config     | 全局配置类         | `@Configuration`、`@Bean`                          | `WebMvcConfig`、`MybatisConfig`  |
| controller | 控制层，接收请求、返回响应 | `@RestController`、`@RequestMapping`、`@GetMapping` | `UserController`                |
| service    | 业务层，处理业务逻辑、事务 | `@Service`、`@Transactional`                       | `UserService`、`UserServiceImpl` |
| dao        | 数据访问层，操作数据库   | `@Repository`、`@Mapper`                           | `UserMapper`、`UserDao`          |




| 注解                                       | 作用                                                                                                                               | 位置              |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| @Component ( )                           | 表示为被Spring管理的类,<br>("自定义别名 ")，在getBean（）时被使用                                                                                     | Class外面，要实现的类   |
| @Configuration                           | 当前类为配置类                                                                                                                          | class外，coonfig类 |
| @ComponentScan（“项目包路径”）比如（"com.example"） | 包为被spring管理，让 Spring 扫描 `com.example` 包及其子包。<br>扫描到 `@Repository`、`@Service`、`@Component`、`@Controller` 后，Spring 会注册 Bean 并创建对象。 | class外,config类  |
| @Scope（“非单/单例”）                          | 默认singleton，prototype非单例                                                                                                         | Class外面，要实现的类   |
| @Lazy                                    | 当前实现类采用懒汉式创建                                                                                                                     | Class外面，要实现的类   |
|                                          |                                                                                                                                  |                 |
|                                          |                                                                                                                                  |                 |

# Bean创建区别
**只要一个类被 Spring 扫描到，并且加了 `@Component` 或它的衍生注解，Spring 就会把它创建成 Bean，放进 IOC 容器。**
单例（饿汉/懒汉）/非单例：

默认单例：容器启动就创建，多次 getBean 同一个对象。
@Lazy 懒汉：第一次 getBean 才创建，之后还是同一个对象。

非单例：prototype：每次 getBean 都创建新对象，容器不负责销毁。

```java
@Component  
public class UserDaoimpl implements UserDao {  
  
    @Override  
    public void show() {  
        System.out.println("这是要创建的Bash类，0000");  
    }  
}

public class Test {  
    public static void main(String[] args) {  
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);  
        Object bean = context.getBean(UserDao.class);    //通过类调取
        Object bean = context.getBean("userDaoimpl");  //具体名调取
        Object bean = context.getBean("userDaoimpl", UserDao.class);  //这个类以及它下面的具体名,主要用这个
        context.close();  
    }  
}
```

Spring管理类注解，区分不同位置
![[Pasted image 20260913112011.png]] 

==原理上这些都是component的延伸，但是实际使用还是区分使用。
不仅为了清晰标记，还有特殊功能的实现==
# Bean生命周期控制：
一定要先加入依赖
```java
<dependency>
	<groupId>jakarta.annotation</groupId>
	<artifactId>jakarta.annotation-api</artifactId>
	<version>2.0.0</version>
</dependency>

```
有创建 调用 销毁 。。。
2个注解：作用对象为实现类的具体方法，在方法名上标注，使这个方法在当前Bean类处于什么阶段时被调用
@PostConstruct  Bean在容器里创建时
@PreDestory   Bean在被销毁时  context . close( )

# DI注入依赖：：

| 注解           | 作用                                                                   | 位置                   |
| ------------ | -------------------------------------------------------------------- | -------------------- |
| @Autowired   | 注入依赖，告诉Spring 我要调用这个Bean类了                                           | class里面 方法外面 区分开其他方法 |
| @Qualifier（） | 要用具体的接口大类下的有多个实现类。要用哪些实现类，括号里面通过具体Bean类名写清楚，如果在标注创建Bean的时候用了别名，就要用别名 | 同                    |
|              |                                                                      |                      |

使不同实现Bean类之间可以互相调用对方的方法
一般情况为一个接口有多个实现方法：
比如有UserDao接口，有UserDaoimpl1  UserDaoimpl2 两个实现类
我要在UserService里也调用UserDaoimpl1的Bean以及里面的方法
 ```java
 @Service
public class UserService {
    @Autowired
    private UserDao userDao;
    @Qualifier("UserDaoimpl1")  // 不加这句就报错，因为有两个实现
    
    public static void main(String[] args){
	    UserDaoimpl1.show();
    }
}
 ```
![[Pasted image 20260915224037.png|568]]![[Pasted image 20260915224100.png|559]]

我的总结：

`@Autowired` 让 Spring 把 Bean 塞进来，不管注入点是字段、构造器还是 setter，最终目的都一样的。
遇到接口有多个实现类时，` @Qualifier` 而是多实现场景下的必需品。为了解决这种继承接口的多实现的手段，`@Autowired` 会分不清用哪个，必须加 `@Qualifier("bean名称")` 指定。  
这等同于 `context.getBean("bean名称", 接口.class)`，只是前者是**被动注入**，后者是**主动获取**。
`Eg :  UserDao userDaoimpl = context.getBean("userDaoimpl", UserDao.class);`


![[Pasted image 20260913102406.png|788]]

# AOP
**作用**：在**不惊动原始设计**的基础上为其进行功能增强。简单的说就是在不改变方法源代码的基础上对方法进行功能增强。

| 注解                        | 作用                                                        | 位置                                    |
| ------------------------- | --------------------------------------------------------- | ------------------------------------- |
| `@EnableAspectJAutoProxy` | 告诉 Spring：开启 AOP 自动代理，让 `@Aspect` 类生效，为目标 Bean 生成代理对象     | 配置类上（`@Configuration` 那个类）            |
| `@Aspect`                 | 告诉 Spring：这个类是一个切面类，里面装着切点和通知，<br>哎，我理解为对原本类（方法）的增加修补，好理解 | 切面类上，通常还要再加 `@Component` 交给 Spring 管理 |

