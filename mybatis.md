---
title: mybatis
date: 2023-10-14 22:42:34
tags:
  - mysql
  - mybatis
  - java
categories: 后端
cover: https://s2.loli.net/2024/06/09/ZuWeizVmJ3DNOgq.webp
---



<img src="https://mybatis.org/images/mybatis-logo.png">



## 简介

### 什么是 MyBatis？

> MyBatis 是一款优秀的==持久层框架==，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

---

### 准备工作

- 新建`maven`项目

- 在`pom.xml`文件中导入相关依赖,数据库依赖导入需要的就行

```xml
<dependencies>
    <!--mybatis依赖-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.13</version>
    </dependency>
    <!--mysql-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.33</version>
    </dependency>
    <!--sqlserver-->
    <dependency>
        <groupId>com.microsoft.sqlserver</groupId>
        <artifactId>mssql-jdbc</artifactId>
        <version>12.2.0.jre8</version>
    </dependency>
    <!--测试-->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.9.3</version>
    </dependency>
</dependencies>
```

---

## [核心配置](https://mybatis.org/mybatis-3/zh/configuration.html)

### 核心配置文件

> 配置标签顺序(核心配置文件中的==标签顺序必须符合下面顺序==)

<img src="https://gitee.com/clibin/image-bed/raw/master/image-20231013140702442.png" alt="图片没了" style="float: left;">

> 核心配置文件`mybatis-config.xml`示例


```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<!--configuration 核心配置文件-->
<configuration>
    <!--1.加载配置文件，从配置文件中读取数据使用${},见下面的数据源配置-->
    <properties resource="db.properties"/>
    
    <!--2.配置日志-->
    <settings>
        <!--标准日志工厂配置-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
        <!--配置log4j日志-->
        <!--<setting name="logImpl" value="LOG4J"/>-->
    </settings>

    <!--
        3.可以给实体类取别名，之后使用类名不需要写全限定类名
            3.1 typeAlias标签页实现
            3.2 package 扫描包下所有类并起一个默认别名为类名(小写也可以)或
                        如果想自定义别名，需要在类名上面加@Alias注解
    -->
    <typeAliases>
        <!--<typeAlias type="com.clb.pojo.User" alias="User"/>-->
        <package name="com.clb.pojo"/>
    </typeAliases>

    <!--4.配置多环境，default指定使用哪个环境-->
    <!--<environments default="mysql">-->
    <environments default="sqlserver">
        <!--mysql数据源-->
        <environment id="mysql">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${mysql.driver}"/>
                <property name="url" value="${mysql.url}"/>
                <property name="username" value="${mysql.username}"/>
                <property name="password" value="${mysql.password}"/>
            </dataSource>
        </environment>
        <!--sql server数据源-->
        <environment id="sqlserver">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${ms.driver}"/>
                <property name="url" value="${ms.url}"/>
                <property name="username" value="${ms.username}"/>
                <property name="password" value="${ms.password}"/>
            </dataSource>
        </environment>
    </environments>
    
    <!--
    每一个Mapper.xml配置文件都需要在mybatis核心配置文件中注册
        1.mapper标签 使用resource指定mapper配置文件注册(无条件!)
        2.mapper标签 使用class指定mapper类注册
        3.package标签扫描包
        总结:第 2,3中方法都需要满足:
                条件：1.接口名和它的mapper配置文件名相同
                     2.接口文件和它的mapper配置文件在同一个包下
            1.所以建议使用第一种方法，指定配置文件路径(使用配置文件时)
            2.使用注解开发时，使用方法2,3
    -->
    <mappers>
        <mapper resource="com/clb/dao/EmpMapper.xml"/>
        <!--<mapper class="com.clb.dao.EmpMapper"/>-->
        <!--<package name="com.clb.dao"/>-->
    </mappers>

</configuration>
```

> 数据源配置文件示例

```properties
# mysql
mysql.driver=com.mysql.cj.jdbc.Driver
mysql.url=jdbc:mysql://localhost:3306/clb
mysql.username=root
mysql.password=123456
# sql server
ms.driver=com.microsoft.sqlserver.jdbc.SQLServerDriver
#ms.url=jdbc:sqlserver://localhost:1433;trustServerCertificate=true
ms.url=jdbc:sqlserver://192.168.0.88:1433;trustServerCertificate=true
ms.username=sa
ms.password=123456
```

### Mapper.xml映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--把原来的UserDaoImpl类转换成一个xml配置文件-->

