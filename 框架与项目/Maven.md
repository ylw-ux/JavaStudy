1.  创建项目选择Maven
2. 创建完检查Setting
3. File Encoding---UTF-8设置
4. BuildExc Devlop --- Maven ---（patn  settings  local repository）

# Maven 依赖导入笔记（初学者）

## 一句话理解
在 `pom.xml` 的 `<dependencies>` 中写 **Maven 坐标**，Maven 就会自动下载对应 `jar` 到本地仓库，并加入项目 classpath。

> Java 代码里该 `import` 还要 `import`；Maven 只负责把 jar 准备好。

## 1. Maven 坐标是什么
Maven 用坐标唯一定位一个 jar，类似“快递地址”。

格式：

```text
groupId:artifactId:version
```

| 字段 | 含义 | 示例 |
|---|---|---|
| groupId | 组织/公司/项目组，通常域名倒写 | `org.junit.jupiter` |
| artifactId | 项目/模块名 | `junit-jupiter` |
| version | 版本号 | `5.10.2` |

示例坐标：

```text
org.junit.jupiter:junit-jupiter:5.10.2
```

## 2. pom.xml 中怎么写依赖
`</project>`下

```xml
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId> 组织坐标
        <artifactId>junit-jupiter</artifactId>  
        <version>5.10.2</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

说明：
- `<dependencies>`：可以放多个依赖。
- `<dependency>`：一个具体依赖。
- `<scope>`：可选，表示依赖作用范围。

## 3. Maven 导入流程

1. 在 `pom.xml` 写坐标。
2. Maven 先查本地仓库：`~/.m2/repository`。
3. 本地没有，就去远程仓库/中央仓库下载。
4. 下载后保存到本地仓库。
5. 项目编译、测试、运行时自动加入 classpath。

在 IDEA/Eclipse 中改完 `pom.xml` 后，点击 **Reload / 刷新 Maven**。

## 4. 常用 scope

| scope      | 作用                           |
| ---------- | ---------------------------- |
| `compile`  | 默认，编译、测试、运行都可用               |
| `test`     | 仅测试，如 JUnit                  |
| `provided` | 编译测试可用，运行由容器提供，如 servlet-api |
| `runtime`  | 运行和测试可用，编译不用，如 JDBC 驱动       |
|            |                              |

## 5. 常见问题

- 坐标写错或版本不存在，会下载失败。
- 通常必须写 `version`；除非父 pom 的 `dependencyManagement` 已管理版本。
- 修改 `pom.xml` 后要刷新 Maven。
- 查看依赖树：

```bash
mvn dependency:tree
```

- 强制更新依赖：

```bash
mvn -U clean package
```

- 搜坐标：https://search.maven.org/

## 记忆口诀

```text
组织 + 项目 + 版本 = 唯一 jar
写进 dependencies，Maven 自动下载。
```