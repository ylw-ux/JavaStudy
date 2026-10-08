
SpringBoot是由Pivotal团队提供的全新框架，其设计目的是**简化Spring应用的初始搭建以及开发过程**
**特点：**
(1)快速开发spring应用的框架
(2)内嵌Tomcat, Jetty不需要单独安装容器，不需要部署，jar包直接发布一个web应用
(3)简化maven配置，parent这种方式，一站式引入需要的各种依赖 
(4)基于注解的零配置思想，尽可能自动配置spring应用 
(5)和各种流行框架，spring web mvc，mybatis，spring cloud无缝整合 
(6)提供生产指标,健壮检查和外部化配置

# 构建 用原生Maven构建
## springboot工程搭建步骤
1.基于maven创建项目
2.检查配置 编码格式 maven配置 jdk版本
3.导入父级依赖
4.导入所需的子依赖
5.定义项目启动类
6.定义核心配置文件
![[Pasted image 20260919151949.png|434]]
## pom导入 parent  和 dependency
```xml
<parent>  
    <!--从父亲那里继承过来springboot启动信息-->  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-parent</artifactId>  
    <version>3.3.3</version>  
</parent>

<groupId>...
<artifactId>...
<version>...
  
<properties>...

<dependencies>  
    <!--整合的web坐标 内置了tomcat服务器 无需再次配置-->  
    <dependency>  
        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter-web</artifactId>  
    </dependency>  
</dependencies>
```
## 创建项目主包 创建Appliction.class
	@SpringBootApplication
		SpringBootApplication.run(class，args)

```java
package com.itgaohe;  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
  
//当前类为项目启动类  
//自动扫描当前包所在的上一级的目录(itgaohe)下面所有的文件
  
@SpringBootApplication  
public class Application {  
    public static void main(String[] args) {  
        SpringApplication.run(Application.class,args);  
    }  
}
```

resources 创建 application.yml 
	server post:端口号
## 扩展
	SpringBootweb默认是采用了Tomcat服务器，Jetty比Tomcat更轻量级，可扩展性更强（相较于Tomcat），谷歌应用引擎（GAE）已经全面切换为Jetty，可以这样替换，先排除出web里面默认已经包含了这个Tomcat依赖，然后再加一个jetty
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <!--web起步依赖环境中，排除Tomcat起步依赖-->
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <!--添加Jetty起步依赖，版本由SpringBoot的starter控制-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>

```

```xml
测试类依赖
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-test</artifactId>  
    <scope>test</scope>  
</dependency>
```

```java
  
# 加载Application启动类
@SpringBootTest(classes = Application.class)  
public class Test1 {  
    @Autowired  
    public Usermapper usermapper;  
  
    @Test  
    public void show() {  
  
        List<IDuser> selectid = usermapper.selectid(3);  
        for (IDuser iDuser : selectid) {  
            System.out.println("iDuser = " + iDuser);  
        }  
    }  
}
```

**实例类构建和数据处理**
```xml
<dependency>  
    <groupId>org.projectlombok</groupId>  
    <artifactId>lombok</artifactId>  
    <optional>true</optional>  
</dependency>
```
![[Pasted image 20260919102732.png|693]]


# SpringBoot 加 Mybatis


![[Pasted image 20260919120259.png|962]]
![[Pasted image 20260919120351.png|916]]
![[Pasted image 20260919201116.png|268]]
## ==依赖==
```xml
<parent>  
    <!--起步依赖-->  
    <!--从父亲那里继承过来springboot启动信息-->  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-parent</artifactId>  
    <version>3.3.3</version>  
</parent>  
  
<groupId>com.itgaohe</groupId>  
<artifactId>SpringBoot_Mybatis</artifactId>  
<version>1.0-SNAPSHOT</version>  
  
<properties>  
    <maven.compiler.source>17</maven.compiler.source>  
    <maven.compiler.target>17</maven.compiler.target>  
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
</properties>  
  
