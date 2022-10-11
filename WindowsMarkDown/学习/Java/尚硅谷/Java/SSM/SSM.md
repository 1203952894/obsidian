# 学习 SSM 框架
#B站 #Java #尚硅谷

> Author: Cola🤩
> Time: 2022-10-10      17:17
> To: 记录尚硅谷 SSM框架学习

> 目录结构：
> - 一、MyBatis
> - 二、 Spring
> - 三、SpringMVC
> - 四、SSM整合

##  一、MyBatis

>环境：
>IDE：IDEA 2021.3.3
>JDK：1.8.0_342
>MYSQL：5.7.34
>MyBatis：3.5.7
>Maven:  3.6.0

>随记：😶‍🌫️
>Time: 2022-10-11  08:53
>ToDO:
>1. 整理项目结构
>2. 完成 一、MyBatis 
>3. 整理 IDEA 的相关配置及使用

>目录结构：
>- 1. MyBatis 简介
>- 2. 搭建MyBatis
>- 3. 核心配置文件详解
>- 4. MyBatis的增删改查
>- 5. MyBatis获取参数值的两种方式
>- 6. MyBatis的各种查询功能
>- 7. 特殊SQL的执行
>- 8. 自定义映射resultMap
>- 9. 动态SQL
>- 10. MyBatis 的缓存
>- 11. MyBatis的逆向工程
>- 12. 分页插件

### 1. MyBatis 简介

>目录结构：
>- 1.1 MyBatis 历史
>- 1.2 MyBatis 特性
>- 1.3 MyBatis 下载
>- 1.4 和其它持久层技术对比

#### 1.1 MyBatis 历史

MyBatis最初是Apache的一个开源项目iBatis, 2010年6月这个项目由Apache Software Foundation迁
移到了Google Code。随着开发团队转投Google Code旗下， iBatis3.x正式更名为MyBatis。代码于
2013年11月迁移到Github。
iBatis一词来源于“internet”和“abatis”的组合，是一个基于Java的持久层框架。 iBatis提供的持久层框架
包括SQL Maps和Data Access Objects（DAO）。

#### 1.2 MyBatis 特性

1. MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架
2. MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集
3. MyBatis可以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数据库中的记录
4. MyBatis 是一个 半自动的ORM（Object Relation Mapping）框架

#### 1.3 MyBatis 下载