<!--namespace=绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.clb.dao.UserMapper">
    <!--结果集映射 type -> 需要将数据库数据映射成user类型 数据库字段名和属性名一致则会自动映射，无需额外配置-->
    <resultMap id="UserMap" type="user">
        <!--colum 数据库中的列 -> property 实体类中的属性-->
        <!--<result column="id" property="id"/>-->
        <!--<result column="name" property="name"/>-->
        <result column="pwd" property="password"/>
    </resultMap>
    
    <!--
        查询语句
            id:         对应namespace中的接口的方法名
            resultType: sql语句执行的返回类型
    -->
    <select id="getUserList" resultType="com.clb.pojo.User">
        select * from mybatis.user
    </select>
    
    
    <!--根据id查询用户,这里#{id}中的id就是方法中的id变量-->
    <select id="selectById" resultType="com.clb.pojo.User" parameterType="int">
        select * from user where id = #{id}
    </select>

    <!--模糊查询，concat用于拼接字符串-->
    <select id="getUserLike" resultType="com.clb.pojo.User">
        select * from user where name like concat('', #{name}, '');
    </select>

    <!--添加一个用户,对象中的属性可以直接取出来，不需要加类名user.-->
    <insert id="addUser" parameterType="com.clb.pojo.User">
        insert into user(id, name, pwd) value (#{id}, #{name}, #{pwd})
    </insert> 
</mapper>
```

---

### MybatisUtils

> 要使用mybatis执行sql语句，要使用`SqlSession`对象,可以直接在测试类中获取

```java
@Test
public void test() throws IOException {
    //加载核心配置文件
    String resource = "mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    //获取SqlSessionFactory对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    //获取SqlSession对象
    try (SqlSession session = sqlSessionFactory.openSession()) {
        //获取Mapper对象，执行xml映射文件中的方法
        EmpMapper mapper = session.getMapper(EmpMapper.class);
        List<Emp> emps = mapper.selectBySalary(5000);
        emps.forEach(System.out::println);
    }
}
```

> 但是每次使用都要写这一串代码，**有点麻烦**，前面获取SqlSession对象的代码是固定代码，将这段固定代码提取到工具类`MybatisUtils`，使用工具类的静态方法获取SqlSession对象

> **MybatisUtils工具类示例**

```java
//sqlSessionFactory --> sqlSession
public class MybatisUtils {
    private MybatisUtils() {}
    //定义一个SqlSessionFactory对象并使用静态初始化块初始化对象
    private static final SqlSessionFactory sqlSessionFactory;
    static {
        try {
            //使用mybatis第一步，获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。SqlSession 提供了在数据库执行 SQL 命令所需的所有方法
    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession();
    }
}
```

```java
@Test
public void testMybatisUtils() {
    //使用工具类直接获取SqlSession对象
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
    List<Emp> emps = mapper.selectBySalary(1000);
    emps.forEach(System.out::println);

    sqlSession.close();
}
```

## 注解开发

> 使用注解直接写sql语句代替xml映射文件

```java
public interface UserMapper {
    @Select("select id, name, pwd password from user")
    List<User> selectAll();
    /*
    * @Param()注解:
    *   1.基本类型或者String类型需要加上
    *   2.引用类型可以不加
    *   3.如果只有一个基本类型的话，可以不加，但是建议加上
    *   4.在注解中的sql语句中{}中的参数就是@Param注解中的属性名
    * */
    @Select("select id, name, pwd password " +
            "from user " +
            "where id = #{id} and name=#{name} and pwd=#{password}")
    User selectById(@Param("id") int i, @Param("name")String n, @Param("password") String p);

    //#{id},#{name},#{password}中的三个属性均来自参数中对象的属性
    @Insert("insert into user(id, name, pwd) VALUE (#{id},#{name},#{password})")
    int addUser(User user);

    @Update("update user set name=#{name},pwd=#{password} where id=#{id}")
    int updateUser(User user);

    @Delete("delete from user where id = #{id}")
    int deleteUserById(@Param("id") int id);
}
```

> 使用`@Result`注解可以定义结果映射，使用`@ResultMap`可以引用xml文件中定义的ResultMap结果映射

```java
@Select("select * from emp where first_name = #{name};")
@Results({
    @Result(column = "first_name", property = "firstName")
})
Emp getByFirstNameEmp(String name);
```

```java
//更推荐使用
@Select("select * from emp where first_name = #{name};")
@ResultMap("EmpMap")
Emp getByFirstNameEmp(String name);
```

> 总结：简单的sql语句使用注解比配置文件来的更方便且更快，但是复杂的sql语句使用注解反而不方便或难以实现，所以==简单的sql使用注解，复杂的使用xml，搭配使用==

---

## [动态 SQL](https://mybatis.org/mybatis-3/zh/dynamic-sql.html)

> ==动态查询==

```xml
<!--where 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。
    而且，若子句的开头为 “AND” 或 “OR”，where 元素也会将它们去除。-->

<!--动态sql if标签 如果参数不为空就加上条件，否则不加-->
<select id="pageQuery" resultType="com.sky.entity.Category">
    select * from category
    <where>
        <if test="name != null and name != ''">
            and name like concat('%',#{name},'%')
        </if>
        <if test="type != null">
            and type = #{type}
        </if>
    </where>
    order by sort asc , create_time desc
</select>
```

> ==动态更新==

```xml
<update id="update" parameterType="Category">
    update category
    <set>
        <if test="type != null">
            type = #{type},
        </if>
        <if test="name != null">
            name = #{name},
        </if>
        <if test="sort != null">
            sort = #{sort},
        </if>
        <if test="status != null">
            status = #{status},
        </if>
        <if test="updateTime != null">
            update_time = #{updateTime},
        </if>
        <if test="updateUser != null">
            update_user = #{updateUser}
        </if>
    </set>
    where id = #{id}
</update>
```

> ==批量插入，传入参数是一个集合类型，删除同理==

```xml
<insert id="insertBatch">
    insert into dish_flavor (dish_id, name, value)values
    <foreach collection="flavors" item="f" separator=",">
        (#{f.dishId},#{f.name},#{f.value})
    </foreach>
</insert>
```

> 更多动态SQL用法见[官网](https://mybatis.org/mybatis-3/zh/dynamic-sql.html)
>
> 接下来，了解进阶框架==mybatis-plus==

