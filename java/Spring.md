# Spring

## 一、环境配置

### 1、maven依赖

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.12</version>
        </dependency>
```

### 2、xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
    
    <context:annotation-config/>
    
</beans>
```

## 二，原始开发(不使用注解)

## 三、使用注解开发

### 1.配置

想使用注解开发，必须导入aop依赖

![image-20211027160614590](https://gitee.com/hu-chaoran/typora-upload-images/raw/master//images/image-20211027160614590.png)

xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 指定要扫描的包，这个包下的注解就会生效 -->
    <context:component-scan base-package="com.chaoran.pojo"/>
    <context:annotation-config/>

</beans>
```

### 2.属性注入

```java
@Component
public class User {
    @Value("超然")
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

### 3.常用注解



* @Autowired 自动装配，先通过type装配，若同类型bean大于一个，再通过name装配。

* @Resourse 自动装配，装配顺序同@Autowired

* @component 作用都是将某个类注册到Spring容器当中,等同于xml中

  <bean id="dog" class="com.chaoran.pojo.Dog"/>

### 4.衍生注解

@component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层！

* dao层 (@Repository)

* service(@Service)

* control(@Control)

  这四个注解的作用都是相同的，只为了区分在哪一层，作用都是将某个类注册到Spring容器当中。

## 四、使用Java的方式配置Spring（不用xml）

Spring4之后，这成为核心功能。

```java
@Configuration
public class Config {

    //等同于
    //<bean id="getUser" class="com.chaoran.pojo.User">
    //    <property name="name" value="超然"/>
   // </bean>
    @Bean
    public User getUser(){
        User user=new User();
        user.setName("超然");
        return user;
    }
}
```

测试类：

```java
public class UserTest {
    public static void main(String[] args) {
        ApplicationContext applicationContext=new AnnotationConfigApplicationContext(Config.class);
        User user=applicationContext.getBean("getUser",User.class);
        User user1=applicationContext.getBean("getUser",User.class);
        System.out.println(user==user1);
    }
}
```

## 五、AOP（Aspect Oriented Programming）

### maven配置

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.7</version>
</dependency>
```

### 1.定义和术语

面向切面编程，代理模式思想

切面（aspect）：横切关注点，被模块化的特殊对象，即，他是一个类

通知（Advice）：切面必须完成的工作。即，类中的方法。

目标（Target）：被通知对象。即，被代理对象。

代理（Proxy）：向目标对象应用通知之后创建的对象。

切入点（pointCut）：切面通知执行的“地点”的定义。

连接点（jointPoint）：与切入点匹配的执行点。

### 2.使用方式

#### 方式一：使用API接口

为通过接口实现AOP

AfterReturningAdvice 接口

BeforeAdvice接口

```java
public class AfterLog implements AfterReturningAdvice {
    @Override
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("returnValue为"+returnValue+"; "+target.getClass().getName()+"的"+method.getName());
    }
}
```

再配置xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"        
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    <bean id="userService" class="com.chaoran.serivce.UserServiceImpl"/>
    <bean id="log" class="com.chaoran.Log.Log"/>
    <bean id="afterLog" class="com.chaoran.Log.AfterLog"/>

<!-- 配置AOP-->
    <aop:config>
        <!-- 切入点； expression：表达式 execution(*修饰词*返回值*类名*方法名*参数) -->
        <aop:pointcut id="pointcut" expression="execution(* com.chaoran.serivce.UserServiceImpl.*(..))"/>
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>
```

#### 方法二：自定义切面类

这种方式无法获取方法，目标类名等参数。

```java
public class Aspect {
    public void before(){
        System.out.println("=======使用方法前==========");
    }
    public void after(){
        System.out.println("=======使用方法后==========");
    }
}
```

```xml
<bean id="userService" class="com.chaoran.serivce.UserServiceImpl"/>
<bean id="aspect" class="com.chaoran.Log.Aspect"/>
<aop:config>
    <!--设置切面-->
    <aop:aspect ref="aspect" >
        <!--设置切入点-->
        <aop:pointcut id="pointcut" expression="execution(* com.chaoran.serivce.UserServiceImpl.*(..))"/>
        <aop:after method="after" pointcut-ref="pointcut"/>
        <aop:before method="before" pointcut-ref="pointcut"/>
    </aop:aspect>
</aop:config>
```

#### 方法三：使用注解实现

```java
@Aspect
public class AnnotationAspect {
    private static final String EXECUTION="execution(* com.chaoran.serivce.UserServiceImpl.*(..))";

    @Before(EXECUTION)
    public void before(){
        System.out.println("=======使用方法前==========");
    }
    @After(EXECUTION)
    public void after(){
        System.out.println("=======使用方法后==========");
    }
    @Around(EXECUTION)
    public void around(ProceedingJoinPoint jp) throws Throwable {
        System.out.println("=======环绕后==========");
        Object o=jp.proceed();
        System.out.println("=======环绕前==========");
    }
}
```

```xml
<bean id="userService" class="com.chaoran.serivce.UserServiceImpl"/>
<bean id="aspect" class="com.chaoran.Log.AnnotationAspect"/>
<!--开启注解 -->
<aop:aspectj-autoproxy/>
```

## 六、mybatis-spring

maven配置需额外加入

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-jdbc</artifactId>
  <version>5.3.12</version>
</dependency>
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>2.0.6</version>
</dependency>
```

Spring的xml文件配置

### 方法一：

spring-mybatis.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd 
        http://www.springframework.org/schema/tx
         http://www.springframework.org/schema/tx/spring-tx.xsd 
         http://www.springframework.org/schema/aop 
         https://www.springframework.org/schema/aop/spring-aop.xsd">
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useUnicode=true&amp;
                                        characterEncoding=utf8&amp;serverTimezone=GMT%2B8&amp;useSSL=false
                                        &amp;allowPublicKeyRetrieval=true"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
</bean>
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource" />
    <property name="configLocation" value="classpath:mybatis-config.xml"/>
    <property name="mapperLocations" value="classpath:com/chaoran/Mapper/*.xml"/>
</bean>
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg index="0" ref="sqlSessionFactory" />
</bean>
<!--注入Dao层类-->
<bean id="userMapper" class="com.chaoran.Mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>
</beans>
```

在Spring中，该方法无法实现接口的bean注入，所以要为Mapper接口建立实现类，在bean中注入。

### 方法二

```properties
jdbc.driverClass=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8&useSSL=true
jdbc.username=root
jdbc.password=123456
```

key名去掉jdbc会出错，暂不知道原因。



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:coontext="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context ">

    <coontext:property-placeholder location="classpath:db.properties"/>
    <!--    注入连接池-->
    <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="${jdbc.driverClass}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <bean class="org.mybatis.spring.SqlSessionFactoryBean" id="sessionFactory">
        <property name="dataSource" ref="datasource"/>
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

    <!--    配置Dao接口扫描包，动态实现将Dao接口注入spring-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sessionFactory"/>
        <!--        设置要扫描的包-->
        <property name="basePackage" value="com.chaoran.Dao"/>
    </bean>

</beans>
```

这种方法不用建立Dao接口的实现类，可以直接将其注入spring。

同时，jdbc的数据也是通过properties文件导入。

## 七、声明式事务

```xml
<!--    配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
<!--    配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="detele" propagation="REQUIRED"/>
            <tx:method name="upadte" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
<!--    配置事务切入-->
    <aop:config>
        <aop:pointcut id="txPointcut" expression="execution(* com.chaoran.Mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
    </aop:config>
```