MyBatis下载地址: [MyBatis官网](https://github.com/mybatis/mybatis-3)
![](assets/Pasted%20image%2020221011121727.png)

![](assets/Pasted%20image%2020221011121736.png)

![](assets/Pasted%20image%2020221011121745.png)
- lib 目录下存放相应依赖 jar 包
- pdf 为 MyBatis 的官方手册

#### 1.4 和其它持久层技术对比

- JDBC
	- SQL 夹杂在 Java 代码中耦合度高，硬编码问题
	- 维护不易且实际开发需求中 SQL 有变化
	- 代码冗长，开发效率低
- Hibernate 和 JPA
	- 操作简便，开发效率高
	- 程序中的长难复杂 SQL 需要绕过框架
	- 内部自动生产的 SQL 不易做特殊优化
	- 基于全映射的全自动框架，大量字段的 POJO 进行部分映射时比较困难
	- 反射操作太多，影响数据库性能
- MyBatis
	- 轻量级。性能出色
	- SQL 和 Java 编码分离，功能边界清晰。Java 代码专注业务，SQL语句专注数据
	- 开发效率不如 Hibernate 但是可以接受

### 2. 搭建MyBatis

>目录结构：
>- 2.1 开发环境
>- 2.2 创建Maven工程
>	- 2.2.1 打包方式 jar
>	- 2.2.2 引入依赖
>- 2.3 创建 MyBatis的核心配置文件
>- 2.4 创建 Mapper 接口
>- 2.5 创建 MyBatis 映射文件
>- 2.6 通过 junit 测试功能
>- 2.7 加入 log4j 日志功能
>	- 2.7.1 引入依赖
>	- 2.7.2 加入 log4j 配置文件

#### 2.1 开发环境

>MYSQL：5.7.34
>MyBatis：3.5.7
>Maven:  3.6.0

-  不同版本 MySQL  注意事项

MySQL8 相较于 MySQL 5需要配置时区，否则会报如下错误：
`java.sql.SQLException: The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more
`
|驱动类 driver-class-name|链接地址 url|
|---|---|
|MySQL 5版本使用 jdbc5驱动，  驱动类为 `com.mysql.jdbc.driver`|`jdbc:mysql://localhost:3306/ssm`|
|MySQL 8 版本使用 jdbc8 驱动，  驱动类为 `com.mysql.cj.jdbc.driver`|`jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC`|

#### 2.2 创建Maven工程
##### 2.2.1 打包方式 jar
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0</modelVersion>  
  
    <groupId>vip.mcdd</groupId>  
    <artifactId>ShangGuiGuSSM</artifactId>  
    <version>1.0-SNAPSHOT</version>  
    <modules>        
	    <module>MyBatisDemo01</module>  
	    <module>MyBatisDemo02</module>  
	    <module>MyBatisDemo03</module>    
    </modules>  
    <properties>        <maven.compiler.source>8</maven.compiler.source>  
        <maven.compiler.target>8</maven.compiler.target>  
    </properties>  
    <packaging>pom</packaging>  

</project>
```
##### 2.2.2 引入依赖
```xml
    <dependencies>        <!-- Mybatis核心 -->  
        <dependency>  
            <groupId>org.mybatis</groupId>  
            <artifactId>mybatis</artifactId>  
            <version>3.5.7</version>  
        </dependency>        <!-- junit测试 -->  
        <dependency>  
            <groupId>junit</groupId>  
            <artifactId>junit</artifactId>  
            <version>4.12</version>  
            <scope>test</scope>  
        </dependency>        <!-- MySQL驱动 -->  
        <dependency>  
            <groupId>mysql</groupId>  
            <artifactId>mysql-connector-java</artifactId>  
            <version>8.0.16</version>  
        </dependency>        <!-- log4j日志 -->  
        <dependency>  
            <groupId>log4j</groupId>  
            <artifactId>log4j</artifactId>  
            <version>1.2.17</version>  
        </dependency>  
    </dependencies>  
```

#### 2.3 创建 MyBatis的核心配置文件
在 `src/main/resources` 目录下创建数据库配置信息文件 `jdbc.properties`
```properties
jdbc.driver=com.mysql.cj.jdbc.Driver  
jdbc.url=jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC  
jdbc.username=root  
jdbc.password=root
```
配置MyBatis的核心配置文件，该文件习惯上命名为 mybatis-config.xml ,这个文件名仅仅时建议，并非强制要求将来整合 Spring之后，这个配置文件可以省略，该文件主要用来配置连接数据库的环境以及MyBatis 的全局配置信息。核心配置文件存放的位置一般为 `src/main/resources`目录下
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <!--引入 properties文件-->  
    <properties resource="jdbc.properties"></properties>  
    <!--类型别名-->  
    <typeAliases>  
        <package name="vip.mcdd.pojo"/>  
    </typeAliases>    <environments default="development">  
        <environment id="development">  
            <transactionManager type="JDBC"/>  
            <dataSource type="POOLED">  
                <property name="driver" value="${jdbc.driver}"/>  
                <property name="url" value="${jdbc.url}"/>  
                <property name="username" value="${jdbc.username}"/>  
                <property name="password" value="${jdbc.password}"/>  
            </dataSource>        </environment>    </environments>    <!--引入MyBatis映射文件-->  
    <mappers>  
        <package name="vip.mcdd.mapper"/>  
    </mappers></configuration>
```
#### 2.4 创建 Mapper 接口

MyBatis 中的 mapper 接口相当于以前的 DAO。区别在于 mapper仅仅是接口，不需要提供实现类。后期我们通过 sqlSession.getMapper() 的方法，获取实现类（代理模式）.
```java
package vip.mcdd.mapper;  
  
import vip.mcdd.pojo.User;  
  
import java.util.List;  
  
public interface UserMapper {  
    /**  
     * 添加用户信息  
     * @return  
     * */  
    int insertUser();  
  
    /**  
     * 修改用户信息  
     * @return  
     */  
    int updateUser();  
  
    /**  
     * 删除用户  
     * @return  
     */  
    int deleteUser();  
  
    /**  
     * 通过id 查找用户  
     */  
    User findUserById();  
  
    /**  
     * 查询所有用户信息  
     * @return  
     */  
    List<User> findAllUser();  
}
```

#### 2.5 创建 MyBatis 映射文件

相关概念：**ORM（Object Relationship Mapping）**对象关系映射。
- 对象：Java的实体类对象
- 关系：关系型数据库
- 映射：二者之间的对应关系
|Java概念|数据库概念|
|---|---|
|类|表|
|属性|字段（列）|
|对象|记录（行）|
- 映射文件的命名规则：
表所对应的实体类的类名+Mapper.xml
例如：表 table_user 映射的实体类为 User，所对应的映射文件为 UserMapper.xml
因此一个映射文件对应一个实体类，对应一张表的操作
MyBatis 映射文件用于编写 SQL，访问以及操作表中的数据，该文件一般存在 `src/main/resources`目录下且与接口包保持层次一致性
> **注意：** IDEA 中 ` resources` 目录没有包的说法，但包的本质上仍为文件夹结构 所以可以采用 `vip/mcdd/mapper` 来实现包层次一致性
- MyBatis 中可以面向接口操作数据但前提要满足两个一致：
	- mapper 接口的全类名和映射文件的命名空间(namespace)保持一致
	- mapper 接口中方法名和映射文件中编写SQL的标签属性id保持一致

- 在  `src/main/resources`  中创建目录 `vip/mcdd/mapper` 
![](assets/Pasted%20image%2020221011215312.png)
- 编写 `UserMapper.xml` 映射文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="vip.mcdd.mapper.UserMapper">  
    <!--int insertUser();-->  
    <insert id="insertUser">  
        insert into table_user values(null,'admin',21)  
    </insert>  
    <!--int updateUser();-->  
    <update id="updateUser">  
        update table_user set username= 'root',age=22 where id=10  
    </update>  
    <!--int deleteUser();-->  
    <delete id="deleteUser">  
        delete from table_user where id=7  
    </delete>  
    <!--User findUserById();  
    resultType: 当数据库属性名与pojo类属性名一致，且为一对一关系时采用 resultType    resultMap: 当数据库属性名与pojo类属性名不一致，且为一对多或多对一时采用 resultMap    -->    <select id="findUserById" resultType="User">  
        select * from table_user where id=1  
    </select>  
  
    <!--List(User) findAllUser();-->  
    <select id="findAllUser" resultType="User">  
        select * from table_user  
    </select>  
  
</mapper>
```
![](assets/Pasted%20image%2020221011215751.png)

```java
package vip.mcdd.pojo;  
  
/**  
 * @ Name: User   
 * @ Author: Cola   
 * @ Time: 2022/10/11 13:08   
 * @ Description: TODO   
 * */  

public class User {  
    private Integer id;  
    private String username;  
    private Integer age;  
  
    public User() {  
  
    }  
  
    public User(Integer id, String username, Integer age) {  
        this.id = id;  
        this.username = username;  
        this.age = age;  
    }  
  
    public Integer getId() {  
        return id;  
    }  
  
    public void setId(Integer id) {  
        this.id = id;  
    }  
  
    public String getUsername() {  
        return username;  
    }  
  
    public void setUsername(String username) {  
        this.username = username;  
    }  
  
    public Integer getAge() {  
        return age;  
    }  
  
    public void setAge(Integer age) {  
        this.age = age;  
    }  
  
    @Override  
    public String toString() {  
        return "User{" +  
                "id=" + id +  
                ", username='" + username + '\'' +  
                ", age=" + age +  
                '}';  
    }  
}
```

- 目录层次
![](assets/Pasted%20image%2020221011215900.png)
由图可以看出 在项目生成文件 `target` 中 `UserMapper` 接口已经和 `UserMapper.xml` 文件保持层次一致性

#### 2.6 通过 junit 测试功能
- 创建 `SqlSessionUtil` 工具类 便于后续获取 sqlSession 对象
```java
package vip.mcdd.utils;  
  
  
import org.apache.ibatis.io.Resources;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.apache.ibatis.session.SqlSessionFactoryBuilder;  
  
import java.io.IOException;  
import java.io.InputStream;  
  
/**  
 * @ Name: SqlSessionUtils * @ Author: Cola * @ Time: 2022/10/11 19:49 * @ Description: TODO */public class SqlSessionUtils {  
  
public static SqlSession getSqlSession(){  
    try {  
        InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");  
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();  
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(stream);  
        SqlSession sqlSession = sqlSessionFactory.openSession(true);  
        return sqlSession;  
    } catch (IOException e) {  
        e.printStackTrace();  
    }  
        return null;  
}  
}
```
在 `tset\java`  下创建 `UserTest` 测试类
```java
package vip.mcdd;  
  
import org.apache.ibatis.session.SqlSession;  
import org.junit.Test;  
import vip.mcdd.mapper.UserMapper;  
import vip.mcdd.pojo.User;  
import vip.mcdd.utils.SqlSessionUtils;  
  
import java.io.IOException;  
  
/**  
 * @ Name: UserTest * @ Author: Cola * @ Time: 2022/10/11 19:56 * @ Description: TODO  
 */  
public class UserTest {  
  
    /**  
     * 测试添加 User 方法  
     * @throws IOException  
     */    @Test  
    public void testInsert() throws IOException {  
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        System.out.println(mapper.insertUser());  
  
        // 关闭资源  
        sqlSession.close();  
  
    }  
  
    /**  
     * 测试修改 User 信息方法  
     * @throws IOException  
     */    @Test  
    public void testUpdate() throws IOException {  
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        System.out.println(mapper.updateUser());  
  
        // 关闭资源  
        sqlSession.close();  
    }  
  
  
    /**  
     * 测试删除 User 方法  
     * @throws IOException  
     */    @Test  
    public void testDelete() throws IOException {  
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        System.out.println(mapper.deleteUser());  
  
        // 关闭资源  
        sqlSession.close();  
    }  
  
    /**  
     * 测试 根据id查询用户信息方法  
     * @throws IOException  
     */    @Test  
    public void testFindUserById() throws IOException {  
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        System.out.println(mapper.findUserById().toString());  
  
        // 关闭资源  
        sqlSession.close();  
    }  
    /**  
     * 测试 查询所有用户信息  
     * @throws IOException  
     */    @Test  
    public void testFindAllUser() throws IOException {  
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        for (User user : mapper.findAllUser()) {  
            System.out.println(user.toString());  
        }  
        // 关闭资源  
        sqlSession.close();  
    }  
  
}
```
测试结果
![](assets/Pasted%20image%2020221011220100.png)
#### 2.7 加入 log4j 日志功能
##### 2.7.1 引入依赖
```xml
<!-- log4j日志 -->
<dependency>
<groupId>log4j</groupId>
<artifactId>log4j</artifactId>
<version>1.2.17</version>
</dependency>

```
##### 2.7.2 加入 log4j 配置文件
log4j 的配置文件名必须为 log4j.xml 存放于 `src/main/resources` 目录下
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">  
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">  
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">  
        <param name="Encoding" value="UTF-8" />  
        <layout class="org.apache.log4j.PatternLayout">  
            <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS}  
%m (%F:%L) \n" />  
        </layout>    </appender>    <logger name="java.sql">  
        <level value="debug" />  
    </logger>    <logger name="org.apache.ibatis">  
        <level value="info" />  
    </logger>    <root>        <level value="debug" />  
        <appender-ref ref="STDOUT" />  
    </root></log4j:configuration>
```
- 日志的级别
> 自上到下打印的内容越来越详细
> 1. FATAL     (致命)
> 2. ERROR （错误）
> 3. WARN  （警告）
> 4. INFO     （信息）😜
> 5. DEBUG （调试）
### 3. 核心配置文件详解

> 核心配置文件中的标签必须按照固定顺序配置：
![](assets/Pasted%20image%2020221011220228.png)

| 元素            | 说明               |
| --------------- | ------------------ |
| `configuration` | 根元素             |
| `properties`    | 用于引入资源文件   |
| `settings`      |                    |
| `typeAliases`   | 用于配置类型的别名 |
| `typeHandlers`  |                    |
| `objectFactory` |                    |
| `plugins`       |         用于配置相关插件           |
| `enviroments`   |           用于配置多个数据库或生产环境连接的配置信息|         |
| `mapper`                |      用于引入映射文件              |
- `configuration` 元素

`mybatis-config.xml` 文件的根元素

-  `properties` 元素

用于引入外部的资源文件例如 `db.properties` 可以采用`${key}`的形式注入相关信息，方便对数据源进行配置

- `settings` 元素


- `typeAliases` 元素

用于设置POJO类的别名有 `typeAlias` 和 `package` 两个子元素
- `typeAlias`  元素
	- `type` ： 用于设置需要设置别名的类型
	- `alias` : 别名 默认情况下为相应的类名且不区分大小分
- `package` 元素
	- `name` : POJO 类所在的包名，该元素可以以包为单位，将该包下所有的类型设置默认别名

- `typeHandlers` 元素

- `objectFactory` 元素

- `plugins` 元素

用于配置相关插件

- `enviroments` 元素
用于配置多个数据库或生产环境连接的配置信息有子元素 `enviroment`通过其属性 id 用于区别不同生成环境，`enviroment` 有两个子元素

- `transactionManager`: 用于设置事务管理的方式 取值为：
	- `JDBC` 表示当前环境使用 JDBC 中原生的事务管理方式，事物的提交和回滚需要手动操作
	- `MANAGED` 被管理，例如：Spring
- `dataSource` ：用于配置数据源 其元素 `property` 用于配置连接信息
	- `type`: 设置数据的类型，取值为：
		- `POOLED`  表示使用数据库连接池缓存数据库连接
		- `UNPOOLED` 表示不适用数据库连接池
		- `JNDI` 表示使用上下文的数据源

`mapper` 元素

用于引入映射文件须保持以下条件：
- mapper 接口所在的包要和映射文件所在的包一致
- mapper 接口要和映射文件的名字一致

### 4. MyBatis的增删改查

>目录结构：
>- 4.1 新增
>- 4.2 删除
>- 4.3 查询一个实体类对象
>- 4.4 查询 list 集合
#### 4.1 新增
- `UserMapper.java` 接口
```java
/**  
 * 添加用户信息  
 * @return  
 * */  
int insertUser();
```
- `UserMapper.xml` 映射文件
```xml
<!--int insertUser();-->  
<insert id="insertUser">  
    insert into table_user values(null,'admin',21)  
</insert>

```
- `UserTest` 测试类

```java
/**  
 * 测试添加 User 方法  
 * @throws IOException  
 */@Test  
public void testInsert() throws IOException {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
    System.out.println(mapper.insertUser());  
  
    // 关闭资源  
    sqlSession.close();  
  
}
```
#### 4.2 删除
- `UserMapper.java` 接口

```java
/**  
 * 删除用户  
 * @return  
 */  
int deleteUser();
```

- `UserMapper.xml` 映射文件

```xml
<!--int deleteUser();-->  
<delete id="deleteUser">  
    delete from table_user where id=7  
</delete>
```

- `UserTest` 测试类

```java
/**  
 * 测试删除 User 方法  
 * @throws IOException  
 */@Test  
public void testDelete() throws IOException {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
    System.out.println(mapper.deleteUser());  
  
    // 关闭资源  
    sqlSession.close();  
}
```

#### 4.3 查询一个实体类对象
- `UserMapper.java` 接口

```java
/**  
 * 通过id 查找用户  
 */  
User findUserById();
```

- `UserMapper.xml` 映射文件

```xml
<!--User findUserById();-->  
<select id="findUserById" resultType="User">  
    select * from table_user where id=1  
</select>
```

- `UserTest` 测试类

```java
/**  
 * 测试 根据id查询用户信息方法  
 * @throws IOException  
 */@Test  
public void testFindUserById() throws IOException {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
    System.out.println(mapper.findUserById().toString());  
  
    // 关闭资源  
    sqlSession.close();  
}
```

#### 4.4 查询 list 集合
- `UserMapper.java` 接口

```java
/**  
 * 查询所有用户信息  
 * @return  
 */  
List<User> findAllUser();
```

- `UserMapper.xml` 映射文件

```xml
<!--List(User) findAllUser();-->  
<select id="findAllUser" resultType="User">  
    select * from table_user  
</select>
```

- `UserTest` 测试类

```java
/**  
 * 测试 查询所有用户信息  
 * @throws IOException  
 */@Test  
public void testFindAllUser() throws IOException {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
    for (User user : mapper.findAllUser()) {  
        System.out.println(user.toString());  
    }  
    // 关闭资源  
    sqlSession.close();  
}
```

> 注意： 查询到标签 `select` 的属性 `resultType` 和 `resultMap` 不能同时设置
>  `resultType` 多用于属性名和字段名一致且一对一的情况
>  `resultMap` 多用于属性名和字段名不一致且为一对多或多对一的情况


### 5. MyBatis获取参数值的两种方式

>目录结构：
>- 5.1 单个字面量类型的参数
>- 5.2 多个字面量类型的参数
>- 5.3 map 集合类型的参数
>- 5.4 实体类类型的参数
>- 5.5 使用 @Param 表示参数

#### 5.1 单个字面量类型的参数

#### 5.2 多个字面量类型的参数

#### 5.3 map 集合类型的参数

#### 5.4 实体类类型的参数

#### 5.5 使用 @Param 表示参数

###  6. MyBatis的各种查询功能


>目录结构：
>- 6.1 查询一个实体类对象
>- 6.2 查询一个 list 集合
>- 6.3 查询单个数据
>- 6.4 查询一条数据为 map 的集合
>- 6.5 查询多条数据为 map 的集合
	- 6.5.1 方式一
>	- 6.5.2 方式二

#### 6.1 查询一个实体类对象

#### 6.2 查询一个 list 集合

#### 6.3 查询单个数据

#### 6.4 查询一条数据为 map 的集合

#### 6.5 查询多条数据为 map 的集合

##### 6.5.1 方式一

##### 6.5.2 方式二

### 7. 特殊SQL的执行

>目录结构：
>- 7.1 模糊查询
>- 7.2 批量删除
>- 7.3 动态设置表名
>- 7.4 添加功能获取自增主键
#### 7.1 模糊查询

#### 7.2 批量删除

#### 7.3 动态设置表名

#### 7.4 添加功能获取自增主键

### 8. 自定义映射resultMap

>目录结构：
>- 8.1 resultMap 处理字段和属性的映射关系
>- 8.2 多对一映射处理
>	- 8.2.1 级联方式处理映射关系
>	- 8.2.2 使用 association 处理映射关系
>	- 8.2.3 分步查询
>		- 8.2.3.1 查询员工信息
>		- 8.2.3.2 根据员工所对应部门 id 查询部门信息
>- 8.3 一对多映射处理
>	- 8.3.1 collection
>	- 8.3.2 分步查询
>		- 8.3.2.1 查询部门信息
>		- 8.3.2.2 根据部门id 查询部门中所有员工
#### 8.1 resultMap 处理字段和属性的映射关系

#### 8.2 多对一映射处理

##### 8.2.1 级联方式处理映射关系

##### 8.2.2 使用 association 处理映射关系

##### 8.2.3 分步查询

###### 8.2.3.1 查询员工信息

###### 8.2.3.2 根据员工所对应部门 id 查询部门信息

#### 8.3 一对多映射处理

##### 8.3.1 collection

##### 8.3.2 分步查询

###### 8.3.2.1 查询部门信息

###### 8.3.2.2 根据部门id 查询部门中所有员工

### 9. 动态SQL

>目录结构：
>- 9.1 if
>- 9.2 where
>- 9.3 trim
>- 9.4 choose、when、otherwise
>- 9.5 foreach
>- 9.6 SQL 片段
#### 9.1 if

#### 9.2 where

#### 9.3 trim

#### 9.4 choose、when、otherwise

#### 9.5 foreach

#### 9.6 SQL 片段

### 10. MyBatis 的缓存

>目录结构：
>- 10.1 MyBatis 的一级缓存
>- 10.2 MyBatis 的二级缓存
>- 10.3 二级缓存的相关配置
>- 10.4 MyBatis 缓存查询的顺序
>- 10.5 整合第三方缓存 EHCache
>	- 10.5.1 添加依赖
>	- 10.5.2 各 jar 包的功能
>	- 10.5.3 创建 EHCache 的配置文件 ehcache.xml
>	- 10.5.4 设置二级缓存日志
>	- 10.5.5 加入 logback 日志
>	- 10.5.6 EHCache 配置文件说明
#### 10.1 MyBatis 的一级缓存

#### 10.2 MyBatis 的二级缓存

#### 10.3 二级缓存的相关配置

#### 10.4 MyBatis 缓存查询的顺序

#### 10.5 整合第三方缓存 EHCache

##### 10.5.1 添加依赖

##### 10.5.2 各 jar 包的功能

##### 10.5.3 创建 EHCache 的配置文件 ehcache.xml

##### 10.5.4 设置二级缓存日志

##### 10.5.5 加入 logback 日志

##### 10.5.6 EHCache 配置文件说明

### 11. MyBatis的逆向工程

>目录结构：
>- 11.1 创建逆向工程的步骤
>	- 11.1.1 添加依赖和插件
>	- 11.1.2 创建 MyBatis 的核心配置文件
>	- 11.1.3 创建逆向工程的配置文件
>	- 11.1.4 执行 MBG 插件的generate 目标
>	- 11.1.5 效果
>- 11.2 QBC 查询
#### 11.1 创建逆向工程的步骤

##### 11.1.1 添加依赖和插件

##### 11.1.2 创建 MyBatis 的核心配置文件

##### 11.1.3 创建逆向工程的配置文件

##### 11.1.4 执行 MBG 插件的generate 目标

##### 11.1.5 效果

#### 11.2 QBC 查询
### 12. 分页插件

>目录结构：
>- 12.1 分页插件的使用步骤
>	- 12.1.1 添加依赖
>	- 12.1.2 配置分页插件
>- 12.2 分页插件的使用
#### 12.1 分页插件的使用步骤

##### 12.1.1 添加依赖

##### 12.1.2 配置分页插件

#### 12.2 分页插件的使用

## 二、 Spring

## 三、SpringMVC

## 四、SSM整合