<dependencies>  
    <!--整合的web坐标 内置了tomcat服务器 无需再次配置-->  
    <dependency>  
        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter-web</artifactId>  
        <version>3.3.3</version>  
    </dependency> 
       <!--mybatis-spring-->  
    <dependency>  
        <groupId>org.mybatis.spring.boot</groupId>  
        <artifactId>mybatis-spring-boot-starter</artifactId>  
        <version>3.0.3</version>  
    </dependency>    
    <!--mysql--> 
    <dependency>  
        <groupId>com.mysql</groupId>  
        <artifactId>mysql-connector-j</artifactId>  
    </dependency>
        <!--druid-->  
    <dependency>  
        <groupId>com.alibaba</groupId>  
        <artifactId>druid</artifactId>  
        <version>1.1.16</version>  
    </dependency>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter-test</artifactId>  
        <scope>test</scope>  
    </dependency>
    
    <dependency>
        <groupId>org.projectlombok</groupId>  
        <artifactId>lombok</artifactId>  
    </dependency>
    
</dependencies>  
<!--打包插件（方便本地运行jar包的时候） parent里面整合了  但是是可选的插件 需要写出来-->  
<build>  
    <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>  
	            <artifactId>spring-boot-maven-plugin</artifactId>  
	            <version>3.3.3</version>  
	        </plugin>
    </plugins>
</build>
```

## ==application.yml==
```yml
spring:  
  datasource:  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    url: jdbc:mysql://localhost:3306/数据库具体名称?serverTimezone=UTC  
    username: root  
    password: 123456  
    type: com.alibaba.druid.pool.DruidDataSource  
  
mybatis:  
  mapper-locations: classpath:com/itgaohe/mapper/*.xml  
  type-aliases-package: com.itgeohe.pojo  
  configuration:  
    map-underscore-to-camel-case: true  
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

## ==主包里的Usermapper,注意为接口文件，不是类文件==
```java
@Mapper  
public interface Usermapper {  
        List <User> selectAll();  
        List <IDuser> selectid(int id);  
}
```
## ==实例类 接受sql的数据==
```java
@Data  
@AllArgsConstructor  
@NoArgsConstructor
public class User {  
    private Integer id;  
    private String username;  
    private String password;  
    private String gender;  
    private String addr;  
}
```


## ==resources里的映像Usermapper.xml==
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="com.itgaohe.mapper.Usermapper">  
  
    <!-- 查询所有用户 -->  
    <select id = "selectAll" resultType="com.itgaohe.pojo.User">select * from tb_user</select>  
	  <!-- 查询部分用户 -->  
    <select id="selectid" resultType="com.itgaohe.pojo.IDuser">  
        select <include refid="1"></include> from tb_user where id  &lt; #{id}  
    </select>  
	  <!-- sql块 -->  
    <sql id="1">  
        username "姓名" , addr "地址"  
    </sql>  
  
</mapper>
```

## DUSA
对于单个要传图=入的值 可以直接定义 
但是面对多个变量时 用@Param（）起别名 用于区分
\#{ }里面的内容就起到接受传入的值替代的作用 和@Param（）起的别名一致
![[Pasted image 20260920184712.png]]
![[Pasted image 20260920185104.png]]

对于多条件查询 有时候不一定对每个条件都有筛选
可以用if 判断来合理拼接不同情况的sql语句
![[Pasted image 20260920192516.png]]

如果也是面对多条件筛选 但是只能选择一个筛选条件 不能叠加
就用when 类似if ，if else  和 switch case 
![[Pasted image 20260920193712.png]]
面对特殊可以反复用的语句 可以封装为sql段 直接调用
```xml
<select id="selectid" resultType="com.itgaohe.pojo.IDuser">  
    select <include refid="1"></include> from tb_user where id  &lt; #{id}  
</select>
<sql id="1">  
    username "姓名" , addr "地址"  
</sql>
```

添加用简单了 注意接受传入的值就行
![[Pasted image 20260920200602.png]]

  批量删除
  假如数组中的id数据是{1,2,3}，那么拼接后的sql语句就是：
  delete from tb_brand where id in (1,2,3);
![[Pasted image 20260920201541.png]]
```java
/**
  * 批量删除
    参数是一个数组，数组中存储的是多条数据的id
  */
