#             **Mybatis**

## 一、配置环境

[入门_MyBatis中文网](https://mybatis.net.cn/getting-started.html)

### 1.maven项目配置

pom.xml:

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.7</version>
        </dependency>
	<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.26</version>
        </dependency>
```

mybatis-config.xml:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
</configuration>
```

db.properties

```properties
driverClass=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/ssmbuild?useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8&useSSL=true
username=root
password=123456
```



mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
 <!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="chaoran.Dao.UserMapper">
    <select id="getUserList" resultType="chaoran.pojo.User">
        select id,name,password  from mybatis.user
    </select>
    <insert id="addUser" parameterType="chaoran.pojo.User">
        insert into mybatis.user(id, name, password) VALUES (#{id},#{name},#{password})
    </insert>
    <update id="updateUser" parameterType="chaoran.pojo.User">
        update mybatis.user
            set name = #{name},password=#{password}
        where id=#{id};
    </update>
    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id=#{id}
    </delete>
</mapper>
```

## 二、配置工具类

```java
public class MybatisUtil {
    private static SqlSessionFactory sqlSessionFactory;
    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory= new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```



## 三、配置解析

## 四、log4j配置

log4j.properties配置

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/shun.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```



## 五、多对一查询

### 1、联表查询

```xml
<select id="getStudentList" resultMap="StudentMap">
        select s.id sid,s.name sname, tid,t.name tname
        from mybatis.student s,mybatis.teacher t
        where s.tid=t.id;
    </select>
    <resultMap id="StudentMap" type="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <association property="teacher" javaType="Teacher">
            <result property="id" column="tid"/>
            <result property="name" column="tname"/>
        </association>
    </resultMap>
```

## 六、静态资源导出问题

```xml
<!--在build中配置resoureces，来防止我们资源导出失效的问题-->
<build>
        <resources>
            <resource>
                <directory>src/main/resources/</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```

## 七、笔记

## 1.like使用



```xml
userName like  concat('%',#{userName},'%')
```

