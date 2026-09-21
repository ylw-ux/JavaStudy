
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
    url: jdbc:mysql://localhost:3306/gaohe2604?serverTimezone=UTC  
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

![[Pasted image 20260920184712.png]]
![[Pasted image 20260920185104.png]]

![[Pasted image 20260920192516.png]]![[Pasted image 20260920193712.png]]
![[Pasted image 20260920200602.png]]![[Pasted image 20260920201541.png]]