void deleteByIds(@Param("ids")int[] ids);
void deleteByIds(@Param("ids")List<Integer> ids);
```
Mybatis 接口方法中可以接收各种各样的参数，如下：

- 多个参数 **@Param("")**
- 单个参数：单个参数又可以是如下类型
    - POJO 类型
    - Map 集合类型
    - Collection 集合类型
    - List 集合类型
    - Array 类型
    - 其他类型int double ....
==之前讲了@Param（）传入多对象
还可以Map和POJO最常用：==
多个参数属于同一个对象：直接传 POJO
比如新增品牌，参数都是 Brand 的属性：
`int add(Brand brand);`
```java
<insert id="add">
    insert into tb_brand(brand_name, company_name, ordered, description, status)
    values (#{brandName}, #{companyName}, #{ordered}, #{description}, #{status})
</insert>
```
这里 `#{}` 里写的是 **Brand 对象的属性名**。
```java
public class Brand {
    private Integer id;
    private String brandName;
    private String companyName; //(#{brandName}, #{companyName}, #{ordered}, #{description}, #{status})
    private Integer ordered;
    private String description;
    private Integer status;

    public String getBrandName() {
        return brandName;
    }

    public void setBrandName(String brandName) {
        this.brandName = brandName;
    }

    public String getCompanyName() {
        return companyName;
    }

    public void setCompanyName(String companyName) {
        this.companyName = companyName;
    }

    // 其他 getter/setter 省略
}
```
Test调用
```java
Brand brand = new Brand();
brand.setBrandName("张三");
brand.setCompanyName("张三公司");
mapper.add(brand);
```

临时多个参数：用 Map
不想定义 POJO 时可以用 Map：
```java
int add2(Map<String, Object> map);

<insert id="add2">
    insert into tb_brand(brand_name, company_name, ordered, description, status)
    values (#{brandName}, #{companyName}, #{ordered}, #{description}, #{status})
</insert>

//Test
Map<String, Object> map = new HashMap<>();
map.put("brandName", "张三");
map.put("companyName", "张三公司");
map.put("ordered", 12);
map.put("description", "张三222");
map.put("status", 1);

mapper.add2(map);
```
这里 `#{}` 里写的是 **Map 的 key**。

## 多表查询
主要难点是java对数据库返回值的接受如何设计

Mybatis有自动类型转换：
数据库Datatime -->  Java  LocalDatatime

查询结果如果同时包含2张表的数据，一般来说一张表用一个对象接着，这可以用2张表接 在一个类里嵌套另一个类 
比如
```java
@Data  
@AllArgsConstructor  
@NoArgsConstructor
public class Book {  
    private Integer id;  
    private String name;
    ....pass    
}

@Data  
@AllArgsConstructor  
@NoArgsConstructor
public class User {  
    private Integer id;  
    private String username;  
    private String password;  
    private String gender;  
    private String addr;
    private Book book;
    // ||一个主键对应返回多个关联表 private List<Book> book; 
}
```
但是实际返回时另一个表是一堆值 我们定义的是一个类名 不匹配
可以在映像xml里定义嵌套结果集   mapper里面
通过id来查找User对应的Book
![[Pasted image 20260922192937.png|797]]
```xml
<select id="selectUserAndBooks" resultMap="userBook"> 和图上Map的id对应
    SELECT 
        u.id AS uid, u.username, u.password, u.gender, u.addr,
        b.id AS bid, b.name, b.type, b.description
    FROM user u
    LEFT JOIN book b ON u.#{id} = b.uid
</select>
```

### 多表查询进阶补充

> 承接图的笔记：
> `private Book book;` 属于一对一（或多对一）。
> 如果返回的是一堆值，如 `private List<Book> bookList;`，则需要处理**一对多**。

#### 1. 一对多 (One-to-Many)

**场景**：一个用户（User）对应多本书（Book）。
**Java 类设计**：在 `User` 类中定义 `List<Book> bookList;`。

**XML 映射核心变化**：
*   将 `<association>` 替换为 **`<collection>`**。
*   注意：`javaType` 改为 **`ofType`**（指集合中元素的类型，而不是集合本身的类型 `List`）。

```xml
<!-- 一对多嵌套结果集 -->
<resultMap id="userBookList" type="User">
    <id column="uid" property="id"/>
    <result column="username" property="username"/>
    <result column="password" property="password"/>
    <result column="gender" property="gender"/>
    <result column="addr" property="addr"/>
    
    <!-- 注意：这里用 collection，ofType 是泛型里的类型 -->
    <collection property="bookList" ofType="Book">
        <id column="bid" property="id"/>
        <result column="name" property="name"/>
        <result column="type" property="type"/>
        <result column="description" property="description"/>
    </collection>
</resultMap>

<select id="selectUserAndBooks" resultMap="userBookList">
    SELECT 
        u.id AS uid, u.username, u.password, u.gender, u.addr,
        b.id AS bid, b.name, b.type, b.description
    FROM user u
    LEFT JOIN book b ON u.id = b.uid
</select>
```

> [!tip] Obsidian Callout 小细节
> 注意列名冲突问题：`user` 表有 `id`，`book` 表也有 `id`。所以在 SQL 中必须给它们起别名（如 `u.id AS uid`, `b.id AS bid`），并在 `<id>` 或 `<result>` 中与 `column` 属性对应。如果不别名，MyBatis 可能会错误映射（尤其是两表主键名同为 `id` 时）。

#### 2. 多对多 (Many-to-Many)

**场景**：一个学生（Student）选多门课程（Course），一门课有多个学生。必须有中间表（如 `student_course`）。
**Java 类设计**：双方都使用 `List<对方类>`。

*   在 `Student` 类中：`private List<Course> courseList;`
*   在 `Course` 类中：`private List<Student> studentList;`

**XML 映射方式 1：联表查询（不推荐，但了解）**
使用 `<collection>` 标签，通过中间表把三张表连起来。

```xml
<resultMap id="studentCourseMap" type="Student">
    <id column="sid" property="id"/>
    <result column="sname" property="name"/>
    <collection property="courseList" ofType="Course">
        <id column="cid" property="id"/>
        <result column="cname" property="name"/>
    </collection>
</resultMap>

<select id="selectStudentCourses" resultMap="studentCourseMap">
    SELECT 
        s.id AS sid, s.name AS sname,
        c.id AS cid, c.name AS cname
    FROM student s
    LEFT JOIN student_course sc ON s.id = sc.student_id
    LEFT JOIN course c ON sc.course_id = c.id
</select>
```

**XML 映射方式 2：分步查询（推荐，更符合多对多设计）**
通过中间表先查学生，再用 `select` 属性查课程。

```xml
<resultMap id="studentMap" type="Student">
    <id column="id" property="id"/>
    <result column="name" property="name"/>
    <!-- 分步查询：根据学生id查课程 -->
    <collection property="courseList" 
                ofType="Course"
                select="com.xxx.mapper.CourseMapper.selectCoursesByStudentId" 
                column="id" />
</resultMap>
```

#### 3. 多表查询小细节

1. **`javaType` vs `ofType`**：
   * `<association>` 用 `javaType`：比如 `javaType="Book"`。
   * `<collection>` 用 `ofType`：比如 `ofType="Book"`。
   * 如果写错了，MyBatis 启动或运行时会报类型不匹配异常。

2. **主键与映射自动填充（`autoMapping`）**：
   如果表字段名和 Java 属性名符合驼峰命名（如 `user_name` -> `userName`），可以在 `<resultMap>` 上加 `autoMapping="true"`，省略大量 `<result>` 标签：
   ```xml
   <resultMap id="userBookMap" type="User" autoMapping="true">
   ```
   但主键 `<id>`、关联对象嵌套 `<association>` 或 `<collection>` 仍需手动写。

3. **`select *` 的隐患**：
   多表查询尽量**不要用 `select *`**。因为不同表如果有同名字段（如 `create_time`、`id`），返回的 ResultSet 里会出现重复列名，MyBatis 默认映射可能会覆盖，或映射错误。显式列出字段并起别名是最安全的做法。

4. **延迟加载（懒加载）**：
   在多对多或多表分步查询中，开启延迟加载可以优化性能（只有用到关联数据时才发 SQL）：
   在 MyBatis 配置文件中开启：
   ```xml
   <settings>
       <setting name="lazyLoadingEnabled" value="true"/>
       <setting name="aggressiveLazyLoading" value="false"/>
   </settings>
   ```

5. **N+1 问题**：
   使用分步查询（`select` 属性）时，如果查了 100 个用户，又对每个用户发一次 SQL 查其书籍，总共发 101 次 SQL，这就是 N+1 问题。解决办法：用 `JOIN` 联表查询（一条 SQL 搞定），或者开启延迟加载在业务层按需加载。

> [!note] 总结记忆口诀
> *   一对一 / 多对一：`<association>` `javaType`
> *   一对多 / 多对多：`<collection>` `ofType`
> *   字段冲突起别名，`select *` 尽量别。